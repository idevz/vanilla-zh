##Vanilla 的內建变量和方法
*为方便业务开发，Vanilla 提供了一些比较实用的內建方法和变量，这里我们说明如下，随着框架的更新，本页面会及时更新，欢迎随时关注。*

###*Vanilla 的內建变量*
*Vanilla 的內建变量很多来自于 nginx.conf，其他则来自于 `ngx.var`，Vanilla 将这些变量都缓存在了 Registry 中*

#### Registry 变量
*Registry 是 Vanilla 中为了全局数据共享，及高效数据访问而封装的一个全局表，这里缓存了刻画当前请求比较全的数据，具体列表说明如下：*

```lua
-- 以下数据以 "curl http://domain.org/?arg1=aa1&arg2=aa2" 访问为例进行说明
Registry['APP_CONF']	-- 当前应用的配置数据，来自于（config/application.lua）
Registry['sys_conf']	-- 当前应用的系统配置，来自于（sys/*路径，比如可以使用 Registry['sys_conf']['cache'] 获取 sys/cache 文件中关于 cache 的配置）
Registry['REQ_URI']		-- 当前请求的 URI ，为 "/"
Registry['REQ_ARGS']	-- 当前请求的参数字符串，即 Query_String，为 "arg1=aa1&arg2=aa2"
Registry['REQ_ARGS_ARR']	-- 当前请求的参数列表，为一个 LUA 数组
Registry['REQ_HEADERS']		-- 当前请求的请求头数组
Registry['APP_NAME'] 		-- 应用名称
Registry['APP_ROOT'] 		-- 应用所在根目录
Registry['APP_HOST'] 		-- 当前请求的 HOST 信息
Registry['APP_PORT'] 		-- 当前请求的 PORT 信息
Registry['VANILLA_ROOT'] 	-- VANIALLA 框架的根目录
Registry['VANILLA_VERSION'] -- 当前所使用的 VANILLA 版本号
Registry['VANILLA_APPLICATION'] -- 'vanilla.v.application' LUA 包
Registry['VANILLA_UTILS'] 		-- 'vanilla.v.libs.utils' LUA 包
Registry['VANILLA_CACHE_LIB'] 	-- 'vanilla.v.cache' LUA 包
Registry['VANILLA_COOKIE_LIB'] 	-- 'vanilla.v.libs.cookie' LUA 包
Registry['APP_BOOTS'] 			-- 应用 'application.bootstrap' LUA 包
Registry['APP_PAGE_CACHE_CONF'] -- 应用 Page Cache 相关配置
```

*注：上面很多全局变量是从 `ngx.var.` 获取来的结果缓存的，这样避免每次都请求 `ngx.var` 而减少这部分性能开销，并且其中有些信息比如 `APP_NAME`，`APP_ROOT` 等服务一经启动就不会更改，而像 `REQ_*` 相关的数据则是每次请求都不一样，好在一次请求可能对这部分数据会多次调用，所以将其缓存在 `Registry` 表中*

###*Vanilla 的內建函数*
*Vanilla 有很多内建的函数，这些函数有些来自于 Vanilla 框架本身功能性的一些 LUA 包中，比如 `vanilla.v.controller`、`vanilla.v.request`、`vanilla.v.response` 等，另一些比如通用的方法，比如 `print_r`、 `page_cache` 和 `vanilla_init`，再有比如 Vanilla 定义的各种包加载函数，列表如下：*

```lua
LoadLibrary 	-- 加载项目 library 路径下的 LUA 包
LoadController 	-- 加载项目 controller
LoadModel 	-- 加载项目 model 路径下的包，包括 DAO 和 Service
LoadPlugin 	-- 加载项目 plugins 路径下所定义的插件
LoadApplication 	-- 加载项目 application 路径下的 LUA 包
LoadApp 	-- 加载项目根目录下面的 LUA 包
LoadV 	-- 加载 Vanilla 框架相关的 LUA 包
```

*注：以上有些加载器功能重复，目的在于减短所传递参数的长度，比如加载 Index Controller， 使用 LoadController 方法是，只需要写 `LoadController('index')， 而如果使用方法 LoadApp 则应该写成 `LoadApp('application.controllers.index')`*

#### 方法 `page_cache`
*该方法调用 Vanilla 封装的页面缓存逻辑，详细内容参见[进阶/页缓存](../advanced/page_cache.md)*

#### 单步调试方法 `print_r`、`sprint_r` 等
*调试系列方法主要为了开发时能清晰方便的查看变量状态，记录开发日志等功能，详细内容参见[快速上手/如何调试](../quick_learning/debug.md)*

#### 方法 `init_vanilla`
*方法 `init_vanilla` 主要完成框架基础功能的初始化，比如 Registry 的初始化，各种 Loader 的定义，页面缓存的实现等，本方法默认在应用请求处理入口的第一句语句执行*
