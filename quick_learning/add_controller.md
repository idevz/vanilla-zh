##Vanilla 的 controller
* vanilla 的 controller 是业务处理的关键，vanilla 通过对 URI 的路由，找到本次请求对应的 controller 和 action。*

###*最简单的 Controller*
*自动生成的 demo 中默认生成了 IndexController 和 index action（`function IndexController:index()`），默认使用简单路由协议（`vanilla.v.routes.simple`）对 URI 进行路由*

```lua
local IndexController = {}

-- curl http://localhost:9110
function IndexController:index()
    local view = self:getView()
    local p = {}
    p['vanilla'] = 'Welcome To Vanilla...' .. user_service:get()
    p['zhoujing'] = 'Power by Openresty'
    -- view:assign(p)
    do return view:render('index/index.html', p) end
    return view:display()
end

-- curl http://localhost:9110/index/action_b
function IndexController:action_b()
    return 'index->action_b'
end

return IndexController
```

#####*以上代码解释*
*关于上面的 controller 实例代码，我们只需关注下面几点*

* IndexController:index （index Controller 中的 index Action），通过 `self:getView()` 方法获取视图实例
* 可以通过先调用 `view:assign(p)` 将所需要的参数传入视图，再调用 `view:display()` 进行模板渲染，或者可以直接调用 `view:render('index/index.html', p)` 方法，指定需要渲染的模板，并同时传入相应的参数
* 模板参数都是与 LUA 数组的形式进行传递
* 每个 action 的返回值都必须是字符串，所以可以知道 `view:display()` 和 `view:render()` 方法都是返回字符串
* IndexController:action_b （index Controller 中的 action_b Action，这里注意，action 的方法名必须小写），使用默认的简单路由协议，访问 URI 为 `curl http://localhost:9110/index/action_b`

###*新添加一个 Controller*
*给 Vanilla 添加一个新的 Controller 非常简单，只需要在*

```bash
╰─○ v-console-0.1.0.rc6
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
v-console>a={}
v-console>a.v1='a_v1'
v-console>a.v2='a_v2'
v-console>lprint_r(a)
{
  v2 = "a_v2",
  v1 = "a_v1"
}
v-console>
```

#####*err_log*
*err_log 方法是对 `ngx.ERR` 的封装，将 `msg` 记录到 nginx 错误日志*