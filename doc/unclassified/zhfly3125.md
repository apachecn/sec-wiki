# ShopXO v1.8.0 后台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3125](https://www.zhihuifly.com/t/topic/3125)

# ShopXO v1.8.0 后台getshell

## 一、漏洞简介

## 二、漏洞影响

ShopXO 小于v1.8.0

## 三、复现过程

默认后台密码
admin shopxo

![image](img/28738c4a7ee0b1abe5140e39fd01587d.png)

![image](img/b3f0c59f8d6178213a4048d8463f147d.png)

登入后台-》应用中心-》应用商店-》主题

随便下载一个主题

![image](img/787626195d098c25af702648d488e079.png)

![image](img/d62d273d623b8a61cf302696aa7ce681.png)

然后把下载下来的压缩包解压出来 把shell放入static目录

![image](img/e9362046fa47118076e61e3e15fe7871.png)

回到网站后台
网站管理-》主题管理-》安装主题

![image](img/37ad380f88bdd07d13e40bddd69e968a.png)

shell地址

```
http://www.0-sec.org/static/index/default/shell.php 
```

> public是运行目录！！！