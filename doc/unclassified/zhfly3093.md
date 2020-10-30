# Phpyun v5.0.1 后台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3093](https://www.zhihuifly.com/t/topic/3093)

# Phpyun v5.0.1 后台getshell

## 一、漏洞简介

## 二、漏洞影响

Phpyun v5.0.1

## 三、复现过程

安装好本地环境,看了下系统功能,等等测试,最后查看前台index.php源码。

![image](img/313b29663a84a3771e2e29568cebd664.png)

后台直接写入被过滤掉了。

![image](img/08a5b59434db3d772923c22d8406cde3.png)

又去翻了下后台功能,发下有个生成功能,并且没有后缀限制。

![image](img/99d9a222993e478e5681e47001187544.png)

生存成功,但是发现()被大写,使用经典的include包含,随意找了一个模板下的info.txt文件,写入执行代码。

```
<?php include'app/template/info.txt';？> 
```

![image](img/a14c74b1261da9ad06ce9abe4d13cbda.png)

成功执行代码
代码分析：

![image](img/a2f0429b1223637ed0bb3d515c424841.png)

对post的数据没有任何验证,直接代入

![image](img/d5163cecc5792a21a1ef333e1e306fe9.png)