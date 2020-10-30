# （CVE-2019-16773）WordPress 5.3.0 储存型xss

> 原文：[https://www.zhihuifly.com/t/topic/3268](https://www.zhihuifly.com/t/topic/3268)

# （CVE-2019-16773）WordPress 5.3.0 储存型xss

## 一、漏洞简介

WordPress是WordPress基金会的一套使用PHP语言开发的博客平台。该平台支持在PHP和MySQL的服务器上架设个人博客网站。 WordPress 3.7版本至5.3.0版本中存在跨站脚本漏洞。该漏洞源于WEB应用缺少对客户端数据的正确验证。攻击者可利用该漏洞执行客户端代码。

## 二、漏洞影响

WordPress 3.7版本至5.3.0

## 三、复现过程

1.  如果论坛用户的个人资料显示在其个人资料页面中，则此方法有效。以管理员身份登录并转到外观->自定义，然后进入BuddyPress Nouveu->成员首页，并确保启用了显示用户个人资料的功能

2.  创建普通论坛用户帐户

3.  登录并编辑您的个人资料。将用户描述修改为一下代码

    ```
    <a href="#" title=" target='abc' rel= onmouseover=alert(/XSS/) ">This is a PoC for a Stored XSS</a> 
    ```

4.  访问您的配置文件并将鼠标悬停在链接上。

## 参考链接

> https://hackerone.com/reports/509930