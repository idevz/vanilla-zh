##Vanilla 中使用 Cookie
*Vanilla 中封装了 `vanilla.v.libs.cookie` 包，源至 [`lua-resty-cookie`](https://github.com/cloudflare/lua-resty-cookie)，提供了简单的 `get`、`set`、`getAll` 等方法来控制 Cookie，下面具体使用举例如下：*

###*vanilla.v.libs.cookie 包使用*

*一例胜千言：*

```lua
local IndexController = {}

-- 载入 `vanilla.v.libs.cookie` 包
local vcookie_lib = LoadV('vanilla.v.libs.cookie')

function IndexController:index()

    -- 实例化 `vanilla.v.libs.cookie` 类
    local cookie = vcookie_lib()

    -- 调用 `set` 方法，设置 cookie
    cookie:set('idevz', 'kkkk', {expires=1000})
    cookie:set('idevz_api', 'kkkk', {expires=1000,path='/'})
    
    -- 调用 `getAll` 方法，获取所有 cookie，也可以调用 `get` 获取单个cookie
    print_r(cookie:getAll())
    do return '' end
end

return IndexController
```

*注：*

* `vanilla.v.libs.cookie` 支持以下 cookie 选项

```lua
path
domain
max_age
secure
httponly
samesite
extension
```
