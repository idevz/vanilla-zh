# coroutine.create

*语法：* `co = coroutine.create(f)`
*上下文：* `rewrite_by_lua*, access_by_lua*, content_by_lua*, init_by_lua*, ngx.timer.*, header_filter_by_lua*, body_filter_by_lua*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*, ssl_session_store_by_lua*`

*使用 Lua 函数创建一个用户态 Lua 协程， 并返回一个协程对象。与标准 Lua 中的协程创建的 API `coroutine.create` 类似，但是工作在 ngx_lua 模块创建的 Lua 协程上下文中，这个 API 第一次是被使用在 0.9.2 版本的 `init_by_lua*` 上下文中。*

这个 API 在 v0.6.0 版本第一次释出。