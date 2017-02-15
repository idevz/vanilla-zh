##Vanilla 的视图引擎
*为去除模板运行时模板解析带来的不必要开销，从 vanilla-0.1.0.rc7 起 Vanilla 开始支持 OpenResty 官方的 Lemplate 模板引擎，下面将简要介绍 Vanilla 中 Lemplate 的用法，以及 Vanilla View 接口介绍*

###*Vanilla 的视图渲染*
*Vanilla 在 `vanilla.v.dispatcher` 中导入了默认的模板引擎（`local View = LoadV 'vanilla.v.views.rtpl'`），但是可以在 Bootstrap 中实现 `initView` 来修改所使用的视图引擎。*
*而 Vanilla 的模板渲染，只需要在相应的 Action 中获取当前视图实例，注入数据，展示即可。下面就以 Lemplate 模板引擎为例，展示相关用法*

####*首先在 Bootstrap 中实现 `initView` 方法，修改项目所使用的视图引擎*

```lua
-- application/bootstrap.lua
function Bootstrap:initView()
    local view = LoadV('vanilla.v.views.lemplate'):new(self.dispatcher.application.config.view)
    self.dispatcher:setView(view)
end

-- boot_list 中打开 Bootstrap.initView 方法调用
function Bootstrap:boot_list()
    return {
        -- Bootstrap.initWaf,
        -- Bootstrap.initErrorHandle,
        -- Bootstrap.initRoute,
        Bootstrap.initView,
        -- Bootstrap.initPlugin,
    }
end
```

####*运行 `./va-{app_name}-service ltpl` 命令调用 `Lemplate` 编译你的 TT2 模板*

*下面是 TT2 模板示意*

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<title>[% title %]</title>
</head>
<body>
<div>
[% FOREACH userinfo IN userlists %]
<p>
	<h6>姓名：[% userinfo.name %] / 地址：[% userinfo.addr %]</h6>
</p>
[% END %]
</div>
</body>
</html>
```

####*在 Vanilla Action 中调用编译好的模板*

```lua
function IndexController:index()
    local view = self:getView()
    local users = {
                        {name='idevz', addr='yunnan'},
                        {name='vanilla', addr='beijing'},
                    }
    return view:assign({userlists=users, title = 'Vanilla-Lemplate'})
end
```

*注：*

* `local view = self:getView()` 获取当前视图实例
* `view:assign({userlists=users, title = 'Vanilla-Lemplate'})` 将数据注入视图

*注：以上为 Lemplate 所使用的 TT2 模板实例，关于 Lemplate 的详细使用，可参考其详细[文档](https://github.com/openresty/lemplate)*