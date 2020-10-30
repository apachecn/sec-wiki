# JYmusic 1.x 版本 前台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3002](https://www.zhihuifly.com/t/topic/3002)

# JYmusic 1.x 版本 前台getshell

## 一、漏洞简介

## 二、漏洞影响

1.x版本

## 三、复现过程

访问前台，注册一个用

注册成功后点击右上角设置，个人资料，头像设置

抓包，修改文件后缀为.php

![image](img/9f45fb27f1ea5f3fbd77bfcae7484479.png)

![image](img/3c659f546afdce57e9da4ef5b60ac6eb.png)

上传成功后访问个人中心，这里头像已经换了，审查元素查看头像的文件路径

![image](img/41c05620de3f1fb8d68ca104520e3503.png)

访问http://0-sec.org/Uploads/Avatars/uid_2/128.php

![image](img/3ee982b8a1df2931e0b1cf25ab40b524.png)

成功解析，通过这种办法上传一个phpinfo

![image](img/56424e607c264b4d14b01d87f1bfba4e.png)

访问文件

![image](img/ffbaa4374f42b2b0742d083f3a982f17.png)