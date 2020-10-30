# 百家cms v4.1.4 远程命令执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3327](https://www.zhihuifly.com/t/topic/3327)

# 百家cms v4.1.4 远程命令执行漏洞

## 一、漏洞简介

## 二、漏洞影响

百家cms v4.1.4

## 三、复现过程

```
# 需要后台权限
http://www.0-sec.org/index.php?mod=site&act=weixin&do=setting&beid=1 
```

首先需要在设置里将图片缩放打开

![image](img/4ebfb35ad0f2ba4d8cabb1180da1bdfd.png)

本地创建`&命令&.txt`格式的文件

![image](img/009a3204a36e5168970f2f673838985d.png)

访问payload，并进行上传

![image](img/0dcc8783ef240a04ec3114ce3e3a2191.png)

命令执行

![image](img/5a36eb8c247236edbf7aa37d361d5ee6.png)

## 参考链接

> https://xz.aliyun.com/t/7542