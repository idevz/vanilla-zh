#香草/Vanilla
*香草/Vanilla是一个基于Openresty实现的高性能Web应用开发框架.*
<p><a href="http://idevz.github.io/vanilla/"><img border="0" src="http://m1.sinaimg.cn/maxwidth.300/m1.sinaimg.cn/120d7329960e19cf073f264751e8d959_2043_2241.png" alt="LuaRocks" width="150px"></a></p>

##安装
```
yum install lua-devel luarocks
luarocks install https://raw.githubusercontent.com/idevz/vanilla/master/vanilla-0.0.1-1.rockspec
```

##使用
```
vanilla new app
cd app
vanilla start
```

##为什么需要Vanilla
回答这个问题，我们只需要看清楚Openresty和Vanilla各自做了什么即可。
####Openresty
* 提供了处理HTTP请求的基本解决方案
* 给Nginx模块开发开辟了一条全新的道路，我们可以使用Lua来处理Web请求
* 形成了一个日趋完善的生态，这个生态包括了高性能Web服务方方面面 

####Vanilla
* 基于Openresty开发，具备Openresty一切优良特性
* 屏蔽了Web业务开发者对Nginx
