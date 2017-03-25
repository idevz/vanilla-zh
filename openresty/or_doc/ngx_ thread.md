# ngx.config.subsystem

*语法：* `co = ngx.thread.spawn(func, arg1, arg2, ...)`
*上下文：* `rewrite_by_lua*, access_by_lua*, content_by_lua*, ngx.timer.*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*`

*使用 Lua 函数 `func` 以及可选的参数 `arg1`，`arg2` 等生成一个新的用户“轻线程”，返回一个 Lua 线程（或者 Lua 协程）对象代表这个 “轻线程”*
“轻线程”仅仅是一种特殊的由 `ngx_lua` 模块来调度的 Lua 协程。

这个字段在 v0.10.1 版本第一次释出。

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