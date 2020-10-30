# （CVE-2019-16131）OKLite v1.2.25 任意文件上传漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3056](https://www.zhihuifly.com/t/topic/3056)

# （CVE-2019-16131）OKLite v1.2.25 任意文件上传漏洞

## 一、漏洞简介

Ethan发现OKLite v1.2.25 存在任意文件上传漏洞（需要登录后台）

漏洞有两处，分别为：module_control.php；plugin_control.php

## 二、漏洞影响

OKLite v1.2.25

## 三、复现过程

### module_control.php

在模块管理管理处导入模块，这里只能上传zip文件。把恶意文件放在zip压缩包中上传：

(我这里是包含了一个shell.php，内容是phpinfo)

![image](img/9a1b4d20a6f4082362d27fbe961344b6.png)

![image](img/12b0c36fc40be51115bacfe2c7349b5c.png)

上传后又访问了另一个地址：

![image](img/cf89751762cfcb17ca5fa6b299e59b44.png)

上传之后就可以在`data\cache`看到shell.php被解压出来了。

![image](img/3e274ec5e18908c37604307388e0505c.png)

### plugin_control.php

和前一个module_control有所不同的是，这里的zip文件夹必须是包含一个文件夹，文件夹中再包含恶意文件。

插件中心：

![image](img/74b3d6c71fce5d69c382f8830b4610e0.png)

验证方式同前一个，就不重复了。

上传完成之后会访问另一个地址：

```
http://localhost/admin.php?c=plugin&f=unzip&id=1196&_noCache=0.08112707662168439&_=1570257969464 
```

## 参考链接

> https://xz.aliyun.com/t/6515#toc-6