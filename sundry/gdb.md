## 使用 GDB 调试 Nginx
'' sudo gdb -q -tui # -q 安静模式启动 GDB -tui 显示代码界面
进入 GDB 运行 `attach` Nginx 子进程报错如下：
'' Missing separate debuginfos, use: debuginfo-install libgcc-4.8.5-4.el7.x86_64 zlib-1.2.7-17.el7.x86_64
直接安装即可

## 断点
'' b ngx_process_events_and_timers 
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