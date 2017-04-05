## 使用 GDB 调试 Nginx
'' sudo gdb -q -tui # -q 安静模式启动 GDB -tui 显示代码界面
进入 GDB 运行 `attach` Nginx 子进程报错如下：
'' Missing separate debuginfos, use: debuginfo-install libgcc-4.8.5-4.el7.x86_64 zlib-1.2.7-17.el7.x86_64
直接安装即可

## 断点
'' b ngx_process_events_and_timers 
子进程即 worker 进程在运行后会停留在`epoll_wait` 处等待相应的事件发生，而这个函数调用被封装在 `ngx_process_events_and_timers` 中