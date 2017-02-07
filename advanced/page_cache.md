##Vanilla 的 Page Cache
*vanilla 的 Page Cache 实现了类似 Nginx 的 FastCGICache 或者 ProxyCache 的访问结果整体缓存，以 URI 的一定规则作为缓存的 KEY，属于内存型 Cache，存储位置可配置，默认存储在 OpenResty 共享字典（Share Dict）中，默认生成的项目中 Page Cache 为关闭状态*

###*Page Cache 相关配置*
*Page Cache 相关的所有配置见项目的 config/application.lua 中， `Appconf.page_cache` 相关配置段，如下所示：*

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