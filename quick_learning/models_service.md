##Vanilla 的 Service
*vanilla 的 Service 预设为项目对某些通用业务逻辑封装为独立的 Service，方便维护、管理、缓存等。 Vanilla 的 Service 在项目的 models/service 路径下，一般使用 `LoadModel` 方法进行加载*

###*最简单的 Service*
*由自动生成的 demo 中默认生成了 UserService，可以看出 UserService 也只是一个普通的 LUA 包。不过 Service 一般调用更底层的 DAO ，并对之做必要封装，并将相关的 Service 暴露给 Controller 使用*

```lua
local table_dao = LoadApplication('models.dao.table'):new()
local UserService = {}

function UserService:get()
    table_dao:set('zhou', 'UserService res')
    return table_dao.zhou
end

return UserService
```

#####*以上代码解释*
*Service 可以是任何对数据层访问封装的 LUA 包*