##Vanilla 的视图引擎
*为去除模板运行时模板解析带来的不必要开销，从 vanilla-0.1.0.rc7 起 Vanilla 默认将使用 OpenResty 官方的 Lemplate 模板引擎，下面是简要用法，以及 Vanilla View 接口介绍*

###*Vanilla 的视图渲染*
*Vanilla 在 `vanilla.v.dispatcher` 中导入了默认的模板引擎（ vanilla-0.1.0.rc7 之前，默认导入的是 `local View = LoadV 'vanilla.v.views.rtpl'` 之后将使用 `vanilla.v.views.lemplate`），如果在 Bootstrap 中未被修改，则使用此引擎来渲染视图。*
*在 Controller 中做模板渲染，只需要获取当前视图实例，注入数据，展示即可。下面就以 Lemplate 模板引擎为例，展示相关用法*

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

*注：以上为 Lemplate 所使用的 TT2 模板实例，关于 Lemplate 的详细使用，可参加其[文档](https://github.com/openresty/lemplate)*