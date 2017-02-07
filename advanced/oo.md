##面向对象的 Vanilla
*Lua 提供了部分面向对象的语法糖，这仅仅能在开发中提供一个功能不完备的独立 Class 的使用，有 `self` 可以来引用 LUA 表的某些属性和方法，但是更多的面向对象特性，比如继承，比如类的构造等，LUA 支持的并不是非常好，日常的业务开发中，我们确实有些通用的逻辑可能需要复用，或者数据需要共享，需要有父子关系等等。所以我们在 Vanilla 中，简单封装了部分面向对象的特性，这里我们简单介绍其使用方法。*

###*一个简单的 Vanilla 类*
*下面我们看一个例子：*

#####*类定义*

```lua
local LibA = Class("LibA")

function LibA:idevzDo(params)
    local params = params or { lib_bb = 'idevzDo LibA'}
    return params
end

function LibA:__construct( data )
    self.name = 'name-->' .. data.name
    self.sex = 'sex-->' .. data.sex
end

return LibA
```

*代码释意：*

* `Class("LibA")` 声明一个 Vanilla 类，类名为 `LibA`
* `LibA:__construct( data )` 提供了一个类 LibA 的构造器，并对相应的属性进行初始化

#####*类使用*

```lua
local LibA = LoadLibrary('aa')
local liba_instance = LibA({name='idevz',sex='man'})
print_r(liba_instance.sex)
```
*执行结果 `sex-->man`*

*代码释意：（类使用的时候需要注意，类的使用分为类文件的加载 `Load( 如这里的 LoadLibrary )` 和实例化 `LibA()`） 两个步骤*

* `local LibA = LoadLibrary('aa')` 载入类名为 `LibA` 的类
* `local liba_instance = LibA({name='idevz',sex='man'})` 传入表 `{name='idevz',sex='man'}` 对类进行相关的实例化
* `liba_instance.sex` 是对实例属性的引用

*注：载入和实例化也可以一步达成 `local liba_instance = LoadLibrary('aa')({name='idevz',sex='man'})`*


###*类继承*
*下面我么定义一个类 LibB，并使之集成于 LibA*

#####*类定义*

```lua
local LibB = Class("LibB", LoadLibrary('LibA'))

function LibB:__construct( data )
	local data = data or {name='kk', sex='xxx'}
	data.sex = data.sex .. '-->son'
	self.parent:__construct(data)
end

return LibB
```

*代码释意：*

* `Class("LibB", LoadLibrary('LibA'))` 声明一个 Vanilla 类，类名为 `LibB` 继承自类 `LibA`
* `self.parent:__construct(data)` 构造器中调用父类的构造器

#####*类使用*

```lua
local LibB = LoadLibrary('LibB')
local libb_instance = LibB({name='idevz',sex='man'})
print_r(libb_instance:idevzDo({doo='xxx'})['doo'])
```
*执行结果 `xxx`*

*代码释意：*

* `libb_instance:idevzDo` 调用父类的 `idevzDo` 方法