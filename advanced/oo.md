##面向对象的 Vanilla
*Lua 提供了部分面向对象的语法糖，这仅仅能在开发中提供一个功能不完备的独立 Class 的使用，有 `self` 可以来引用 LUA 表的某些属性和方法，但是更多的面向对象特性，比如继承，比如类的构造等，LUA 支持的并不是非常好，日常的业务开发中，我们确实有些通用的逻辑可能需要复用，或者数据需要共享，需要有父子关系等等。所以我们在 Vanilla 中，简单封装了部分面向对象的特性，这里我们简单介绍其使用方法。*

###*一个简单的 Vanilla 类*
*没啥好说的，直接上代码：*

####*类定义*

```lua
local LibA = Class("LibA")

function LibA:idevzDo(params)
    local params = params or { lib_bb = 'idevzDo LibA'}
    return params
end

function LibA:__construct( data )
    print_r('===============init bbb=========')
    self.lib = 'LibA---------------xxx' .. data.info
    -- self.a = 'ppp'
end

return LibA
```

####*类使用*

```lua
local LibA = LoadLibrary('LibA')

```

*代码释意：*

* `Class("LibA")` 声明一个 Vanilla 类，类名为 `LibA`
* `LibA:__construct( data )` 提供了一个类 LibA 的构造器

```lua
Appconf.page_cache = {}
Appconf.page_cache.cache_on = true
-- Appconf.page_cache.cache_handle = 'lru'
Appconf.page_cache.no_cache_cookie = 'va-no-cache'
Appconf.page_cache.no_cache_uris = {
    'uris'
}
Appconf.page_cache.build_cache_key_without_args = {'rd'}
```

#####*配置释意*

* cache_on 缓存开关，true 为开启 Page Cache，false 则为关闭
* cache_handle 设置 Page Cache 的存储介质，目前支持 Memcache、Redis、resty.lrucache、OpenResty Share Dict，默认为 OpenResty Share Dict
* no_cache_cookie 设置不缓存的 cookie KEY，Vanilla Page Cache 使用这个设置所指的 cookie KEY 来对某些特殊页面不缓存，默认当页面中有 KEY 为 `va-no-cache` 这个 COOKIE 的时候，当前页面不缓存
* no_cache_uris 设置不缓存的 URI 列表，默认配置例如 http://app.com/uris 命中 `uris` 则，当前页面不缓存
* build_cache_key_without_args 设置在缓存 KEY 中去除某些参数，比如某些 API 的版本号，或者随机数等，默认配置中的 `rd` 设置代表，当 URI 中有 rd 参数时，则生成的 Page Cache KEY 中清除这个参数

*注：缓存的清理，只需要在请求的 URL 中，添加参数 `vapurge`*