### *基于 OpenResty 安装 Luarocks*

*基于 OpenResty 来安装 其实意在基于 OpenResty 自带的 Luajit 来安装 Luarocks， Luarocks 安装时需要指定 lua 目录和 lua 的 include 目录，而 OpenResty 自身带有的 Luajit 就包含所需的 Lua 解释器和头文件。*
*只不过 Luarocks 安装需要的是 Lua 而不是 Luajit，这就是关键的一步*

```bash
# 我本机的 OpenResty 安装路径为 /usr/local/openresty-1.11.2.1/
# OpenResty 本身带的 Lua 解释器 Luajit 在 /usr/local/openresty-1.11.2.1/luajit/bin/ 路径
# OpenResty 本身带的 Lua 相关头文件 include 在 /usr/local/openresty-1.11.2.1/luajit/include/ 路径
# 我们需要做的就是建一个软连接，给 Luarocks 使用

cd /usr/local/openresty-1.11.2.1/luajit/bin
sudo ln -sf luajit-2.1.0-beta2 lua

cd /usr/local/openresty-1.11.2.1/luajit/include
sudo ln -sf luajit-2.1 lua5.1
```

*经过以上几步的准备工作，就可以进入正常的安装步骤，演示如下：*

```bash
./configure --with-lua-bin=/usr/local/openresty-1.11.2.1/luajit/bin --with-lua-include=/usr/local/openresty-1.11.2.1/luajit/include --prefix=/usr/local/luarocks-2.4.1
sudo make build
sudo make install
```

*安装完毕后，只需要将 Luarocks 安装路径的 bin 目录（/usr/local/luarocks-2.4.1/bin）加到 PATH 环境变量下，即可使用 `luarocks` 命令来管理标准 Lua 包，使用 Luarocks 安装的包将保存在 Luarocks 安装的 share 目录，而可执行文件（比如 Vanilla 的可执行命令 vanilla 和 v-console）都在 luarocks 命令同级目录。*