# ngx.config.subsystem

*语法：* `subsystem = ngx.config.subsystem`
*上下文：* `set_by_lua*, rewrite_by_lua*, access_by_lua*, content_by_lua*, header_filter_by_lua*, body_filter_by_lua*, log_by_lua*, ngx.timer.*, init_by_lua*, init_worker_by_lua*`

*这个字符串字段表示了当前基于哪个 Nginx 子系统，对当前模块（ngx_stream_lua_module），这个字段始终返回 “http”，而对 ngx_stream_lua_module 模块，这个字段将返回 “stream”*

这个字段在 v0.10.1 版本第一次释出。

