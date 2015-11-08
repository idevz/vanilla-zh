###Openresty是什么？
重申一下Openresty是什么？截一张Openresty官网，春哥对Openresty的一个官方说明如下图：
![Openresty官方说明](imgs/openresty-cn.png)

我的理解Openresty = Nginx + ngx_http_lua_module + lua_resty_*；它是一个原生Nginx合上一个HTTP_LUA模块，在加上一系列Lua_resty模块组成的一个Ngx_Lua高性能服务生态。

###Openresty处理HTTP请求的执行阶段
Openrestry处理HTTP请求的执行阶段来自于Nginx，Nginx的HTTP框架依据常见的处理流程将处理阶段划分为11个阶段，其中每个处理阶段可以由任意多个HTTP模块流水式地处理请求，Openresty通过ngx_http_lua_module将Lua特性嵌入Nginx，ngx_http_lua_module属于一个Nginx的HTTP模块，为高性能服务开发封装了7个相应HTTP请求处理阶段如下：
- *set_by_lua*
- *content_by_lua*
- *rewrite_by_lua*
- *access_by_lua*
- *header_filter_by_lua*
- *body_filter_by_lua*
- *log_by_lua*

有同学会发现，Openresty所提供的LUA解析指令还有以下两个：
- *init_by_lua*
- *init_worker_by_lua*
```init_by_lua```作用在配置加载阶段，```init_worker_by_lua```作用在worker进程初始化阶段，并非HTTP请求处理阶段。

另外，需要重点提一下的是，Nginx输出过滤器是流式处理模型，一个数据块body filter就被调用一次。可以用以下简单的例子测试验证：
*nginx.conf*
```
http {
    # use sendfile
    sendfile on;

    # Va initialization
    lua_package_path "...;;";
    lua_package_cpath "...;";
    lua_code_cache off;

    init_by_lua_block {ngx.log(ngx.ERR, '=======>init')}
    init_worker_by_lua_block {ngx.log(ngx.ERR, '=======>init_worker')}
    rewrite_by_lua_block {ngx.log(ngx.ERR, '=======>rewrite')}
    access_by_lua_block {ngx.log(ngx.ERR, '=======>access')}
    header_filter_by_lua_block {ngx.log(ngx.ERR, '=======>header_filter')}
    body_filter_by_lua_block {ngx.log(ngx.ERR, '=======>body_filter')}
    log_by_lua_block {ngx.log(ngx.ERR, '=======>log')}
    

    server {
        # List port
        listen 7200;
        set $template_root '';
        set_by_lua_block $a {ngx.log(ngx.ERR, '=======>set'); return 'xxx'}

        # Access log with buffer, or disable it completetely if unneeded
        access_log logs/development-access.log combined buffer=16k;
        # access_log off;

        # Error log
        error_log logs/development-error.log;

        # Va runtime
        location / {
            content_by_lua_block {
                ngx.log(ngx.ERR, '=======>content')
                ngx.say('----------')
                ngx.say('---ccc-------')
                ngx.say('---vvvv-------')
                ngx.say('---vdddv-------')
                ngx.say('---ggvv-------')
            }

    		# content_by_lua_file ./pub/index.lua;
    	}
    }
}
```

*logs/development-error.log截取*
```
set_by_lua:1: =======>set
rewrite_by_lua(development-nginx.conf:31):1: =======>rewrite
access_by_lua(development-nginx.conf:32):1: =======>access
content_by_lua(development-nginx.conf:60):2: =======>content
header_filter_by_lua:1: =======>header_filter
body_filter_by_lua:1: =======>body_filter
body_filter_by_lua:1: =======>body_filter
body_filter_by_lua:1: =======>body_filter
body_filter_by_lua:1: =======>body_filter
body_filter_by_lua:1: =======>body_filter
body_filter_by_lua:1: =======>body_filter
log_by_lua(development-nginx.conf:35):1: =======>log while logging request
```
可以看出body_filter的执行次数等于ngx.say数量加一