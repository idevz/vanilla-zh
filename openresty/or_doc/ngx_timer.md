# ngx.timer

*语法：* `ok, err = ngx.timer.at(delay, callback, user_arg1, user_arg2, ...)`
*上下文：* `init_worker_by_lua*, set_by_lua*, rewrite_by_lua*, access_by_lua*, content_by_lua*, header_filter_by_lua*, body_filter_by_lua*, log_by_lua*, ngx.timer.*, balancer_by_lua*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*, ssl_session_store_by_lua*`

*使用一个自定义函数以及可选的自定义参数创建一个 Nginx 计时器*

第一个参数 `delay` 以秒为单位指定计时器的延迟时间，支持分秒设置，比如 0.001 在这里表示 1 毫秒延迟。`delay` 同样可以设置为 0 ，此时如果当前句柄正被唤醒则计时器将立即获得执行。（in which case the timer will immediately expire when the current handler yields execution.//TODO yields）

第二个参数 `callback` 可以是任何 Lua 函数，后期延迟时间到了，该函数将被以一个后台 “轻线程” 的形式被调用。这个自定义的回调函数将被 Nginx 核心使用 `premature` 参数、user_arg1、user_arg2 等参数自动调用，参数 `premature` 是一个 boolean 值，表示当前定时器是否过期以后，而 user_arg1、user_arg2 等参数就是调用 `ngx.timer.at` 时所传递的余下参数列表。

当 Nginx 工作进程尝试关闭，比如在 Nginx 由于收到 HUP 信号而触发了 Nginx 配置重载的时候，或者 Nginx 服务正在关闭的时候，将会出现无效的计时器（//TODO Premature timer）。当 Nginx 工作进程尝试关闭，将无法通过调用 `ngx.timer.at` 来创建一个新的非零延迟的计时器，并且此时 `ngx.timer.at` 将返回 `nil` 和 “process exiting” 错误。

这个 API 从 v0.9.3 版本开始发布，并且及时 Nginx 工作进程开始关闭的时候，仍然允许创建零延迟计时器。

When a timer expires, the user Lua code in the timer callback is running in a "light thread" detached completely from the original request creating the timer. So objects with the same lifetime as the request creating them, like cosockets, cannot be shared between the original request and the timer user callback function.