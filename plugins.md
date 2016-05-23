##插件
*香草/Vanilla的插件在Vanilla处理请求的过程中提供了六个执行相关逻辑的时机.*

- *routerStartup 路由开始*
- *routerShutdown 路由结束，已经算出controller和action结果*
- *dispatchLoopStartup 开始准备进入请求转发循环*
- *preDispatch 准备转发（目前的版本0.1.0-rc4，此时机同dispatchLoopStartup重合，后期有可能变化）*
- *postDispatch 请求计算结果获取到后准备POST结果*
- *dispatchLoopShutdown 响应结束，请求完成*

###使用
#####*例子：一个管理后台的插件Admin，功能就是对用户权限做一个判定（判断比如这个用户是否已经登录，如果未登录则不允许访问）*
我们可以在请求路由结束后，已经计算出当前请求需要执行的controller和action的情况下，在AdminPlugin:routerShutdown这个hook添加一个验证的逻辑，如果为登录则跳转到登录页面。
``` lua
function AdminPlugin:routerShutdown(request, response)
	if auth() ~= true then
		return ngx.redirect("/login", ngx.HTTP_MOVED_TEMPORARILY)
	end
end
```
