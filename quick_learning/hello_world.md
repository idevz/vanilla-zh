##Vanilla 的安装

### 准备安装

1. 安装好 OpenResty
2. Vanilla Github 地址：https://github.com/idevz/vanilla


### 安装

```bash
# 1.git clone 最新 Vanilla 版本（或者下载相应的 Vanilla release 版本）
git clone https://github.com/idevz/vanilla.git

# 2. 切换到vanilla文件夹
cd vanilla

# 3.编译vanilla： ./setup-framework -v $VANILLA_PROJ_ROOT -o $OPENRESTY_ROOT 其中 $VANILLA_PROJ_ROOT 为vanilla框架安装目录。 -o 为openresty 安装目录

./setup-framework -v /application/vanilla -o /application/openresty

```

*经过这3不如果没有报错，则安装vanilla成功*


### 创建vanilla项目

```bash
#1. 创建 vanilla 的运行用户 

useradd -s /sbin/nologin -M nginx

id nginx # 可以查看到创建的用户

# 2、创建vanilla项目, -a 为 项目路径，-u 为执行用户 -g 为用户组

./setup-vanilla-demoapp -a /home/webserver/cms -u nginx -g nginx

# 3、删掉默认nginx服务 

pkill -9 nginx

# 4、切换到项目文件夹 编辑项目配置文件，改成你要的

cd /home/webserver/cms
cd nginx_conf
vim va-nginx.conf
vim va-nginx-development.conf 

# 5、同步配置文件到运行目录

./va-cms-service initconf dev -f #开发模式
./va-cms-service initconf -f #生产模式


# 6、启动项目（2选1）

./va-cms-service start dev  # 启动开发模式
./va-cms-service start  # 启动生产模式
```

*服务启动后，开发环境默认启动在 9110 端口，http://localhost:9110 即可访问*

### vanilla常用命令

1. 启动项目： `./va-cms-service start` 或者 `./va-orcms-service start dev`

2. 重启项目  `./va-cms-service restart` 或者 `./va-orcms-service restart dev`

3. 停止项目： `./va-cms-service stop` 或者 `./va-orcms-service stop dev`

4. 创建配置文件 `./va-cms-service initconf dev -f`