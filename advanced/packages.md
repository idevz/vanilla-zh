##Vanilla 的包开发
*可以使用任意 LUA 包的开发方式来开发 Vanilla 包（Controllers，Library，Dao，Services等），也可以使用 Vanilla 所提供的[面向对象](./oo.md) 方式进行开发*

###*对 Controller 使用继承和构造器*
*下面我们看一个例子：*

```lua
local IndexController = Class('controllers.index', 
    LoadApplication('controllers.base'))

function IndexController:__construct()
    self.parent:__construct()
end

return IndexController
```
