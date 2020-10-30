# XDCMS 1.0 sql注入漏洞（一）

> 原文：[https://www.zhihuifly.com/t/topic/3272](https://www.zhihuifly.com/t/topic/3272)

# XDCMS 1.0 sql注入漏洞（一）

## 一、漏洞简介

## 二、漏洞影响

XDCMS 1.0

## 三、复现过程

注入存在于用户登录页面：`/index.php?m=member&f=login`

![image](img/038063a5883f75fa3f5f756800544a18.png)

漏洞文件:`/modules/member/index.php`，`lines:112`

![image](img/466aff386a0c39728153a59e8e4f910a.png)

```
login_save()`在用户登录界面时调用，URL:`/index.php?m=member&f=login 
```

参数m与f的包含方式为:`/ modules/$m/$c.php`

> index.php -> system/common.inc.php -> fun.inc.php -> global.inc.php[接受m、f参数的值] -> 包含modules/$m/$c.php

$username值使用了`safe_html()`进行过滤，且过滤字符均可使用大小写绕过

htmlspecialchars()未设置第二个参数，导致仅对双引号”进行转义，单引号’不会被转义掉，因而存在注入

> 第二个参数详解：
> 
> ENT_COMPAT（默认值）：只转换双引号。
> 
> ENT_QUOTES：两种引号都转换。
> 
> ENT_NOQUOTES：两种引号都不转换。

![image](img/1365992e420be550541c80862b1480b9.png)

但此处注入由于过滤了`.`，无法通过information_schema来获取表名，需去猜测，较为鸡肋

![image](img/8d24b952b16f3a6ac386e71faf1bae89.png)