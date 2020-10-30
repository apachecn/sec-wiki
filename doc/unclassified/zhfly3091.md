# Phpyun v4.2（部分） 4.3 4.5 系统重装漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3091](https://www.zhihuifly.com/t/topic/3091)

# Phpyun v4.2（部分） 4.3 4.5 系统重装漏洞

## 一、漏洞简介

## 二、漏洞影响

经测试该漏洞影响从4.3到 4.5 所有版本，4.2部分版本受影响，4.2最终版本不受影响。具体情况请自行测试。

## 三、复现过程

### 漏洞分析

看到install 文件夹里的index.php，这里分php5,php7两种情况进行调用安装。

以php5为例。

文件 根目录/install/php5/install.php 代码中：

先判断了是否存在lock文件，存在即退出安装。

![image](img/1963bb3ecd666308d2f7fef89cec6007.png)

其中S_ROOT这个常量是在前面index.php文件中定义的。

![image](img/99e228cb41a5c1f2926d861ff8bc15f6.png)

取得是当前文件的绝对路径。拼接起来，检测的lock文件位置应该是 根目录/install/data/phpyun.lock。

这里没什么问题。

按照正常安装走完，看到最后一步

![image](img/7696773473862eec5d54f6ab611f2163.png)

创建lock文件，这里用的是相对路径。install.php是被index.php 用require的模式调用的。

取得路径应该是 根目录/install/，按照上图的路径创造的lock文件应该是放至于 根目录/data/phpyun.lock。

**创建的lock文件路径是 根目录/data/phpyun.lock，检测的路径却是 根目录/install/data/phpyun.lock**

那么一个重装的安全隐患就埋下了。

当用户安装完成之后，是可以被无限重装的，因为这个路径错误问题。

以本地phpyun4.3 已经安装完成系统为例，是可以被重装的。

![image](img/d7ed0a6ac24881b540a59f676be379b2.png)

最新版phpyun 4.5这里的代码和4.3是一样的。

![image](img/02fb244ce4fcb85b7e42d0faf53fe529.png)

phpyun 4.2 版本处理逻辑不一样，这个版本不受影响。

![image](img/d4b13e3d792842ce4b06ee7a8e6df9dc.png)

![image](img/599ef3a39c42f85b3caeebabb3062baf.png)

**经测试phpyun 4.2某些版本依旧是受影响的。**

### 版本测试

网上一些系统：

#### 官方测试站，版本phpyun 4.2111：

![image](img/1f636b5d77dea99527d3e052e7b736e8.png)

#### 某招聘网，版本phpyun 4.3

![image](img/a27fb2f21aed672b79efb473fa53a376.png)

## 参考链接

> https://www.cnblogs.com/r00tuser/p/8533517.html