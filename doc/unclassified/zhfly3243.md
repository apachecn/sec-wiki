# WordPress Plugin - Quizlord 2.0 XSS

> 原文：[https://www.zhihuifly.com/t/topic/3243](https://www.zhihuifly.com/t/topic/3243)

# WordPress Plugin - Quizlord 2.0 XSS

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

首先搭建worepress，我的版本是4.4。然后进入后台下载插件Quizlord，版本是2.0。

![image](img/2cdc77b677d440725e606c99bcf11a85.png)

![image](img/bb1addbca003642e7ad5605818eafb80.png)

下载、安装完成后，需要点击启用插件。

![image](img/57af13b65e51e92e171bfa73e60082bf.png)

根据exploit-db给出的漏洞详情，找到触发漏洞的位置。

![image](img/dd0b90faef0b505b4d41ac2235986f62.png)

进入后台选择Quizlord插件

![image](img/d62ea1f408ccf9f79f36389272d04a64.png)

此时浏览器的地址栏正好对应poc中的referer内容，现在只要使用火狐插件hackbar并根据POC构造POST请求

![image](img/fdd4a2d7a3126a3dce97ba32569e516e.png)

点击execute即可发送该POST请求。

![image](img/7e50cb434385119fa3af223fdb639c5d.png)

请求成功后，返回是一个空白页。

![image](img/bc9bed4e4732913b97048a29dede34cc.png)

返回上一页并刷新即可触发该漏洞。

![image](img/cec2c11995fb5e874f74262edc387407.png)

通过复现这个漏洞，我们可以知道它属于后台存储型XSS，且它的危害其实并不是很大。

需要进入后台，因此必须得知道后台用户的账号和密码。

下面我们来简单分析一下漏洞的成因。

### 漏洞成因分析

WordPress插件源码位置：

```
\wp4_4\wp-content\plugins\ 
```

进入Quizlord插件目录，找到quizlord.php文件。

![image](img/25a5ef541606fb2d74c1ff7b8ca46777.png)

下图是quizlord.php文件的内容

![image](img/08a89dd7e26dbc596d36a9be1a9a95fd.png)

根据POC快速定位到函数。发现POST传入的数据并没有被程序过滤就写入了数据库中。

![image](img/ecbd990c494e13ff83b2a0ef1dbc5bc5.png)

## 四、参考链接

> https://www.freebuf.com/vuls/189814.html

> https://www.exploit-db.com/exploits/45307/