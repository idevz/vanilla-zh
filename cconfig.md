##配置
*香草/Vanilla的配置由以下三个部分组成.*
- *App配置*
- *Nginx配置*
- *WAF配置*

###App配置
#####*application.lua*
```
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

#####*errors.lua*
*根据errors.lua文件中实例，配置用户级别错误码.*
```
local Errors = {
    [1000] = { status = 500, message = "Controller Err." },
}

```

###Nginx配置
#####*nginx.lua*
*分为ngx_conf.common和ngx_conf.env两个部分，common是对Openresty指令集的配置如INIT_BY_LUA，可以是包或者文件(BY_LUA_FILE)，env是环境的部分，包括了开发环境，测试环境和生产环境端口和缓存配置等控制.*
```
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
