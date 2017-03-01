##Vanilla 中使用 Cookie
*Vanilla 中封装了 `vanilla.v.libs.cookie` 包，源至 [`lua-resty-cookie`](https://github.com/cloudflare/lua-resty-cookie)，提供了简单的 `get`、`set`、`getAll` 等方法来控制 Cookie，下面具体使用举例如下：*

###*vanilla.v.libs.cookie 包使用*

*一例胜千言：*

```lua
local IndexController = {}
local vcookie_lib = LoadV('vanilla.v.libs.cookie')

function IndexController:index()
    local cookie = vcookie_lib()
    cookie:set('idevz', 'kkkk', {expires=1000})
    cookie:set('idevz_api', 'kkkk', {expires=1000,path='/'})
    print_r(cookie:getAll())
    do return '' end
end

return IndexController
```

*例释：*

* `local vcookie_lib = LoadV('vanilla.v.libs.cookie')` 载入 `vanilla.v.libs.cookie` 包
* `local cookie = vcookie_lib()` 实例化 `vanilla.v.libs.cookie` 类
* `cookie:set('idevz', 'kkkk', {expires=1000})` 调用 `set` 方法，设置 cookie
* `cookie:getAll()` 调用 `getAll` 方法，获取所有 cookie，也可以调用 `get` 获取单个cookie