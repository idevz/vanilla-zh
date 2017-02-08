##Vanilla 的路由体系
*Vanilla 实现的路由体系有一个路由器（`vanilla.v.router`）和若干路由协议构成（Vanilla 默认实现了 `vanilla.v.routes.simple` 和 `vanilla.v.routes.restful` 两种路由协议，默认使用 `simple` 路由来路由请求），请求处理的开始阶段，Vanilla 通过调用路由器协议栈中的各种路由协议，计算出处理当前请求的 `controller` 和 `action`，这就是 Vanilla 路由体系的职责所在。如果默认的两种路由协议不能满足你的 URI 路由需求，你可以参考我的一篇[《如何给Vanilla(OpenResty)添加一个路由协议》](https://my.oschina.net/idevz/blog/603657)的博文*

###*Vanilla 路由器*
*Vanilla 的路由器 `vanilla.v.router` 是请求路由的基础，路由器提供了对路由协议的添加 `addRoute(route, only_one)`，删除 `removeRoute(route_name)`，获取路由列表 `getRoutes()` 等方法，用户可以调用这些方法来管理路由协议栈并使用路由器，不过用户不需要关心路由器的实现，而只需要关注路由协议的实现。*

####*给路由器添加一条路由协议*
*路由器只有唯一一个 `vanilla.v.router`，但路由协议可以有多个，通过 `addRoute(route, only_one)` 方法的调用可以向路由协议栈添加一条路由协议，第二个参数为可选参数，当设置为 `true` 时，代表将清空路由协议栈，只使用当前添加的这条路由协议，因为 Vanilla 路由请求的方式是路由器根据路由协议栈中的路由协议挨条解析，直到找到匹配的 `controller` 和 `action` 为止，太多的路由协议栈可能影响路由性能。*

####*删除一条路由协议*
*Vanilla 的每条路由协议都有 `route_name` 属性，删除时只需要调用 `removeRoute(route_name)`*

####*获取当前所使用的路由协议*
*Vanilla 支持获取当前请求所使用的路由协议，只需调用 `getCurrentRoute()` 方法，调用 `getCurrentRouteName()` 方法可以获取当前路由协议名*

###*路由协议*
*路由协议非常的简单，因为路由协议的关键功能在于为当前请求找到对应的 `controller` 和 `action`，核心在于根据当前请求实例 `request`，通过实现 `match` 方法，来获取结果，下面是根据 `vanilla.v.routes.simple` 路由协议提炼出来的路由协议简单骨架：*

```lua
local Simple = {}

function Simple:new(request)
    local instance = {
        route_name = 'vanilla.v.routes.simple',
    	request = request
    }

    setmetatable(instance, {
        __index = self,
        __tostring = function(self) return self.route_name end
        })
    return instance
end

function Simple:match()
end

return Simple
```

*注：我们需要关注以下两点*

* `route_name` 这是路由协议栈索引的关键，协议栈中的路由协议依靠 route_name 进行管理
* `request` 是当前请求的实例，包含了当前请求携带的 URI，http_header 等数据，是请求路由的依据