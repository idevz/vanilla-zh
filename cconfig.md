##配置
*香草/Vanilla的配置由以下三个部分组成.*
- *App配置*
- *Nginx配置*
- *WAF配置*

###App配置
#####*应用基础配置（config/application.lua）*

```lua
Appconf.sysconf = {				--系统预加载配置文件
    'v_resource',
}
Appconf.name = 'app_name'		--app名称，执行vanilla new命令时给定的应用名

Appconf.route='vanilla.v.routes.simple'		--路由器，指定URL路由方式，目的解析出需要执行的controller与action
Appconf.bootstrap='application.bootstrap'		--初始化bootstrap（用来对应用进行初始化操作）
Appconf.app={}		--app相关配置
Appconf.app.root='./'		--当前vanilla start命令执行路径

Appconf.controller={}		--当前app的controller相关配置
Appconf.controller.path=Appconf.app.root .. 'application/controllers/'		--controller文件所在路径（使用默认生成路径即可）

Appconf.view={}		--当前app的视图层相关配置
Appconf.view.path=Appconf.app.root .. 'application/views/'		--模板路径
Appconf.view.suffix='.html'		--模板后缀
Appconf.view.auto_render=true		--是否开启自动渲染
```

#####*应用基础配置的引用*

```lua
-- 如上的配置，可以在代码中通过 Registry['APP_CONF'] 表来进行获取，比如获取 APP_NAME
local app_name = Registry['APP_CONF']['name']
```

#####*错误处理配置（config/errors.lua）*
*根据errors.lua文件中实例，配置用户级别错误码.*

```lua
local Errors = {
    [1000] = { status = 500, message = "Controller Err." },
}
```

#####*Restful 路由协议配置（config/restful.lua）*
*根据 URI 需要来自定义路由协议的配置*

```lua
local restful = {
    v1={},
    v={}
}

restful.v.GET = {
    {pattern = '/', controller = 'index', action = 'index'},
    {pattern = '/:category', controller = 'index', action = 'list'}
}

restful.v.POST = {
    {pattern = '/post', controller = 'index', action = 'post'},
}

restful.v1.GET = {
    {pattern = '/api', controller = 'index', action = 'api_get'},
}

return restful
```



#####*系统相关配置（sys/*）*
*比如DB、MC等资源配置，系统相关的分机房配置等（在某些大公司，这部分配置又运维人员统一管理和下发），文件格式目前使用相对更运维友好的 ini 文件，开发中可以方便的在 Registry['sys_conf'] 中获取相关数据，如 `Registry['sys_conf']['cache']['lrucache']` 获取 lrucache 相关配置*

#####*系统缓存相关配置 （sys/cache）*

```ini
[shared_dict]
dict=idevz
exptime=100

[memcached]
instances=127.0.0.1:11211 127.0.0.1:11211
exptime=60
timeout=100
poolsize=100
idletimeout=10000

[redis]
instances=127.0.0.1:6379 127.0.0.1:6379
exptime=60
timeout=100
poolsize=100
idletimeout=10000

[lrucache]
items=200
exptime=60
useffi=false
```

* 目前这部分配置一般由 vanilla.v.libs.cache 来使用
* 目前支持的配置项如 poolsize（连接池大小）、timeout（数据获取超时等）

#####*系统缓存相关配置 （sys/v_resource）*

```ini
[mc]
conf=127.0.0.1:7348 127.0.0.1:11211

[redis]
conf=127.0.0.1:7348 127.0.0.1:7349

[redisq]
conf=127.0.0.1:7348 127.0.0.1:7349

[db.user.write]
host =127.0.0.1
port =3306
dbname =user.info
user =idevz
passwd =idevz

[db.user.read]
host =127.0.0.1
port =3306
dbname =user.info
user =idevz
passwd =idevz
```

* 对所使用的数据资源做配置
* `Registry['sys_conf']['v_resource）']['db.user.write']['host']` 获取写库的 HOST 信息

###Nginx配置
#####自动生成的 Nginx 配置文件
*初始化项目的时候会在项目目录下（nginx_conf/）自动生成这个项目所对应的两套（分别对应开发和线上环境）配置文件，生成的两套配置文件中，每套都包含 nginx.conf 和 vhost 两个配置文件*

#### 生产环境
###### va-nginx.conf 文件
*va-nginx.conf 配置文件内容包含 nginx 配置主干（main、events、http 等重点配置段），包括用户、组的配置，工作进程等等通用配置，关键的还有 `lua_package_path`、`lua_package_cpath` 的配置，还有框架初始化文件（vanilla/framework/init.lua）的加载*

###### vhost/app_name.conf 文件
*vhost/app_name.conf 文件是当前应用的相关配置，包括 APP_NAME、VANILLA_VERSION、$template_root、$va_cache_status 等全局变量的初始化，$document_root，Server_name 等的设置，还有关键的应用入口（content_by_lua_file），lua_shared_dict 等的设置，不过这些设置都是自动生成的，开发人员没有特殊需求的话，并不需要关注这些*

#### 开发环境
###### va-nginx-development.conf 文件
*va-nginx-development.conf 文件的内容跟开发环境类似，唯一的区别在于加载框架初始化文件（vanilla/framework/init.lua）的方式为 `init_by_lua_file`*

###### dev_vhost/app_name.conf 文件
*默认的dev_vhost/app_name.conf 文件的配置同生产环境的配置基本一样，关键不同在于 `lua_code_cache` 的设置*

*注：所以初始化项目后，首先需要执行 `sudo ./va-app_name-service initconf dev` 命令，就是为了将自动生成的配置文件部署到 OpenResty 默认的配置文件路径下，如果需要更新 va-nginx（-development）.conf 则还需要在命令后面加上 `-f` 参数进行强行部署，每次如果需要修改配置，也只需修改这部分配置，然后执行 `initconf` 即可*

#####*nginx.lua*( vanilla-0.1.0.rc5 后废弃此配置 )
*分为ngx_conf.common和ngx_conf.env两个部分，common是对Openresty指令集的配置如INIT_BY_LUA，可以是包或者文件(BY_LUA_FILE)，env是环境的部分，包括了开发环境，测试环境和生产环境端口和缓存配置等控制.*

```lua
ngx_conf.common = {
    INIT_BY_LUA = 'nginx.init',
    CONTENT_BY_LUA_FILE = './pub/index.lua'
}
ngx_conf.env = {}
ngx_conf.env.development = {
    LUA_CODE_CACHE = false,
    PORT = 7200
}
```

###WAF配置
#####*waf.lua*
*包括WAF规则的配置，及各种规则参数的配置，相关使用方法详见[waf](https://github.com/loveshell/ngx_lua_waf)*
