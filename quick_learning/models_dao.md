##Vanilla 的 DAO
*vanilla 的 DAO 预设为项目对数据源的封装，一切对数据源的操作都可以封装成 DAO，方便维护、管理、缓存等。 Vanilla 的 DAO 在项目的 models/dao 路径下，一般使用 `LoadModel` 方法进行加载*

###*最简单的 DAO*
*由自动生成的 demo 中默认生成了 TableDao，可以看出 TableDao 只是一个普通的 LUA 包。*

```lua
local TableDao = {}

function TableDao:set(key, value)
    self.__cache[key] = value
    return true
end

function TableDao:new()
    local instance = {
        set = self.set,
        __cache = {}
    }
    setmetatable(instance, TableDao)
    return instance
end

function TableDao:__index(key)
    local out = rawget(rawget(self, '__cache'), key)
    if out then return out else return false end
end
return TableDao
```

#####*以上代码解释*
*DAO 可以是任何对数据层访问封装的 LUA 包*