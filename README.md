#香草/Vanilla
*香草/Vanilla是一个基于Openresty实现的高性能Web应用开发框架.*
<p><a href="http://idevz.github.io/vanilla/"><img border="0" src="http://m1.sinaimg.cn/maxwidth.300/m1.sinaimg.cn/120d7329960e19cf073f264751e8d959_2043_2241.png" alt="LuaRocks" width="150px"></a></p>

##安装说明
1. Vanilla使用luarocks管理包依赖
2. 本身也已经发布到Lua包管理平台Luarocks，需要先安装Luarocks
3. Vanilla运行在Openresty而Openresty使用Luajit加速，Luajit使用Lua5.1的ABI，所以需要Lua5.1支持。

#####*安装示例/Linux*
```
yum install lua-devel luarocks  -- 需要安装Lua开发版
luarocks install vanilla
```
#####*MacOSX平台*
```
wget lua5.1(lua5.1 源码地址)
源码安装lua5.1
wget luarocks（luaro源码地址）
源码安装luarocks
luarocks install vanilla
```
#####*建议：*


##使用
```
vanilla new app
cd app
vanilla start
```

##为什么需要Vanilla
回答这个问题，我们只需要看清楚Openresty和Vanilla各自做了什么即可。
#####*Openresty*
* 提供了处理HTTP请求的全套整体解决方案
* 给Nginx模块开发开辟了一条全新的道路，我们可以使用Lua来处理Web请求
* 形成了一个日趋完善的生态，这个生态包括了高性能Web服务方方面面 

#####*Vanilla*
* 基于Openresty开发，具备Openresty一切优良特性
* 实现了自动化、配置化的Nginx指令集管理
* 更合理的利用Openresty封装的8个请求处理Phase
* 支持不同运行环境（开发、测试、上线）服务的自动化配置和运行管理
* 使复杂的Nginx配置对Web业务开发者更透明化
* 开发者不再需要了解Openresty的实现细节
* 实现了Web开发常规的调试，错误处理，异常捕获
* 实现了请求的完整处理流程和插件机制，支持路由协议、模板引擎的配置化
* 整合、封装了一系列Web开发常用的工具集、类库（cookie、应用防火墙等）
* 功能使用方便易于扩展
