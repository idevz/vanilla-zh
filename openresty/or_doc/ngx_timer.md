# ngx.timer

*语法：* `ok, err = ngx.timer.at(delay, callback, user_arg1, user_arg2, ...)`
*上下文：* `init_worker_by_lua*, set_by_lua*, rewrite_by_lua*, access_by_lua*, content_by_lua*, header_filter_by_lua*, body_filter_by_lua*, log_by_lua*, ngx.timer.*, balancer_by_lua*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*, ssl_session_store_by_lua*`

*使用一个自定义函数以及可选的自定义参数创建一个 Nginx 计时器*

第一个参数 `delay` 以秒为单位指定计时器的延迟时间，支持分秒设置，比如 0.001 在这里表示 1 毫秒延迟。`delay` 同样可以设置为 0 ，此时如果当前句柄正被唤醒则计时器将立即获得执行。（in which case the timer will immediately expire when the current handler yields execution.//TODO yields）

第二个参数 `callback` 可以是任何 Lua 函数，后期延迟时间到了，该函数将被以一个后台 “轻线程” 的形式被调用。这个自定义的回调函数将被 Nginx 核心使用 `premature` 参数、user_arg1、user_arg2 等参数自动调用，参数 `premature` 是一个 boolean 值，表示当前定时器是否过期以后，而 user_arg1、user_arg2 等参数就是调用 `ngx.timer.at` 时所传递的余下参数列表。

当 Nginx 工作进程尝试关闭，比如在 Nginx 由于收到 HUP 信号而触发了 Nginx 配置重载的时候，或者 Nginx 服务正在关闭的时候，将会出现无效的计时器（//TODO Premature timer）。当 Nginx 工作进程尝试关闭，将无法通过调用 `ngx.timer.at` 来创建一个新的非零延迟的计时器，并且此时 `ngx.timer.at` 将返回 `nil` 和 “process exiting” 错误。

这个 API 从 v0.9.3 版本开始发布，并且即使 Nginx 工作进程开始关闭的时候，仍然允许创建零延迟计时器。

当一个计时器运行时，计时器中用户定义回调的 Lua 代码将在一个与创建这个计时器的源请求完全隔离的 “轻线程” 中运行，所以，源请求生命周期内的对象，比如 `cosockets` 并不能与回调函数共享。

下面来看一个简单的例子：

```lua
location / {
 ...
 log_by_lua_block {
     local function push_data(premature, uri, args, status)
         -- push the data uri, args, and status to the remote
         -- via ngx.socket.tcp or ngx.socket.udp
         -- (one may want to buffer the data in Lua a bit to
         -- save I/O operations)
     end
     local ok, err = ngx.timer.at(0, push_data,
                                  ngx.var.uri, ngx.var.args, ngx.header.status)
     if not ok then
         ngx.log(ngx.ERR, "failed to create timer: ", err)
         return
     end
 }
}
```

还可以创建一个无限执行的计时器，例如，一个每 5 秒触发执行一次的计时器，在它的回调方法中递归的调用 `ngx.timer.at` ，这里给出这样的一个例子。

```lua
local delay = 5
local handler
handler = function (premature)
 -- do some routine job in Lua just like a cron job
 if premature then
     return
 end
 local ok, err = ngx.timer.at(delay, handler)
 if not ok then
     ngx.log(ngx.ERR, "failed to create the timer: ", err)
     return
 end
end

local ok, err = ngx.timer.at(delay, handler)
if not ok then
 ngx.log(ngx.ERR, "failed to create the timer: ", err)
 return
end
```

因为定时器的回调函数都是运行在后端，而且他们的运行时间不会叠加到客户端请求的相应时间中，它们可能会因为 Lua 语法错误，或者过多的客户端请求而很容易在服务端造成累积，或者耗尽系统资源

Because timer callbacks run in the background and their running time will not add to any client request's response time, they can easily accumulate in the server and exhaust system resources due to either Lua programming mistakes or just too much client traffic. To prevent extreme consequences like crashing the Nginx server, there are built-in limitations on both the number of "pending timers" and the number of "running timers" in an Nginx worker process. The "pending timers" here mean timers that have not yet been expired and "running timers" are those whose user callbacks are currently running.