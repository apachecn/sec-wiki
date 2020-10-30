# Finecms 5.0.10 多重漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2941](https://www.zhihuifly.com/t/topic/2941)

# Finecms 5.0.10 任意文件上传漏洞

## 一、漏洞简介

## 二、漏洞影响

Finecms 5.0.10

## 三、复现过程

用十六进制编辑器写一个有一句话的图片 去网站注册一个账号，然后到上传头像的地方。 抓包，把jepg的改成php发包。

![image](img/1894f295ee549fe7c39210af6209207c.png)

可以看到文件已经上传到到`/uploadfile/member/用户ID/0x0.php`

![image](img/621ee3f539bf14f2192c88c1af7cc9e5.png)