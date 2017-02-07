##使用 Bootstrap 来做服务初始化
*Vanilla 使用 Bootstrap 来做应用初始化的工作，用户可以在此对应用做一些配置（比如所使用的路由协议，使用何种视图引擎），对配置做一些初始化加载，初始化 WAF，初始化 Plugins 等操作，Vanilla 运行在 OpenResty content_by_lua\*这个 Phrase，使用 Bootstrap 可以很好的实现对请求的细粒度控制*

###*Bootstrap 即类 `application.bootstrap`*
*Bootstrap 的实现其实是一个名为 `application.bootstrap` 的 Vanilla 类，实现了构造器初始化的属性只有一个（当前请求所使用的 dispatcher），我们只需要关注根据需求实现各种 init 方法即可，最后只要在 `boot_list` 方法返回的列表中的 init 方法都会被顺序执行*

```lua
function Bootstrap:boot_list()
    return {
        -- Bootstrap.initWaf,
        -- Bootstrap.initErrorHandle,
        Bootstrap.initRoute,
        -- Bootstrap.initView,
        -- Bootstrap.initPlugin,
    }
end

function Bootstrap:__construct(dispatcher)
    self.dispatcher = dispatcher
end
```

*上面的定义代表只有 `initRoute` 方法会被执行，而上面两个方法的实现我们并不需要关心和更改，只需要定义各种 init 方法，并更新 `boot_list` 返回的表元素即可*