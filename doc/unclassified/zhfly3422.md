# 4.2.1.4 PsTools

> 原文：[https://www.zhihuifly.com/t/topic/3422](https://www.zhihuifly.com/t/topic/3422)

#### 4.2.1.4 PsTools

PsTools 是 sysinternals 开发的一个功能强大的安全管理工具包，最新版目前共有 13 个 各种功能的小工具。这里介绍下 psexec，psexec 是一款远程执行命令的工具，在内网渗透 的时候常它来进行横向移动。 下载地址:https://docs.microsoft.com/zh-cn/sysinternals/downloads/psexec

![image](img/b068aabce299afec9246b67acf7924cb.png)

##### PsExec

###### 1、 若已建立 IPC$连接，无需再次输入账号密码，所有 pstools 工具可直接使用 net use //查看 ipc 连接状态

![image](img/1b141e3febfc768e2a841e43148cedbb.png)

![image](img/f2af72959758df6e9f253e15aaf23df2.png)

![image](img/3e858816d6f790e7ca1aec360f9f0cf7.png)

###### 2、 若未建立 IPC 连接，则要使用账号密码进行登录

```
PsExec.exe \\192.168.19.178 -u adtest\jeff1 -p test@123 cmd.exe 
```

![image](img/c83d766b90095908f24f36dc73a668e3.png)