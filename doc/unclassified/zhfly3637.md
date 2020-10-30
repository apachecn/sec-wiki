# Burpsuite 伪造ip爆破脚本

> 原文：[https://www.zhihuifly.com/t/topic/3637](https://www.zhihuifly.com/t/topic/3637)

# Burpsuit 伪造ip爆破脚本

https://github.com/ianxtianxt/burpsuiefakeip

## BurpFakeIP

![image](img/d94e8b718d1bcde47ccadd76b5ecf0b3.png)

四个小功能

```
伪造指定ip
伪造本地ip
伪造随机ip
随机ip爆破 
```

### 0x01 伪造指定ip

在Repeater模块右键选择fakeIp菜单,然后点击inputIP功能,然后输入指定的ip

![image](img/ccf141e961bca71a06e14b7c0ef9e78a.png)

![image](img/742a4df72fb6daa9313a7c4a0d70c453.png)

程序会自动添加所有可伪造得字段到请求头中。

### 0x02 伪造本地ip

在Repeater模块右键选择fakeIp菜单,然后点击127.0.0.1功能：

![image](img/731b699fa79c404281ef1154bac56228.png)

### 0x03 伪造随机ip

在Repeater模块右键选择fakeIp菜单,然后点击randomIP功能：

![image](img/db02137cf3ae2f0d0a060df247f9e96e.png)

### 0x04 随机ip爆破

伪造随机ip爆破是本插件最核心的功能。

将数据包发送到Intruder模块,在Positions中切换Attack type为Pitchfork模式,选择好有效的伪造字段,以及需要爆破的字段:

![image](img/f2a11323035e9a28833cbba1b6957e20.png)

按照箭头顺序将Payload来源设置为Extensin-generated,并设置负载伪fakeIpPayloads,然后设置第二个变量。

![image](img/724bcc4e6be1b563ad9fce68eceb0004.png)

![image](img/e28c25c3231c5bab87340077a1262f93.png)

点击Start attack开始爆破.

![image](img/96247090400f3ea38320ad5c780b6166.png)

如上图,实现每次爆破都使用不同的伪ip进行,避免被ban。

> PS：伪造随机ip爆破的先决条件可以伪造ip绕过服务器限制。