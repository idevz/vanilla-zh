## 路由
*路由是香草/Vanilla的请求分发的关键，这个过程由路由器```vanilla.v.router```和一序列路由协议协同完成（从```vanilla-0.1.0.rc4```开始，vanilla支持两种比较实用的路由协议```vanilla.v.routes.restful```和```vanilla.v.routes.simple```）*


- *简单路由协议```vanilla.v.routes.simple```用法*
- *Restful路由协议```vanilla.v.routes.restful```用法*
- *路由器*
- *路由协议*
- *默认的路由协议*
- *bootstrap初始化路由器*
- *路由协议栈*
- *路由协议的加载与管理*

### *简单路由协议```vanilla.v.routes.simple```用法*

```vanilla.v.routes.simple```是Vanilla协议栈中默认使用的协议，不需要特殊配置，直接可用

##### *使用Tips：*

- *URI为根'/'时，```controller```和```action```都默认为```index```*
- *按照URI的路径进行分割，最后的一段为```action```，前面则为```controller```（例如URI为'/a/b/c'，则路由结果```controller```为```a.b```，```action```为```c```）*

### *简单路由协议```vanilla.v.routes.restful```用法*

### 路由器

*Vanilla通过路由器管理着一个路由协议栈，并通过遍历协议栈上每一个路由协议的```match```方法来获取当前请求的```controller```和```action```*

下面是```vanilla.v.router```目前所提供的函数列表：

```
function Router:new(request) --传入request实例初始化路由器，默认已经启用：vanilla.v.routes.simple
function Router:addRoute(route, only_one) --添加路由协议
function Router:removeRoute(route_name) --传入路由协议名称删除路由协议
function Router:getRoutes() --获取当前的路由协议栈
function Router:getCurrentRoute() --获取当前请求所使用的路由协议实例
function Router:getCurrentRouteName() --获取当前请求所使用的路由协议名称
function Router:route() --路由当前请求
```


### 路由协议
### 默认的路由协议
##### *application.lua*
##### *errors.lua*
### bootstrap初始化路由器
### 路由协议栈
### 路由协议的加载与管理
