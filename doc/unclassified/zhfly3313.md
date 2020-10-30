# （CVE-2018-13056）Zzcms 8.3 任意文件删除

> 原文：[https://www.zhihuifly.com/t/topic/3313](https://www.zhihuifly.com/t/topic/3313)

# （CVE-2018-13056）Zzcms 8.3 任意文件删除

## 一、漏洞简介

## 二、漏洞影响

版本：Zzcms 8.3 user / del.php

## 三、复现过程

漏洞点出现在 `/user/del.php` 的55行到64行

![image](img/d7b66745490f153e5367eead83adafbe.png)

看到是从数据库中取出的img字段，然后直接判断了是不是默认值，然后检查文件存在，最后是删除，这其中并没有涉及到文件合法性检查。
那我们就可以去找一下这个字段是怎么进入数据库的：

![image](img/809d10da71b016ecb8efe0786428bde1.png)

首先找到表名，是`zzcms_main`表，然后找一下insert或者update操作，然后我们在`/user/zssave.php`中发现了有关操作。

![image](img/746fd87eaf4a45b7c847a5ce452d4c3e.png)

然后查看是否经过了过滤，因为经过了伪全局过滤，这里的`$img`并没有经过任何有效过滤。

![image](img/01418ab6fcf3f2d97a9203384a162d08.png)

所以向数据库中插入污点数据是很简单的，只要在发布招商信息的时候，抓包，修改掉img的数据即可，这里我们测试`/install/install.lock`：

![image](img/d24e57459e73e02a77196fe54caba412.png)

然后就可以到`/user/del.php`中去删除文件，这样就能触发删除操作。
但是这里有一个问题，就是删除的时候需要知道这一条记录在数据库中的id编号，可以采用爆破的方法来得到。

![image](img/16125d71e0d781c84d66e3d2aa9dd061.png)

发包以后，我们刷新页面就可以发现，安装的锁文件已经被成功删除：

![image](img/4f6ae26966f19739fae71364619c307f.png)

然后就可以配合之前的漏洞，任意文件写入拿到webshell。

## 参考链接

> https://www.anquanke.com/post/id/156660