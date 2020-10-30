# Zzzcms 1.75 后台任意文件读取

> 原文：[https://www.zhihuifly.com/t/topic/3323](https://www.zhihuifly.com/t/topic/3323)

# Zzzcms 1.75 后台任意文件读取

## 一、漏洞简介

*   管理员权限
*   后台管理目录
*   后台数据库为mysql

## 二、漏洞影响

Zzzcms 1.75

## 三、复现过程

### 任意文件读取（一）

![image](img/10d39819cd1fcde890ee2630ad943ef5.png)

首先来看防护规则，不允许出现./

![image](img/758503c7b9cb730a2551df5074fc0b2b.png)

看 safe_path 只能是upload template runtime路径下的

![image](img/fb75d2d6ce32bc511c0185415f26fc6e.png)

所以构造/runtime/..\config/zzz_config.php 即可绕过防护

### 任意文件读取（二）

![image](img/c02296887ee0a767858a7363a0a21fb7.png)

![image](img/012a5ccabc3975d0b9fa1ad0f6c2b1fe.png)

首先来看restore函数，mysql数据库，发现path是可控的，看955行，跟进到load_file函数

![image](img/5852501612ca98d221c20be60a8710b7.png)

在zzz_file.php文件中，如果存在该path,则通过file_get_contents读取

![image](img/cbf5b6ea2ecb1b2b244736725f664ade.png)

然后现在的想法是如何输入出来，跟进到db_exec()函数

在zzz_db.php中，看str_log把sql语句写入到了log中

![image](img/a966f566704e2212005f3f7df711c86f.png)

在zzz.file.php中，跟进到str_log文件，看到文件的命名规则，

![image](img/f15e815c894a40187c1c952d8480f751.png)

文件命名规则为当天时间的时间戳+数据库用户+数据库密码，并且是未授权访问

![image](img/58966fce33db664b00bdb3f943f4d294.png)