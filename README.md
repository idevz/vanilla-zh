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

## 特性

- 提供很多优良组件诸如：bootstrap、 router、 controllers、 models、 views。
- 强劲的插件体系。
- 多 Application 部署。
- 多版本框架共存，支持便捷的框架升级。
- 一键 nginx 配置、 应用部署。
- 便捷的服务批量管理。
- 你只需关注自身业务逻辑。

## 安装

##### *Vanilla-V0.1.0-rc4.1 或之前版本的 Vanilla 安装请参见 ： [README-V0.1.0-rc4.1.md](README/README-zh-V0.1.0-rc4.1.md)*

``` bash
$ ./setup-framework -v $VANILLA_PROJ_ROOT -o $OPENRESTY_ROOT        #运行 ./setup-framework -h 查看更多参数细节
```

## 快速开始

**部署你的第一个Vanilla Application**

``` bash
$ ./setup-vanilal-demoapp  [-a $VANILLA_APP_ROOT -u $VANILLA_APP_USER -g $VANILLA_APP_GROUP -e $VANILLA_RUNNING_ENV]    #运行 ./setup-vanilal-demoapp -h 查看更多参数细节
```

**启动你的 Vanilla 服务**

``` bash
$ ./$VANILLA_APP_ROOT/va-appname-service start
```

## 更多信息

- 参见 [文档](https://idevz.gitbooks.io/vanilla-zh/content/index.html)


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