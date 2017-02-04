##Vanilla 的 controller
* vanilla 的 controller 是业务处理的关键，vanilla 通过对 URI 的路由，找到本次请求对应的 controller 和 action。*

###*最简单的Controller*
*自动生成的 demo 中默认生成了 IndexController 和 index action（`function IndexController:index()`）*

```lua
local IndexController = {}

function IndexController:index()
    local view = self:getView()
    local p = {}
    p['vanilla'] = 'Welcome To Vanilla...' .. user_service:get()
    p['zhoujing'] = 'Power by Openresty'
    -- view:assign(p)
    do return view:render('index/index.html', p) end
    return view:display()
end

-- curl http://localhost:9110/get?ok=yes
function IndexController:get()
    local get = self:getRequest():getParams()
    print_r(get)
    do return 'get' end
end

-- curl -X POST http://localhost:9110/post -d '{"ok"="yes"}'
function IndexController:post()
    local _, post = self:getRequest():getParams()
    print_r(post)
    do return 'post' end
end

-- curl -H 'accept: application/vnd.YOUR_APP_NAME.v1.json' http://localhost:9110/api?ok=yes
function IndexController:api_get()
    local api_get = self:getRequest():getParams()
    print_r(api_get)
    do return 'api_get' end
end

return IndexController
```

#####*print_r*
*类似 `ngx.say` 的效果，将对象、变量等以易读的格式进行输出，适用于 Vanilla 开发的 Web 服务*

#####*lprint_r*
*`print_r` 的 CLI 版本，适用于 v-console 命令行环境*

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