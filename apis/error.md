##Vanilla 的错误处理
*Vanilla 的错误处理分为框架系统错误和应用错误两种类型，系统错误由框架控制，一般导致致命错误，直接抛出 500 内部错误，切不再往下执行，而应用错误则可以通过定义 `errorController` 来自定义处理*

###*Vanilla 应用错误*
*Vanilla 提供了方便的错误处理方式，避免当代码运行报错后，页面只显示一个 500 错误的白页，没有详细报错信息，影响开发效率，Vanilla 的应用错误处理非常简单，在业务开发中，我们所关注的各个组件比如 DAO、Service、Controller、Action、Library 等都可能报错，Controller Action 作为 Vanilla 项目处理请求的执行体，一切业务组件的错误都可以通过一个统一的处理口径 `errorController` 来方便的处理。在业务组件开发过程中的错误，或者用户自定义的错误，都可以在 `errorController` 中得到捕获和处理，默认初始化的 demo 项目中，application/controllers/ 路径下，默认定义了一个 error.lua 文件，这就是前面所说的 `errorController`，下面我们具体来看看这段代码：*

```lua
local ErrorController = {}
local ngx_log = ngx.log
local ngx_redirect = ngx.redirect
local os_getenv = os.getenv


function ErrorController:error()
    local env = os_getenv('VA_ENV') or 'development'
    if env == 'development' then
        local view = self:getView()
        view:assign(self.err)
        return view:display()
    else
        local helpers = require 'vanilla.v.libs.utils'
        ngx_log(ngx.ERR, helpers.sprint_r(self.err))
        -- return ngx_redirect("http://sina.cn?vt=4", ngx.HTTP_MOVED_TEMPORARILY)
        return helpers.sprint_r(self.err)
    end
end
return ErrorController
```

*代码释意：（你只需要关注以下几点，即可随意，按需定义适合你的 errorController）*

* 这是一个普通的 LUA 包，一个普通的 Vanilla Controller，唯一需要注意的一点就是需要实现一个 `error` 方法，注意方法名小写
* 可以通过对运行环境的判断来，对不同的运行环境进行不同的错误处理，比如开发环境可能需要直接将错误打印到页面，而生产环境可能需要出错误页面等


###*Vanilla 框架系统错误（致命错误）*
*当有些 Vanilla 项目所必须的配置或者关键步骤执行异常而影响项目往下运行的情况下，会抛出致命错误，并结束当前请求，目前有以下几种情况*

####*关键配置缺少*
*项目未配置项目名 `name`，或者未指定项目根路径 `root`，Vanilla 有很多地方依赖项目名，比如缓存的 KEY 设置，项目的各类包加载依赖于项目根路径做全局加载，多 APP 支持也依赖与此。如果 config/application.lua 中缺少这两个配置，则会如下错误：*

```bash
            Sys Err: Please set app name and app root in config/application.lua like:
            
                Appconf.name = 'idevz.org'
                Appconf.app.root='/data1/VANILLA_ROOT/idevz.org/'
```

####*bootstrap 报错*
*Bootstrap 中的各种 init 方法并不是必须的，但是如果这部分方法定义后，执行错误，将影响整体项目的正常运行，所以 application/bootstrap.lua 中的运行报错也会报出系统致命错误，举例如下：*

```lua
function Bootstrap:initRoute()
    local router = self.dispatcher:getRouter()
    local restful_route = restful:new(self.dispatcher:getRequest())
    router:addRoute(restful_route, true)
    print_r('xx' .. false)
end
```

*注：以上代码，最后一行操作试图将字符串与 bool 值 false 连接，会报出致命错误，如下：*

```bash
<pre />
"...g/idevz/code/www/vanilla/orcon/application/bootstrap.lua:18: attempt to concatenate a boolean value"
```

####*dispatch 执行报错*
*dispatch 属于框架的请求分发操作，请求分发执行出错直接导致致命错误，不过这个错误由框架自己处理，用户不需要关注*