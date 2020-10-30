# 通达oa 越权访问

> 原文：[https://www.zhihuifly.com/t/topic/3339](https://www.zhihuifly.com/t/topic/3339)

# 通达oa 越权访问

## 一、漏洞简介

## 二、漏洞影响

2013、2015版本

## 三、复现过程

将get型访问转换成post,并且post参数_SERVER,即可越权访问admin才能访问的⻚面。根据⽹上的通达 OA的源码找这些敏感地址,如: /general/system/database/

![image](img/8f0937fa6705ce9a292bfc75ad284ae2.png)

![image](img/0c130469d6a4b83cc1b61a81d7d2df93.png)

![image](img/babc442f6bc8bbd1da50262b4390b1a7.png)

根据源码,几乎所有敏感的⻚面都可以使用这种方式进行越权访问,⽐如说设置⻆色权限的⻚面啊什么的。这个⻚面就已经可以XX了!