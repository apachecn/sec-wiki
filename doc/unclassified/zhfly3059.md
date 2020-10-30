# OpenSNS 后台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3059](https://www.zhihuifly.com/t/topic/3059)

# OpenSNS 后台getshell

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

### 1\. 打开网站后台找到模板上传位置：

http://www.0-sec.org/index.php?s=/admin/theme/add.html

![image](img/5374e4b3b475c230488e39760161d7c2.png)

### 2\. 配置一个压缩包，压缩包里是一句话木马：

![image](img/c6dae443e0b5366b1ff45e59795141bd.png)

### 3\. 选择上传：

![image](img/a66c08534eec94f402514b1c6e1de96e.png)

### 4、会在./Theme目录下自动生成刚刚上传好的马儿

```
http://www.0-sec.org/Theme/shell.php 
```

![image](img/9fb335dfd274f9e8fcfecac3be8209a2.png)