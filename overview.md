##说明
香草/vanilla 的实现大致分为以下三个部分，本手册将会分别进行详述。
1. 系统层面关于Nginx指令集的封装、应用防火墙的实现等（vanilla/sys）
2. 框架层面关于请求处理的各种类包封装（vanilla/v）
3. 框架单元测试相关封装（vanilla/spec）

###系统层面（Vanilla关于Nginx控制的抽象和封装）
```
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