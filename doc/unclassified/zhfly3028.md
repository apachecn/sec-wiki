# MKCMS v6.2 任意用户密码找回漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3028](https://www.zhihuifly.com/t/topic/3028)

# MKCMS v6.2 任意用户密码找回漏洞

## 一、漏洞简介

## 二、漏洞影响

MKCMS v6.2

## 三、复现过程

任意用户密码找回
这个问题主要是`/ucenter/repass.php`代码里，找回密码的逻辑有问题，第10行查询到`username`、 `email`能对应上之后，14行就直接重置密码了。。。而且密码的范围在12行有写，只有90000种可能，重置之后，burp跑一下不就ok了？（当然要结合验证码重用才能有效爆破）

## 参考链接

> https://xz.aliyun.com/t/7580#toc-4