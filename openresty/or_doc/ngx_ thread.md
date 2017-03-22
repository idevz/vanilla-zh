# ngx.config.subsystem

*语法：* `co = ngx.thread.spawn(func, arg1, arg2, ...)`
*上下文：* `rewrite_by_lua*, access_by_lua*, content_by_lua*, ngx.timer.*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*`

*这个字符串字段表示了当前基于哪个 Nginx 子系统，对当前模块（ngx_stream_lua_module），这个字段始终返回 “http”，而对 ngx_stream_lua_module 模块，这个字段将返回 “stream”*

这个字段在 v0.10.1 版本第一次释出。


syntax: co = ngx.thread.spawn(func, arg1, arg2, ...)

context: rewrite_by_lua*, access_by_lua*, content_by_lua*, ngx.timer.*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*

Spawns a new user "light thread" with the Lua function func as well as those optional arguments arg1, arg2, and etc. Returns a Lua thread (or Lua coroutine) object represents this "light thread".

"Light threads" are just a special kind of Lua coroutines that are scheduled by the ngx_lua module.

Before ngx.thread.spawn returns, the func will be called with those optional arguments until it returns, aborts with an error, or gets yielded due to I/O operations via the Nginx API for Lua (like tcpsock:receive).

After ngx.thread.spawn returns, the newly-created "light thread" will keep running asynchronously usually at various I/O events.

All the Lua code chunks running by rewrite_by_lua, access_by_lua, and content_by_lua are in a boilerplate "light thread" created automatically by ngx_lua. Such boilerplate "light thread" are also called "entry threads".

By default, the corresponding Nginx handler (e.g., rewrite_by_lua handler) will not terminate until

both the "entry thread" and all the user "light threads" terminates,
a "light thread" (either the "entry thread" or a user "light thread" aborts by calling ngx.exit, ngx.exec, ngx.redirect, or ngx.req.set_uri(uri, true), or
the "entry thread" terminates with a Lua error.
When the user "light thread" terminates with a Lua error, however, it will not abort other running "light threads" like the "entry thread" does.

Due to the limitation in the Nginx subrequest model, it is not allowed to abort a running Nginx subrequest in general. So it is also prohibited to abort a running "light thread" that is pending on one ore more Nginx subrequests. You must call ngx.thread.wait to wait for those "light thread" to terminate before quitting the "world". A notable exception here is that you can abort pending subrequests by calling ngx.exit with and only with the status code ngx.ERROR (-1), 408, 444, or 499.

The "light threads" are not scheduled in a pre-emptive way. In other words, no time-slicing is performed automatically. A "light thread" will keep running exclusively on the CPU until

a (nonblocking) I/O operation cannot be completed in a single run,
it calls coroutine.yield to actively give up execution, or
it is aborted by a Lua error or an invocation of ngx.exit, ngx.exec, ngx.redirect, or ngx.req.set_uri(uri, true).
For the first two cases, the "light thread" will usually be resumed later by the ngx_lua scheduler unless a "stop-the-world" event happens.

User "light threads" can create "light threads" themselves. And normal user coroutines created by coroutine.create can also create "light threads". The coroutine (be it a normal Lua coroutine or a "light thread") that directly spawns the "light thread" is called the "parent coroutine" for the "light thread" newly spawned.

The "parent coroutine" can call ngx.thread.wait to wait on the termination of its child "light thread".

You can call coroutine.status() and coroutine.yield() on the "light thread" coroutines.

The status of the "light thread" coroutine can be "zombie" if

the current "light thread" already terminates (either successfully or with an error),
its parent coroutine is still alive, and
its parent coroutine is not waiting on it with ngx.thread.wait.
The following example demonstrates the use of coroutine.yield() in the "light thread" coroutines to do manual time-slicing: