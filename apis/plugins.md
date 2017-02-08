##Vanilla 的插件体系
*为了减少运行占用的系统资源，使开发更简便，Vanilla 默认只运行在 content_by_lua 这个 phrase，但是为了支持业务开发有层次化的请求控制，Vanilla 实现了便捷的插件机制，提供了六个钩子，给请求的细粒度控制提供了可能。下面我们来看看如何使用*

###*Vanilla Plugin 的简单使用*
*在 Vanilla 项目中使用插件是非常简单的，只需要在 application/plugins/ 路径下实现 Vanilla 的插件 LUA 包即可，插件包可以按需实现 6 个钩子的方法。默认生成的 demo 项目中自动生成了一个 admin plugin，见 application/plugins/admin.lua。*

```lua
local AdminPlugin = LoadV('vanilla.v.plugin'):new()

function AdminPlugin:routerStartup(request, response)
    print_r('<pre>')
    if request.method == 'GET' then
        print_r('-----------' .. sprint_r(request.headers) .. '----------')
    else
        print_r(request.headers)
    end
end

function AdminPlugin:routerShutdown(request, response)
end

function AdminPlugin:dispatchLoopStartup(request, response)
end

function AdminPlugin:preDispatch(request, response)
end

function AdminPlugin:postDispatch(request, response)
end

function AdminPlugin:dispatchLoopShutdown(request, response)
end

return AdminPlugin
```

*注：以上为 Vanilla 简单 plugin 的实例，六个钩子方法按需实现，空方法可去掉，如下所示：*

```lua
local AdminPlugin = LoadV('vanilla.v.plugin'):new()

function AdminPlugin:routerStartup(request, response)
    print_r('<pre>')
    if request.method == 'GET' then
        print_r('-----------' .. sprint_r(request.headers) .. '----------')
    else
        print_r(request.headers)
    end
end

return AdminPlugin
```

####*Vanilla Plugin 的调用*
*Vanilla Plugins 的调用非常简单，只需要在 application/bootstrap.lua 中实现 `initPlugin` 方法，并调用 `dispatcher` 的插件注册方法将插件注入项目（ `self.dispatcher:registerPlugin(admin_plugin)`），即能在对应的时机执行相关钩子对应的方法*

```lua
local Bootstrap = Class('application.bootstrap')

function Bootstrap:initPlugin()
    local admin_plugin = LoadPlugin('plugins.admin'):new()
    self.dispatcher:registerPlugin(admin_plugin);
end

function Bootstrap:boot_list()
    return {
        Bootstrap.initPlugin,
    }
end

function Bootstrap:__construct(dispatcher)
    self.dispatcher = dispatcher
end

return Bootstrap
```

###*Vanilla 支持的插件钩子*
*以下列出 Vanilla 支持的 6 中插件钩子*

```lua
function Plugin:routerStartup(request, response)			-- 开始路由
function Plugin:routerShutdown(request, response)			-- 路由结束
function Plugin:dispatchLoopStartup(request, response)		-- 开始请求分发
function Plugin:preDispatch(request, response)				-- 预分发（载入相关的 controller）
function Plugin:postDispatch(request, response)				-- 请求响应
function Plugin:dispatchLoopShutdown(request, response)		-- 请求分发执行结束
```