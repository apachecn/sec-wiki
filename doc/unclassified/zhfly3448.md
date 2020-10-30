# frp

> 原文：[https://www.zhihuifly.com/t/topic/3448](https://www.zhihuifly.com/t/topic/3448)

## frp内网穿透

### 测试环境：

阿里云公网主机：ip:47.xx.xx.xx
内网主机 windows
域名 （有没有都可以）

### 测试流程

先分别在公网和内网主机上下载frp的对应版本
地址为https://github.com/fatedier/frp/releases
linux直接wget或者直接下载好拖进去即可

#### 1.

在linux文件目录里修改frps.ini配置文件

![image](img/01dd9f1b3f9d627aef213f559c99116d.png)
第一个端口为链接服务器的端口
第二个端口为访问web的端口

#### 2.

修改windows文件目录里的frpc.ini文件

![image](img/fae1c6d7f1b7c784a7b40d719c9d6954.png)
对应端口对着填就行了

#### 3.

完事之后访问公网ip+web端口 如图

![image](img/20ac8864c7145a16aacf8d7a85e66890.png)

### 排坑指南

1.我是阿里云的服务器，在阿里云控制台 开放了端口之后 依然不通 后来想到我装的有宝塔，然后吧宝塔的防火墙策略也添加了规则就行了

2.custom_domains =这一项 也可以填域名，但是如果填域名的话，测试的时候 比如说你填的带www 访问的时候就得带www