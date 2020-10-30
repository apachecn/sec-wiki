# 利用BITS

> 原文：[https://www.zhihuifly.com/t/topic/3525](https://www.zhihuifly.com/t/topic/3525)

## 利用BITS

`BITS` (后台智能传送服务) 是一个 Windows 组件，它可以在前台或后台异步传输文件，为保证其他网络应用程序获得响应而调整传输速度，并在重新启动计算机或重新建立网络连接之后自动恢复文件传输。

`bitsadmin`是一个命令行工具，用于创建下载或上传任务并监视其进度。你可以执行`bitsadmin /?`或`bitsadmin /HELP`获取帮助列表。

常见的`bitsadmin`命令

```
bitsadmin /create [type] DisplayName //创建一个任务
bitsadmin /cancel <Job> //删除一个任务
bitsadmin /list /allusers /verbose //列出所有任务
bitsadmin /AddFile <Job> <RemoteURL> <LocalName> //给任务test添加一个下载文件
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters] //设置在任务完成传输时或任务进入状态时将运行的命令行命令。
bitsadmin /Resume <Job> //激活传输队列中的新任务或挂起的任务。
bitsadmin /cancel <Job> //删除某个任务
bitsadmin /reset /allusers //删除所有任务
bitsadmin /complete <Job> //完成某个任务 
```

下面我们来测试一下：

```
bitsadmin /create test
bitsadmin /addfile test c:\windows\system32\calc.exe c:\Users\qiyou\Desktop\calc.exe //为了方便起见我们直接复制本地文件
bitsadmin /SetNotifyCmdLine test cmd.exe "cmd.exe /c calc.exe"
bitsadmin /resume test 
```

效果如下：

![image](img/796dd4b5d42a118b998c81fc8d2cd7e3.png)

重启电脑之后任务还是存在

![image](img/5940273798440231291091717a269f63.png)

重启电脑之后任务会再一次被激活，大概几分钟之后我们的命令会再次执行（由于时间太长了就不录制gif了）

![image](img/29cfbd65d05fba0ff88ec4346f2d5f32.png)

如果我们想让任务完成，可以执行`bitsadmin /complete test`，`calc.exe`也会复制到桌面上

![image](img/5baa67b10ee76e84d0c763c9a40c13d8.png)