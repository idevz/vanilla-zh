##香草/Vanilla
*香草/Vanilla是一个基于Openresty实现的高性能Web应用开发框架.*

![Vanilla](http://m1.sinaimg.cn/maxwidth.300/m1.sinaimg.cn/120d7329960e19cf073f264751e8d959_2043_2241.png)

### *邮件列表*
- vanilla-en <vanilla-en@googlegroups.com>
- vanilla-devel <vanilla-devel@googlegroups.com>
- vanilla中文邮件列表 <vanilla@googlegroups.com>

### *推荐始终使用最新版的Vanilla*
*当前Vanilla最新版本0.1.0.rc5，支持命令：*

- vanilla-0.1.0.rc5（*你没看错，自0.1.0.rc5起，vanilla的命令行和框架代码都带着版本号，方便多版本共存，也方便框架升级*）
- v-console-0.1.0.rc5

### *安装*

####*Vanilla-V0.1.0-rc4.1 及其之前版本的配置和使用，请查阅 [README-V0.1.0-rc4.1.md](install/0_1_rc4.md)*

#### *通过 ./setup-framework 脚本安装 Vanilla*

*`./setup-framework` 脚本是对 `make install` 后面安装方式一个自动化封装，仅需要指定 OpenResty 的安装路径，即可简单安装 Vanilla*

```
./setup-framework -h
Usage: ./setup-framework
                 -h   show this help info
                 -v   VANILLA_PROJ_ROOT, vanilla project root, will contain vanilla framework and apps
                 -o   OPENRESTY_ROOT, openresty install path(openresty root)
```

`./setup-framework` 脚本参数选项说明：

- -v ： 指定 Vanilla 项目的根目录，此选项默认为 `/data/vanilla`，默认则将 Vanilla 安装到 `/data/vanilla/framework/0_1_0_rc5/vanilla/` 目录下。

- -o ： 指定 OpenResty 的安装目录， 此选项默认为 `/usr/local/openresty`， 如果你的 OpenResty 安装路径与此不同，则需要指定为正真的 OpenResty 安装目录。

安装目录结构如下：

```
tree /data/vanilla -L 2
/data/vanilla
├── framework
│   ├── 0_1_0_rc5
│   └── 0_1_0_rc5.old_2016_04_12_11_04_18 # 重复安装则会将之前的老版本按照时间自动备份
```

#### *通过 ```make install``` 安装 Vanilla*

Vanilla 支持的选项都提供了默认值，如果你的环境与默认值不一样，请configure时指定成你自己的。

特别注意选项```--openresty-path```，默认为```/usr/local/openresty```，请确保设置正确。

可以在源码目录下执行```configure --help```来查看安装选项的使用方法。

下面是一个简单的安装示例：

```
./configure --prefix=/usr/local/vanilla --openresty-path=/usr/local/openresty

make install （不需要make，直接make install）
```


### Vanilla 使用

####*Vanilla-V0.1.0-rc4.1 及其之前版本的配置和使用，请查阅[README-zh-V0.1.0-rc4.1.md](README/README-zh-V0.1.0-rc4.1.md)*

#### *Vanilla命令*

*Vanilla-V0.1.0-rc5 目前依旧提供两个命令， 但是 rc5 以后提供的命令与安装的框架代码一样，都自动携带版本号，```vanilla-0.1.0.rc5```，和 ```v-console-0.1.0.rc5``` 这样做的好处在于方便多版本共存以及 Vanilla 的无痛升级*
- ```vanilla-0.1.0.rc5```用来初始化应用骨架，而 `vanilla-0.1.0.rc5` 之后，服务的停启不再通过 `vanilla-0.1.0.rc5` 命令管理，而是通过项目路径下面的 `va-appname-service` 脚本进行管理，使用细节见后面说明。
- ```v-console-0.1.0.rc5``` 是一个交互式命令行，主要提供一种方便学习Lua入门的工具，可以使用一些 vanilla 开发环境下的包，比如table输出的 lprint_r 方法等。

#### *创建应用*
```
vanilla-0.1.0.rc5 new app_full_path							#使用 vanilla-0.1.0.rc5 命令 自动生成应用骨架，注意这里需要传递应用的全路径，而不只是一个APP_NAME
chmod +x app_full_path/va-appname-service					#给生成的项目骨架根目录下面的 va-appname-service 脚本添加执行权限
app_full_path/va-appname-service initconf [dev]				#初始化应用的nginx配置文件，此文件基于 app_full_path/nginx_conf 下面的配置文件生成，如果有特殊选项需要配置，可以先修改 app_full_path/nginx_conf 下面的配置文件，再进行 initconf 操作，[dev] 参数为可选项， 加上则表示执行开发环境相关操作，不加则默认为生产环境
app_full_path/va-appname-service start [dev]				#启动生成的服务，即可通过http://localhost访问服务，[dev] 参数如上。
```
上面创建应用的过程也可以通过脚本 `./setup-vanilal-demoapp` 简单自动完成：
```
./setup-vanilal-demoapp -h
Usage: ./setup-vanilal-demoapp -h   show this help info
                 -a   VANILLA_APP_ROOT, app absolute path(which path to init app)
                 -u   VANILLA_APP_USER, user to run app
                 -g   VANILLA_APP_GROUP, user group to run app
                 -e   VANILLA_RUNNING_ENV, app running environment
```
`./setup-vanilal-demoapp` 脚本参数选项说明：
- -a ： 指定初始化应用的全路径（绝对路径），默认为 /data/vanilla/vademo
- -u ： 指定运行服务的用户名，默认为 idevz
- -g ： 指定运行服务的用户组，默认为 sina
- -e ： 指定运行服务的环境，默认为'' 指生产环境

安装目录结构如下：

```
tree /data/vanilla/ -L 1
/data/vanilla/
├── framework 							# vanilla 框架安装目录
├── vademo 								# 应用初始化目录
└── vademo.old_2016_04_12_11_04_26 		# 重复安装后会将之前版本按照时间进行备份
```

#### *应用配置初始化与服务管理*
*通过脚本 /data/vanilla/vademo/va-vademo-service 管理 vademo 服务*

```
/data/vanilla/vademo/va-vademo-service -h
Usage: ./va-ok-service {start|stop|restart|reload|force-reload|confinit[-f]|configtest} [dev] #dev 指定了运行的环境，不加 dev 则默认为生产环境。
```

注意：* 如果没有使用 ./setup-vanilal-demoapp 脚本，而是手动 new 的 app， 则在启动服务之前需要先运行 /data/vanilla/vademo/va-vademo-service initconf [dev] 对相应环境的 nginx 配置文件进行初始化。  *

#### *代码目录结构*
```
 /Users/zj-git/app_name/ tree ./
./
├── application（应用代码主体目录）
│   ├── bootstrap.lua（应用初始化 / 可选<以下未标可选为必选>）
│   ├── controllers(应用业务代码主体目录)
│   │   ├── error.lua（应用业务错误处理，处理本路径下相应业务报错）
│   │   └── index.lua（hello world示例）
│   ├── library（应用本地类包）
│   ├── models（应用数据处理类）
│   │   ├── dao（数据层业务处理）
│   │   │   └── table.lua
│   │   └── service（服务化业务处理，对DAO的再次封装）
│   │       └── user.lua
│   ├── nginx（*Openresty所封装Nginx请求处理各Phase）
│   │   └── init.lua（*init_by_lua示例）
│   ├── plugins（插件目录）
│   └── views（视图层，与controllers一一对应）
│       ├── error（错误模板）
│       │   └── error.html
│       └── index（index controller模板）
│           └── index.html
├── config（应用配置目录）
│   ├── application.lua（应用基础配置 / 路由器、初始化等设置）
│   ├── errors.lua（应用错误信息配置）
│   ├── nginx.conf（nginx配置文件模板）
│   ├── nginx.lua（服务各种运行环境配置 / 是否开启lua_code_cache等）
│   ├── waf-regs（应用防火墙规则配置目录）
│   │   ├── args
│   │   ├── cookie
│   │   ├── post
│   │   ├── url
│   │   ├── user-agent
│   │   └── whiteurl
│   └── waf.lua（服务防火墙配置）
├── logs（日志目录）
│   └── hack（攻击日志目录 / 保持可写权限）
├── pub（应用Nginx配置根路径）
    └── index.lua（应用请求入口）
```
#### *业务代码示例 IndexController*
```
local IndexController = {}

function IndexController:index()
    local view = self:getView()
    local p = {}
    p['vanilla'] = 'Welcome To Vanilla...'
    p['zhoujing'] = 'Power by Openresty'
    view:assign(p)
    return view:display()
end

return IndexController
```
#### *模板示例 views/index/index.html*
```
<!DOCTYPE html>
<html>
<body>
  <img src="http://m1.sinaimg.cn/maxwidth.300/m1.sinaimg.cn/120d7329960e19cf073f264751e8d959_2043_2241.png">
  <h1><a href = 'https://github.com/idevz/vanilla'>{{vanilla}}</a></h1><h5>{{zhoujing}}</h5>
</body>
</html>
```

### 为什么需要Vanilla
回答这个问题，我们只需要看清楚Openresty和Vanilla各自做了什么即可。
#### *Openresty*

- 提供了处理HTTP请求的全套整体解决方案
- 给Nginx模块开发开辟了一条全新的道路，我们可以使用Lua来处理Web请求
- 形成了一个日趋完善的生态，这个生态涵盖了高性能Web服务方方面面 

#### *Vanilla*
- 使复杂的Nginx配置对Web业务开发者更透明化
- 开发者不再需要了解Openresty的实现细节，而更关注业务本身
- 实现了Web开发常规的调试，错误处理，异常捕获
- 实现了请求的完整处理流程和插件机制，支持路由协议、模板引擎的配置化
- 整合、封装了一系列Web开发常用的工具集、类库（cookie、应用防火墙等）
- 实现了自动化、配置化的Nginx指令集管理
- 更合理的利用Openresty封装的8个处理请求Phase
- 支持不同运行环境（开发、测试、上线）服务的自动化配置和运行管理
- 功能使用方便易于扩展
- 基于Openresty开发，具备Openresty一切优良特性

### 社区组织
#### *QQ群&&微信公众号*
- *Openresty/Vanilla开发QQ群：205773855（专题讨论Vanilla相关话题）*
- *Openresty 技术交流QQ群：34782325（讨论OpenResty和各种高级技术）*
- *Vanilla开发微信公众号:Vanilla-OpenResty(Vanilla相关资讯、文档推送)*

![vanilla](va_c.jpeg)

[![QQ](http://pub.idqqimg.com/wpa/images/group.png)](http://shang.qq.com/wpa/qunwpa?idkey=673157ee0f0207ce2fb305d15999225c5aa967e88913dfd651a8cf59e18fd459)