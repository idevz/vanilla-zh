##Vanilla 的 Hello World
*Vanilla 的起步非常简单， 根据README 文档中的步骤在环境中部署完框架后，使用 setup-vanilal-demoapp 脚本即可快速初始化一个项目.*

```bash
git clone https://github.com/idevz/vanilla.git
cd vanilla
./setup-framework -v $MCODE/www/vanilla -o /usr/local/openresty-1.11.2.1
```

###setup-framework 部署框架核心
#####*-v 参数指定 Vanilla 框架部署路径*
*-v 所指定的路径下面会自动生成 freamwork 目录，里面会按照日期来区分不同 Vanilla 版本（前提是如果安装了多个版本 Vanilla）*

#####*-o 参数指定 OpenResty 安装路径*
*-o 参数必须指定 OpenResty 的安装路径，因为 Vanilla 的多项目支持依赖于 OpenResty 服务的统一管理，比如使用指定的 nginx.conf 文件， 已经统一的配置存放目录等*

###setup-vanilal-demoapp 初始化应用项目
#####*setup-vanilal-demoapp -a 参数指定项目绝对路径*
*-a 必须指定为开发项目的绝对路径，多项目的加载是基于根路径的绝对路径加载*

###va-appname-service 服务管理脚本
#####*服务配置初始化*
*setup-vanilal-demoapp 命令自动生成的初始化项目根路径下有一个 nginx_conf 路径，这个路径下存放着默认的当前项目所需要的配置文件，有开发环境和生产环境两个部分，包括 nginx.conf 以及项目所使用的 server 配置（vhost 路径下），首先，应该执行 `va-appname-service initconf` 命令，将这些配置文件初始化到系统 OpenResty 环境的相关 nginx 配置文件位置（根据前面所指定的 OpenResty 安装根路径来进行操作，如果有自定义需求，可先修改默认生成的 nginx_conf 配置，再执行 initconf）*
#####*服务启动*
*应用运行所需的配置文件 `initconf` 后，可以通过 `va-appname-service start|stop|restart...\` 等命令来很方便的控制服务的停起，或者重载等*
