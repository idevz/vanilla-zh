# ngx.timer

*语法：* `ok, err = ngx.timer.at(delay, callback, user_arg1, user_arg2, ...)`
*上下文：* `init_worker_by_lua*, set_by_lua*, rewrite_by_lua*, access_by_lua*, content_by_lua*, header_filter_by_lua*, body_filter_by_lua*, log_by_lua*, ngx.timer.*, balancer_by_lua*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*, ssl_session_store_by_lua*`

*使用一个自定义函数以及可选的自定义参数创建一个 Nginx 计时器*

第一个参数 `delay` 以秒为单位指定计时器的延迟时间，支持分秒设置，比如 0.001 在这里表示 1 毫秒延迟。`delay` 同样可以设置为 0 ，此时如果当前句柄正被唤醒则计时器将立即获得执行。（in which case the timer will immediately expire when the current handler yields execution.//TODO yields）

第二个参数 `callback` 可以是任何 Lua 函数，后期延迟时间到了，该函数将被以一个后台 “轻线程” 的形式被调用。这个自定义的回调函数将被 Nginx 核心使用 `premature` 参数、user_arg1、user_arg2 等参数自动调用，参数 `premature` 是一个 boolean 值，表示当前定时器是否过期以后，而 user_arg1、user_arg2 等参数就是调用 `ngx.timer.at` 时所传递的余下参数列表。
