# QCMS 3.0 留言板xss

> 原文：[https://www.zhihuifly.com/t/topic/3097](https://www.zhihuifly.com/t/topic/3097)

# QCMS 3.0 留言板xss

## 一、漏洞简介

## 二、漏洞影响

QCMS 3.0

## 三、复现过程

![image](img/f96e3f01a048466bb2a08a57bc4092a0.png)

按照如图所示构造payload

![image](img/af52186fc6f6775cedc86e9f2ab840ce.png)

提交之后无需审核，直接先弹个窗。。

![image](img/933dc61cb360e475483c00e6a2190ac2.png)

登录后台再弹一个。。

![image](img/382d8f58a75722214d194980b570e4e0.png)

查看数据库，没有过滤直接插入

## 参考链接

> https://xz.aliyun.com/t/7269