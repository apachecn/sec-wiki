# Emlog phpinfo 泄漏

> 原文：[https://www.zhihuifly.com/t/topic/2909](https://www.zhihuifly.com/t/topic/2909)

# Emlog phpinfo 泄漏

## 一、漏洞简介

需要登陆（至少是网站的会员/作者权限）

## 二、漏洞影响

## 三、复现过程

首先看看漏洞出现的位置：

![image](img/4ade020ec4c4083f2982c90b7d6fc96b.png)

如上图，我们只要构造如下的URL：

```
http://www.0-sec.org:81/admin/index.php?action=phpinfo 
```

直接访问：

![image](img/6a4b454e205399c9a32be0d7fec4d409.png)

## 四、参考链接

> http://www.jeepxie.net/article/687123.html