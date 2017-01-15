##框架说明
香草/vanilla 的实现大致分为以下三个部分，本手册将会分别进行详述。
1. 系统层面关于Nginx指令集的封装、应用防火墙的实现等（vanilla/sys）
2. 框架层面关于请求处理的各种类包封装（vanilla/v）
3. 框架单元测试相关封装（vanilla/spec）

###系统层面（Vanilla关于Nginx控制的抽象和封装/vanilla/sys）
```
├── vanilla
│   ├── sys
│   │   ├── application.lua（生成项目骨架）
│   │   ├── config.lua（项目系统环境配置）
│   │   ├── nginx
│   │   │   ├── config.lua（Nginx配置文件解析，包括框架和应用两部分Nginx配置的合并）
│   │   │   ├── directive.lua（Nginx配置指令封装）
│   │   │   └── handle.lua（Nginx服务管理，start、stop）
│   │   ├── vanilla.lua（Vanilla Nginx服务管理/启动，关闭等配置自动生成和销毁）
│   │   └── waf
│   │       ├── acc.lua（应用防火墙实现）
│   │       └── config.lua（应用防火墙配置）
```

###框架层面（Vanilla对Nginx请求的处理/vanilla/v）
```
├── vanilla
│   └── v
│       ├── application.lua（application封装，标记一个应用）
│       ├── bootstrap.lua（application初始化）
│       ├── controller.lua（vanilla业务处理封装）
│       ├── dispatcher.lua（vanilla请求处理分发器）
│       ├── error.lua（vanilla系统错误实现）
│       ├── libs（Web开发类包）
│       │   ├── cookie.lua
│       │   ├── http.lua
│       │   ├── logs.lua
│       │   ├── session.lua
│       │   ├── shcache.lua
│       │   └── utils.lua
│       ├── plugin.lua（vanilla插件实现）
│       ├── registry.lua（vanilla数据寄存器）
│       ├── request.lua（vanilla请求封装）
│       ├── response.lua（vanilla响应封装）
│       ├── routes（vanilla实现的路由协议簇，目前仅支持简单路由，路由协议实现了URL到controller、action的规则化映射）
│       │   └── simple.lua（简单路由）
│       ├── view.lua（vanilla视图层）
│       └── views（vanilla支持的模板引擎）
│           └── rtpl.lua（vanilla目前默认的模板引擎resty.template）
```

###单元测试
vanilla使用busted作为单元测试框架
#####*[Busted](http://olivinelabs.com/busted/)*
######add

### 社区组织
#### *QQ群&&微信公众号*
- *Openresty/Vanilla 开发 1 群：205773855（已满）*
- *Openresty/Vanilla 开发 2 群：419191655*
- *Openresty 技术交流 1 群：34782325（已满）*
- *Openresty 技术交流 2 群：481213820（已满）*
- *Openresty 技术交流 3 群：124613000*
- *Vanilla开发微信公众号:Vanilla-OpenResty(Vanilla相关资讯、文档推送)*

![vanilla](va_c.jpeg)

[![QQ](http://pub.idqqimg.com/wpa/images/group.png)](http://shang.qq.com/wpa/qunwpa?idkey=673157ee0f0207ce2fb305d15999225c5aa967e88913dfd651a8cf59e18fd459)