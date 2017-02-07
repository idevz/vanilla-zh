##安装

*vanilla-0.1.0.rc5 与 vanilla-0.1.0.rc4 在安装方面并没有太大的区别，只是为了使用和升级更加方便，为了解开版本与项目的耦合带来的一系列问题，vanilla-0.1.0.rc5 在安装后以添加版本号的方式来组织命令和框架代码。这些做法来自于新浪移动事业部线上业务的真实实践，可以预见后面的 vanilla 版本也将采用此种组织形式。*

### *你可以使用以下两种方法来安装 vanilla*

- *configure make install*

- *Luarocks install vanilla*

### vanilla 安装 FAQ

#### vanilla 就是一堆Lua文件，不需要编译，为什么要用 `configure make install` 的形式？

##升级

*vanilla-0.1.0.rc5是vanilla-0.1.0.rc4在新浪移动全线推广过程中针对一些工程化部署问题改进版本，可以肯定的一点vanilla-0.1.0.rc4在功能、扩展性、使用方面表现良好，非常适合移动业务的场景，更不止于API，但是在集群部署、多App部署、Vanilla框架升级等方面vanilla-0.1.0.rc4存在短板，而vanilla-0.1.0.rc5极大程度的弥补了这些短板。*

### *当前Vanilla最新版本vanilla-0.1.0.rc5*

### ChangeLog

#### Vanilla-0.1.0.rc5

1.通过对安装的命令和框架代码添加上版本号，给多版本共存和版本升级提供了方便

2.去除在业务启动时自动生成nginx配置文件的操作，而将nginx的配置在生成项目骨架的时候默认生成

3.在项目骨架中提供专门的`va-{{vanilla app name}}-service`脚本来管理项目服务的启动和停止，废除原来不太方便集群部署的`vanilla start`

4.多项目部署，各自项目使用独立的nginx_vhost配置（仅包含server段和该项目需要使用的share_dic、init_by_lua*等配置）

#### Vanilla-0.1.0.rc4



### 社区组织
#### *QQ群&&微信公众号*
- *Openresty/Vanilla 开发 1 群：205773855*
- *Openresty/Vanilla 开发 2 群：419191655*
- *Openresty 技术交流 1 群：34782325*
- *Openresty 技术交流 2 群：481213820*
- *Openresty 技术交流 3 群：124613000*
- *Vanilla开发微信公众号:Vanilla-OpenResty(Vanilla相关资讯、文档推送)*

![vanilla](va_c.jpeg)

[![QQ](http://pub.idqqimg.com/wpa/images/group.png)](http://shang.qq.com/wpa/qunwpa?idkey=673157ee0f0207ce2fb305d15999225c5aa967e88913dfd651a8cf59e18fd459)