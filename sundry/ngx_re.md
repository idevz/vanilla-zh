###Openresty 环境中正则表达式使用示例积累

*一些觉得比较有代表性的使用方式都收集到这里*

####精确匹配字符

*要求 citycode 的值必须是前4位为字符，后4位为数字的字符串*

```lua
local query_str = [[citycode=CHXX0056&timestamp=1487127951]]
local args = ngx.decode_args(query_str, 0)		--解析 query_string 获取的字段数组
local str = args['citycode']
local rs1,rs2, err = ngx.re.find(str, [[^([A-Z]{4})([0-9]{4})$]])  --正则查找
print_r(rs1)
print_r(rs2)
```