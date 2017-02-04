##Vanilla 的 调试
*除了查看 nginx 错误日志辅助开发外，为了方便 Vanilla 项目的开发和调试，Vanilla 提供了诸如 `print_r` 之类的对象输出方法，以及详细友好的页面报错输出，你不需要到服务器日志去查看，就能所见即所得的开发调试代码.*

###sprint_r，print_r，lprint_r，err_log

#####*sprint_r*
*将 LUA 对象等格式化为易读的字符串返回*

#####*print_r*
*类似 `ngx.say` 的效果，将对象、变量等以易读的格式进行输出，适用于 Vanilla 开发的 Web 服务*

#####*lprint_r*
*`print_r` 的 CLI 版本，适用于 v-console 命令行环境*

```bash
╰─○ v-console-0.1.0.rc6
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
v-console>a={}
v-console>a.v1='a_v1'
v-console>a.v2='a_v2'
v-console>lprint_r(a)
{
  v2 = "a_v2",
  v1 = "a_v1"
}
v-console>
```

#####*err_log*
*err_log 方法是对 `ngx.ERR` 的封装，将 `msg` 记录到 nginx 错误日志*