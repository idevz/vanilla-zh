# ngx.config.subsystem

*语法：* `co = ngx.thread.spawn(func, arg1, arg2, ...)`
*上下文：* `rewrite_by_lua*, access_by_lua*, content_by_lua*, ngx.timer.*, ssl_certificate_by_lua*, ssl_session_fetch_by_lua*`

*使用 Lua 函数 `func` 以及可选的参数 `arg1`，`arg2` 等生成一个新的用户“轻线程”，返回一个 Lua 线程（或者 Lua 协程）对象代表这个 “轻线程”*
“轻线程”仅仅是一种特殊的由 `ngx_lua` 模块来调度的 Lua 协程。
在 `ngx.thread.spawn` 返回之前， `func` 函数将会被使用响应的可选参数进行调用，直到此函数调用返回、或者因为错误而终止或是因为通过使用 Nginx 的 I/O 操作 API 导致请求挂起（如 `tcpsock:receive` 操作）。
在 `ngx.thread.spawn` 返回后，新被创建的“轻线程”将在各种 I/O 事件中保持通常的异步运行。

所有在 `rewrite_by_lua`、`access_by_lua` 和 `content_by_lua` 运行的 Lua 代码块都在一个由 ngx_lua 自动创建的样板“轻线程”中，这些样板“轻线程”通常又叫“入口线程”。

默认情况下，相应的 Nginx 处理程序（例如 `rewrite_by_lua` 处理程序）不会终止直到“入口线程”和所有的用户“轻线程”都终止，一个“轻线程（要么是“入口线程”要么是“用户轻线程”因为调用 `ngx.exit`，`ngx.exec`，`ngx.redirect` 或者 `ngx.req.set_uri(uri, true)`）或者“入口线程”因为报错而终止。
当一个用户“轻线程”因为报错而终止，他将不会像“入口线程”一样终止其他线程的运行。

因为 Nginx 子请求模块的限制，一般不允许中止一个正在运行中的 Nginx 子请求。所以同样禁止中止一个运行中的正在等待一个或多个 Nginx 子请求的“轻线程”。你应该调用 `ngx.thread.wait` 来在结束前等待这些“轻线程”结束。这里有个值得注意的例外是你可以通过使用而且只能使用 `ngx.ERROR(-1),408,444或者499` 状态调用 `ngx.exit` 来中止等待的子请求。

“轻线程”不是使用预先抢占的方式来调度的，换句话说，没有自动执行的时间片，一个“轻线程”将保持在 CPU 运行，直到一个（非阻塞）I/O 操作在一个单线程运行不能被完成。

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