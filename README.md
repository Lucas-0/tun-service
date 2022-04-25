# tun-service
tunnel service over clash

## why
因为觉得 CFW 太臃肿，但又想要 `tun mode`，所以称不上移植，嫁接了一下。实际上做的是手动把 CFW 开启 tun 的步骤实现。

## 过程
1. 从 CFW 复制需要的文件

    CFW 开启 `service mode`，会在 `home directory` 下的 `clash` 文件夹生成一个 service 文件夹。把生成的文件夹放进 CDN 的对应位置。

    CFW 在这一步做的事情是在系统里注册了一个名叫 `Clash Core Service` 的服务。实际上我们不需要注册这个服务，当然注册了可以删除，或者去注册表里修改可执行文件的路径。

我推荐直接解压 CFW 的 7z 压缩包，从里面翻出 `clash-core-service.exe` 和 `service.exe`，对照本 repo，手动创建 `service.yml` 和 `service.wrapper.log`。把这四个文件放进 service 文件夹再放到 CDN 的对应位置。至于 `yml` 和 `log` 文件对于正常实现功能而言是不是必要的，笔者没有试过。

2. 注册服务

以管理员权限运行如下命令。
```
sc.exe create <clash-core> binpath= "$HOME\clash\service\service.exe" type= own start= auto displayname= <clash-core>
```
这行命令在系统中创建名叫 `clash-core` 的服务，`binpath` 是 `service.exe` 的路径，注意把 `binpath` 中的 `$HOME` 对应替换。<>中的名字自己决定。

完成这一步应该能在`计算机管理->服务和应用程序->服务`中看到<clash-core>服务在运行。

3. 开启 tun mode

把 wintun.dll 或是 copy 或是下载到 CDN 的 `$HOME\clash`。在 mixin 中加入对应配置，这部分简单且有充分的教程，无庸赘述。

在管理员模式下启动 CDN，开启 mixin。正常情况下可以在设备管理器中的网络适配器内看到 `clash tunnel`。
