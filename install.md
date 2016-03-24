##安装

*vanilla-0.1.0.rc5 与 vanilla-0.1.0.rc4 在安装方面并没有太大的区别，只是为了使用和升级更加方便，为了解开版本与项目的耦合带来的一系列问题，vanilla-0.1.0.rc5 在安装后以添加版本号的方式来组织命令和框架代码。这些做法来自于新浪移动事业部线上业务的真实实践，可以预见后面的 vanilla 版本也将采用此种组织形式。*

### *当前最新版的 vanilla-0.1.0.rc5 安装过程如下*

#### 直接configure、make install

*这其实是个很简单的过程，不用多说，直接看命令行*

```
wget https://github.com/idevz/vanilla/archive/V0.1.0-rc5.0.tar.gz
tar zxf V0.1.0-rc5.0.tar.gz
cd vanilla-0.1.0.rc5
./configure --prefix=/{VANILLA_ROOT}/framework --openresty-path=/{OPENRESTY_PATH} ##注：框架与vatest安装到同级目录不是必须的，这么做只是为了方便管理和升级Vanilla相关的项目
sudo make install
```

*注：关于目录结构的组织，我们推荐[这种](intro/index_structure.md)，即：将 Vanilla 相关的内容都放到 `{VANILLA_ROOT}` 下面，框架如：`/data1/vanilla/framework` ，Vanilla 项目如：`/data1/vanilla/va-app-1/`等，这样方便项目统一管理，也方便框架升级。

#### 依然支持Luarocks

*vanilla-0.1.0.rc5 的安装依然支持Luarocks*

```
luarocks install vanilla
```

*注：不建议使用这种方式，因为Vanilla目前没有针对此种方法做多版本支持，依旧在期待春哥的 OPM*


### 社区组织
#### *QQ群&&微信公众号*
- *Openresty/Vanilla开发QQ群：205773855（专题讨论Vanilla相关话题）*
- *Openresty 技术交流QQ群：34782325（讨论OpenResty和各种高级技术）*
- *Vanilla开发微信公众号:Vanilla-OpenResty(Vanilla相关资讯、文档推送)*

![vanilla](va_c.jpeg)

[![QQ](http://pub.idqqimg.com/wpa/images/group.png)](http://shang.qq.com/wpa/qunwpa?idkey=673157ee0f0207ce2fb305d15999225c5aa967e88913dfd651a8cf59e18fd459)