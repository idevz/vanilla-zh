## 使用 GDB 调试 Nginx
udo gdb -q -tui # -q 安静模式启动 GDB -tui 显示代码界面
进入 GDB 运行 `attach` Nginx 子进程报错如下：
issing separate debuginfos, use: debuginfo-install libgcc-4.8.5-4.el7.x86_64 zlib-1.2.7-17.el7.x86_64
直接安装即可

## 断点
 ngx_process_events_and_timers 
子进程即 worker 进程在运行后会停留在`epoll_wait` 处等待相应的事件发生，而这个函数调用被封装在 `ngx_process_events_and_timers` 中

```gdb
1       breakpoint     keep y   0x000000000046c8f0 in ngx_process_events_and_timers at src/event/ngx_event.c:195
        breakpoint already hit 39 times
2       breakpoint     keep y   0x000000000046c93e in ngx_process_events_and_timers at src/event/ngx_event.c:242
        breakpoint already hit 36 times
3       breakpoint     keep y   0x000000000046c95a in ngx_process_events_and_timers at src/event/ngx_event.c:249
        breakpoint already hit 35 times
4       breakpoint     keep y   0x000000000046ce7d in ngx_event_process_posted at src/event/ngx_event_posted.c:33
5       breakpoint     keep y   0x0000000000475c24 in ngx_epoll_process_events at src/event/modules/ngx_epoll_module.c:793
        breakpoint already hit 25 times
6       breakpoint     keep y   0x0000000000475d28 in ngx_epoll_process_events at src/event/modules/ngx_epoll_module.c:926
7       breakpoint     keep y   0x0000000000476022 in ngx_epoll_process_events at src/event/modules/ngx_epoll_module.c:900
        breakpoint already hit 28 times
8       breakpoint     keep y   0x000000000048cd4a in ngx_http_wait_request_handler at src/http/ngx_http_request.c:497
        breakpoint already hit 6 times
9       breakpoint     keep y   0x000000000048cd6a in ngx_http_wait_request_handler at src/http/ngx_http_request.c:504
        breakpoint already hit 7 times
10      breakpoint     keep y   0x000000000048c900 in ngx_http_process_request_line at src/http/ngx_http_request.c:945
        breakpoint already hit 6 times
11      breakpoint     keep y   0x000000000048c912 in ngx_http_process_request_line at src/http/ngx_http_request.c:952
        breakpoint already hit 6 times
12      breakpoint     keep y   0x000000000048ca95 in ngx_http_process_request_line at src/http/ngx_http_request.c:1011
13      breakpoint     keep y   0x000000000048cbce in ngx_http_process_request_line at src/http/ngx_http_request.c:1027
        breakpoint already hit 6 times
14      breakpoint     keep y   0x000000000048c5ef in ngx_http_process_request_headers at src/http/ngx_http_request.c:1322
        breakpoint already hit 7 times
15      breakpoint     keep y   0x000000000048c7ed in ngx_http_process_request_headers at src/http/ngx_http_request.c:1342
        breakpoint already hit 2 times
16      breakpoint     keep y   0x000000000048c826 in ngx_http_process_request_headers at src/http/ngx_http_request.c:1348
        breakpoint already hit 2 times
17      breakpoint     keep y   0x000000000048c231 in ngx_http_process_request at src/http/ngx_http_request.c:1916
        breakpoint already hit 2 times
18      breakpoint     keep y   0x0000000000480757 in ngx_http_handler at src/http/ngx_http_core_module.c:839
        breakpoint already hit 2 times
19      breakpoint     keep y   0x0000000000484ced in ngx_http_core_rewrite_phase at src/http/ngx_http_core_module.c:910
20      breakpoint     keep y   0x0000000000485e60 in ngx_http_core_content_phase at src/http/ngx_http_core_module.c:1372
        breakpoint already hit 1 time
21      breakpoint     keep y   0x0000000000485eac in ngx_http_core_content_phase at src/http/ngx_http_core_module.c:1386
22      breakpoint     keep y   0x0000000000485e7d in ngx_http_core_content_phase at src/http/ngx_http_core_module.c:1379
        breakpoint already hit 1 time
23      breakpoint     keep y   0x0000000000504c19 in ngx_http_lua_content_handler at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:222
```

## Lua 请求处理
### 堆栈

```gdb
0  __libc_writev (fd=3, vector=0x7ffe08245dd0, count=14) at ../sysdeps/unix/sysv/linux/writev.c:68
1  0x00000000004717e9 in ngx_writev (c=c@entry=0x7ff03d02f730, vec=vec@entry=0x7ffe08245db0) at src/os/unix/ngx_writev_chain.c:189
2  0x0000000000476a7e in ngx_linux_sendfile_chain (c=0x7ff03d02f730, in=0x7ff03d001418, limit=2147479551) at src/os/unix/ngx_linux_sendfile_chain.c:215
3  0x00000000004a7d15 in ngx_http_write_filter (r=0x7ff03cfffcf0, in=0x7ff03d001b78) at src/http/ngx_http_write_filter_module.c:254
4  0x00000000004a909d in ngx_http_chunked_body_filter (r=0x7ff03cfffcf0, in=<optimized out>) at src/http/modules/ngx_http_chunked_filter_module.c:224
5  0x00000000004ac7dc in ngx_http_gzip_body_filter (r=0x7ff03cfffcf0, in=0x7ffe082466d0) at src/http/modules/ngx_http_gzip_filter_module.c:326
6  0x00000000004afd95 in ngx_http_ssi_body_filter (r=0x7ff03cfffcf0, in=<optimized out>) at src/http/modules/ngx_http_ssi_filter_module.c:411
7  0x00000000004b2cf0 in ngx_http_charset_body_filter (r=0x7ff03cfffcf0, in=0x7ffe082466d0) at src/http/modules/ngx_http_charset_filter_module.c:647
8  0x0000000000506c7c in ngx_http_lua_capture_body_filter (r=0x7ff03cfffcf0, in=0x7ffe082466d0) at ../ngx_lua-0.10.6/src/ngx_http_lua_capturefilter.c:133
9  0x0000000000453395 in ngx_output_chain (ctx=ctx@entry=0x7ff03d001428, in=in@entry=0x7ffe082466d0) at src/core/ngx_output_chain.c:74
10 0x00000000004b4f95 in ngx_http_copy_filter (r=0x7ff03cfffcf0, in=0x7ffe082466d0) at src/http/ngx_http_copy_filter_module.c:152
11 0x0000000000485a37 in ngx_http_output_filter (r=r@entry=0x7ff03cfffcf0, in=in@entry=0x7ffe082466d0) at src/http/ngx_http_core_module.c:1979
12 0x0000000000489e33 in ngx_http_send_special (r=r@entry=0x7ff03cfffcf0, flags=flags@entry=1) at src/http/ngx_http_request.c:3358
13 0x00000000004ffc38 in ngx_http_lua_send_special (flags=1, r=0x7ff03cfffcf0) at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:569
14 ngx_http_lua_send_chain_link (r=0x7ff03cfffcf0, ctx=<optimized out>, in=0x0) at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:523
15 0x0000000000501f5f in ngx_http_lua_run_thread (L=L@entry=0x40030378, r=r@entry=0x7ff03cfffcf0, ctx=ctx@entry=0x7ff03d000dc8, nrets=nrets@entry=0) at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:1476
16 0x00000000005050f0 in ngx_http_lua_content_by_chunk (L=L@entry=0x40030378, r=r@entry=0x7ff03cfffcf0) at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:120
17 0x000000000050548d in ngx_http_lua_content_handler_file (r=0x7ff03cfffcf0) at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:284
18 0x0000000000504c21 in ngx_http_lua_content_handler (r=0x7ff03cfffcf0) at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:222
19 0x0000000000485e7f in ngx_http_core_content_phase (r=0x7ff03cfffcf0, ph=<optimized out>) at src/http/ngx_http_core_module.c:1379
20 0x0000000000480675 in ngx_http_core_run_phases (r=r@entry=0x7ff03cfffcf0) at src/http/ngx_http_core_module.c:856
21 0x000000000048075c in ngx_http_handler (r=r@entry=0x7ff03cfffcf0) at src/http/ngx_http_core_module.c:839
22 0x000000000048c249 in ngx_http_process_request (r=0x7ff03cfffcf0) at src/http/ngx_http_request.c:1916
23 0x000000000048cbf6 in ngx_http_process_request_line (rev=0x7ff03d069520) at src/http/ngx_http_request.c:1027
24 0x0000000000476029 in ngx_epoll_process_events (cycle=0x7ff03cffbce0, timer=<optimized out>, flags=<optimized out>) at src/event/modules/ngx_epoll_module.c:900
25 0x000000000046c947 in ngx_process_events_and_timers (cycle=cycle@entry=0x7ff03cffbce0) at src/event/ngx_event.c:242
26 0x0000000000473d35 in ngx_worker_process_cycle (cycle=cycle@entry=0x7ff03cffbce0, data=data@entry=0x0) at src/os/unix/ngx_process_cycle.c:753
27 0x0000000000472820 in ngx_spawn_process (cycle=cycle@entry=0x7ff03cffbce0, proc=0x473cf0 <ngx_worker_process_cycle>, data=0x0, name=0x6e22f5 "worker process", respawn=respawn@entry=0)
   at src/os/unix/ngx_process.c:198
28 0x0000000000475216 in ngx_reap_children (cycle=0x7ff03cffbce0) at src/os/unix/ngx_process_cycle.c:621
29 ngx_master_process_cycle (cycle=cycle@entry=0x7ff03cffbce0) at src/os/unix/ngx_process_cycle.c:174
30 0x000000000044e139 in main (argc=<optimized out>, argv=<optimized out>) at src/core/nginx.c:367
```

### 断点

```gdb
1       breakpoint     keep y   0x000000000048c420 in ngx_http_process_request_headers at src/http/ngx_http_request.c:1185
        breakpoint already hit 13 times
2       breakpoint     keep y   0x0000000000504ba0 in ngx_http_lua_content_handler at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:154
        breakpoint already hit 13 times
3       breakpoint     keep y   0x0000000000504c98 in ngx_http_lua_content_handler at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:189
4       breakpoint     keep y   0x0000000000504c19 in ngx_http_lua_content_handler at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:222
        breakpoint already hit 8 times
5       breakpoint     keep y   0x0000000000505482 in ngx_http_lua_content_handler_file at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:284
        breakpoint already hit 8 times
6       breakpoint     keep y   0x0000000000505024 in ngx_http_lua_content_by_chunk at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:54
        breakpoint already hit 8 times
7       breakpoint     keep y   0x00000000005050e0 in ngx_http_lua_content_by_chunk at ../ngx_lua-0.10.6/src/ngx_http_lua_contentby.c:120
        breakpoint already hit 8 times
8       breakpoint     keep y   0x00000000005014d3 in ngx_http_lua_run_thread at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:1005
        breakpoint already hit 22 times
9       breakpoint     keep y   0x0000000000501f58 in ngx_http_lua_run_thread at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:1476
        breakpoint already hit 8 times
10      breakpoint     keep y   0x00000000004ffc4a in ngx_http_lua_send_chain_link at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:484
11      breakpoint     keep y   0x00000000004ffb63 in ngx_http_lua_send_chain_link at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:557
        breakpoint already hit 7 times
12      breakpoint     keep y   0x00000000004ffb86 in ngx_http_lua_send_chain_link at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:523
        breakpoint already hit 6 times
13      breakpoint     keep y   0x00000000004ffc2b in ngx_http_lua_send_chain_link at ../ngx_lua-0.10.6/src/ngx_http_lua_util.c:569
        breakpoint already hit 6 times
14      breakpoint     keep y   0x0000000000489e1b in ngx_http_send_special at src/http/ngx_http_request.c:3358
        breakpoint already hit 6 times
15      breakpoint     keep y   0x0000000000485a2b in ngx_http_output_filter at src/http/ngx_http_core_module.c:1979
        breakpoint already hit 30 times
16      breakpoint     keep y   0x0000000000506c70 in ngx_http_lua_capture_body_filter at ../ngx_lua-0.10.6/src/ngx_http_lua_capturefilter.c:133
        breakpoint already hit 30 times
17      breakpoint     keep y   0x00000000004b2ce1 in ngx_http_charset_body_filter at src/http/modules/ngx_http_charset_filter_module.c:647
        breakpoint already hit 30 times
18      breakpoint     keep y   0x00000000004afd89 in ngx_http_ssi_body_filter at src/http/modules/ngx_http_ssi_filter_module.c:411
        breakpoint already hit 25 times
19      breakpoint     keep y   0x00000000004ad41d in ngx_http_postpone_filter at src/http/ngx_http_postpone_filter_module.c:82
        breakpoint already hit 25 times
20      breakpoint     keep y   0x00000000004ac7d0 in ngx_http_gzip_body_filter at src/http/modules/ngx_http_gzip_filter_module.c:326
        breakpoint already hit 25 times
21      breakpoint     keep y   0x00000000004a908f in ngx_http_chunked_body_filter at src/http/modules/ngx_http_chunked_filter_module.c:224
        breakpoint already hit 25 times
22      breakpoint     keep y   0x00000000004a7cff in ngx_http_write_filter at src/http/ngx_http_write_filter_module.c:254
        breakpoint already hit 5 times
23      breakpoint     keep y   0x00000000004769e1 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:92
        breakpoint already hit 4 times
24      breakpoint     keep y   0x0000000000476957 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:49
        breakpoint already hit 4 times
25      breakpoint     keep y   0x00000000004b4f8a in ngx_http_copy_filter at src/http/ngx_http_copy_filter_module.c:152
        breakpoint already hit 5 times
26      breakpoint     keep y   0x000000000045338b in ngx_output_chain at src/core/ngx_output_chain.c:74
        breakpoint already hit 5 times
27      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
28      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
29      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:21
30      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
31      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
32      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
33      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
34      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
35      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
36      breakpoint     keep y   0x0000000000476a71 in ngx_linux_sendfile_chain at src/os/unix/ngx_linux_sendfile_chain.c:215
        breakpoint already hit 1 time
```

## 使用 GDB 调试 OpenResty

### 加断点

比如 `ngx_http_lua_ffi_worker_pid`

```lua
local ffi = require 'ffi'
local C = ffi.C


ffi.cdef[[
int ngx_http_lua_ffi_worker_pid(void);
]]

local _M = { _VERSION = '0.10' }
local mt = { __index = _M }

function _M.new(self)
    local _m_name = 'weibo.com'
    return setmetatable({ m_name = _m_name }, mt)
end

function _M.getWorkerPid(self)
    return C.ngx_http_lua_ffi_worker_pid()
end

return _M
```
运行调用这个包的代码，则可以在 GDB 中加断点 `ngx_http_lua_ffi_worker_pid` 即可直接追踪 OpenResty 的大致运行过程。通过 `c` 继续下一步

#### breakpoints
```
ngx_http_process_request_headers
ngx_http_lua_content_handler
ngx_http_lua_content_handler_file
ngx_http_lua_content_by_chunk
ngx_http_lua_run_thread
ngx_http_lua_send_chain_link
ngx_http_send_special
ngx_http_output_filter
ngx_http_lua_capture_body_filter
ngx_http_charset_body_filter
ngx_http_ssi_body_filter
ngx_http_postpone_filter
ngx_http_gzip_body_filter
ngx_http_chunked_body_filter
ngx_http_write_filter
ngx_linux_sendfile_chain
ngx_http_copy_filter
ngx_output_chain
ngx_linux_sendfile_chain
```
