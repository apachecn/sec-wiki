# MetInfo系统中一处旧插件导致的ssrf

> 原文：[https://www.zhihuifly.com/t/topic/3018](https://www.zhihuifly.com/t/topic/3018)

# MetInfo系统中隐藏的一处旧插件导致的ssrf

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

利用的话，ssrf嘛你可以扫描扫描端口咯。
我这里的话，简单说明利用一下

![image](img/c6b82af8c4565712a42443d480e790e6.png)

![image](img/1945163711b1b620d72452baac8f2bc2.png)

### 分析过程

文件：MetInfo_v6.1.0\app\app\ueditor\php\Uploader.class.php

![image](img/9bf5985ce73e862b69092bc04cbd0cd5.png)

get_headers这里是验证资源是否存在的，不存在就不走下面了，所以这里可以用来判断端口，例如81端口不存在那么他这里就会直接报错了
$this->stateInfo = $this->getStateInfo("ERROR_DEAD_LINK");

这个时候我还不清楚的$this->fileField 的值是向哪里获取的=-=
又搜索了一下。

![image](img/fb542b80caba53e9f888ff118548ac85.png)

需要注意的是：$type == "remote" 才能进入$this->saveRemote();流程引起漏洞触发。

搜索一下

![image](img/cbd99a5b1df6b7b550f6f454f42fbb29.png)

打开文件：MetInfo_v6.1.0\app\app\ueditor\php\action_crawler.php
打开文件以后，虽然引入了Uploader.class.php 但是没有引入$CONFIG所以直接调用代码会报错，那么就继续找引入

搜索一下

![image](img/0a8faeec1b096bb2ef1954eb9dc5eedd.png)

打开文件：MetInfo_v6.1.0\app\app\ueditor\php\controller.php

![image](img/d0ffcb7f955ff57bc55e13a7c217eb26.png)