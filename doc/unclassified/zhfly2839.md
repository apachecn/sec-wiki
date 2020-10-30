# CatfishCMS 4.6.15 前台xss

> 原文：[https://www.zhihuifly.com/t/topic/2839](https://www.zhihuifly.com/t/topic/2839)

# CatfishCMS 4.6.15 前台xss

## 一、漏洞简介

## 二、漏洞影响

CatfishCMS 4.6

## 三、复现过程

### 代码分析

url：

```
http://0-sec.org/cms/CatfishCMS-4.6.12/index.php/index/Index/pinglun 
```

文件：application/index/controller/Index.php

方法：pinglun(

![image](img/abafa410c23139b9f2a98a35f3b7d2d4.png)

文件：application\index\controller\Common.php

过滤函数：filterJs()

![image](img/17f6657cc10cda561045f44b41730df6.png)

### 漏洞复现

首先注册一个用户

![image](img/b5f387a13cd3fd1dcf5ff34c645646ee.png)

![image](img/2b20abb285e4ae076f2d51ae2927b91e.png)

![image](img/17005d76e29aee8679f8f346419e5b3e.png)

![image](img/5448fb3408263a4f87aeb317e96c73e9.png)