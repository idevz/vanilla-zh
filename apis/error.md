##Vanilla 的错误处理
*Vanilla 的错误处理分为*

###*Bootstrap 即类 `application.bootstrap`*
*Bootstrap 的实现其实是一个名为 `application.bootstrap` 的 Vanilla 类，实现了构造器初始化的属性只有一个（当前请求所使用的 dispatcher），我们只需要关注根据需求实现各种 init 方法即可，最后只要在 `boot_list` 方法返回的列表中的 init 方法都会被顺序执行*