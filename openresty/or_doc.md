# OR文档精炼

*感谢春哥给我们带来这么好的平台，在这里希望能通读 OR 文档，把自己的理解记录下来，并与时俱进的更新*

## 描述 / Description

`lua-nginx-module` 模块通过标准的 Lua 5.1 解释器，或者 LuaJIT 2.0/2.1 在 Nginx 运行环境中嵌入 Lua，并利用 Nginx 的子请求，允许在 Nginx 的时间模块中集成强大的 Lua 线程（Lua 协程）。

与 Apache 的 `mod_lua` 和 Lighttpd 的 `mod_magnet` 不同的是，只要使用 `lua-nginx-module` 模块为 Lua 提供的 Nginx API 来处理上游服务的请求，诸如 MySQL、PostgreSQL、Memcached、Redis 或者上游的 HTTP Web 服务，网络传输都是 100% 非阻塞的。

至少下面列举的这些 Lua 包，和 Nginx 模块可以与 `ngx_lua` 模块完美结合使用：

