##Vanilla 的內建变量和方法
*为方便业务开发，Vanilla 提供了一些比较实用的內建方法和变量，这里我们说明如下，随着框架的更新，本页面会及时更新，欢迎随时关注。*

###*Vanilla 的內建变量*
*Vanilla 的內建变量*

VANILLA_REGISTRY = {}
Registry = require('registry'):new()
local sysconf_files = Registry['APP_CONF']['sysconf']
Registry['sys_conf'] = {}
Registry['sys_conf'][v] = conf_handle:get('sys/' .. v)
Registry.namespace = ngx_var.APP_NAME
Registry['REQ_URI'] = ngx_var.uri
Registry['REQ_ARGS'] = ngx_var.args
Registry['REQ_ARGS_ARR'] = ngx_req.get_uri_args()
Registry['REQ_HEADERS'] = ngx_req.get_headers()
Registry['APP_CACHE_PURGE'] = Registry['REQ_ARGS_ARR']['vapurge']
Registry['VA_ENV'] = ngx_var.VA_ENV
Registry['APP_NAME'] = Registry.namespace
Registry['APP_ROOT'] = ngx_var.document_root
Registry['APP_HOST'] = ngx_var.host
Registry['APP_PORT'] = ngx_var.server_port
Registry['VANILLA_ROOT'] = ngx_var.VANILLA_ROOT
Registry['VANILLA_VERSION'] = ngx_var.VANILLA_VERSION
Registry['VANILLA_APPLICATION'] = LoadV 'vanilla.v.application'
Registry['VANILLA_UTILS'] = LoadV 'vanilla.v.libs.utils'
Registry['VANILLA_CACHE_LIB'] = LoadV 'vanilla.v.cache'
Registry['VANILLA_COOKIE_LIB'] = LoadV 'vanilla.v.libs.cookie'
Registry['APP_CONF'] = LoadApp 'config.application'
Registry['APP_BOOTS'] = LoadApp 'application.bootstrap'
Registry['APP_PAGE_CACHE_CONF'] = Registry['APP_CONF']['page_cache']
Registry['VANILLA_INIT'] = true
local cookie_lib = Registry['VANILLA_COOKIE_LIB']
local no_cache_uris = Registry['APP_PAGE_CACHE_CONF']['no_cache_uris']
Registry['COOKIES'] = cookie:getAll()
if Registry['APP_PAGE_CACHE_CONF']['cache_on'] then
local del_keys = Registry['APP_PAGE_CACHE_CONF']['build_cache_key_without_args']
Registry['USE_PAGE_CACHE'] = use_page_cache()
local cache_lib = Registry['VANILLA_CACHE_LIB']
Registry['page_cache_handle'] = Registry['APP_PAGE_CACHE_CONF']['cache_handle'] or 'shared_dict'
local cache = cache_lib(Registry['page_cache_handle'])
Registry['APP_PAGE_CACHE_KEY'] = Registry['REQ_URI'] .. ngx.encode_args(clean_args(Registry['REQ_ARGS_ARR']))
if Registry['APP_CACHE_PURGE'] then
cache:del(Registry['APP_PAGE_CACHE_KEY'])
local rs = cache:get(Registry['APP_PAGE_CACHE_KEY'])
if ngx_re_find(Registry['REQ_HEADERS']['accept'], 'json') then

###*Vanilla 的內建函数*

LoadLibrary
LoadController
LoadModel
LoadPlugin
LoadApplication
LoadApp
LoadV
