# （CVE-2018-1305）Tomcat 安全绕过漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3200](https://www.zhihuifly.com/t/topic/3200)

# （CVE-2018-1305）Tomcat 安全绕过漏洞

## 一、漏洞简介

近日，Apache发布安全公告称Apache Tomcat 7、8、9多个版本存在安全绕过漏洞。攻击者可以利用这个问题，绕过某些安全限制来执行未经授权的操作，这可能有助于进一步攻击。

Apache Tomcat servlet 注释定义的安全约束，只在servlet加载后才应用一次。由于以这种方式定义的安全约束，应用于URL模式及该点下任何URL，很可能取决于servlet加载的次序，这可能会将资源暴露给未经授权访问它们的用户。

## 二、漏洞影响

Apache Tomcat < 9.0.5

Apache Tomcat < 8.5.28

Apache Tomcat < 8.0.50

Apache Tomcat < 7.0.85

## 三、复现过程

Java EE 提供了类似 ACL 权限检查的ServletSecurity注解，可以用于修饰Servlet对其进行保护，如果有两个servlet，Servlet1，访问路径为“/servlet1/*”并且添加了ServletSecurity注解，Servlet2，访问路径为“/servlet1/servlet2/*”但没有ServletSecurity注解，首次访问servlet1/servlet2，servlet1的ServletSecurity注解并不会生效，无法保护“/servlet1/servlet2”路径，因此可能会导致未授权访问。

如果在访问“/servlet1/servlet2”之前先访问过“/servlet1”，Tomcat会加载ACL并启动对“/servlet1/servlet2”的保护，则漏洞不会触发。

![image](img/f44434184c1dac62385c94aba9e5b76d.png)

在Servlet1前加上ServletSecurity注解，Servlet2无此注解。

![image](img/27abdb3d0e6725819739acc840604b18.png)

将web.xml文件中servlet相对应的url-pattern修改为如下图所示后。

![image](img/63461844e47e55441cc81cace9793b04.png)

运行该项目。首次访问servlet2的URL，发现可以未授权访问，针对servlet1的ACL并未生效。

http://localhost:8080/CVE-2018-1305/servlet1/servlet2/

![image](img/2fa1d0fb7fe08ee2a87c460b1f86efd9.png)

第二次访问servlet1的URL，访问被禁止，此时ACL生效。

http://localhost:8080/CVE-2018-1305/servlet1/

![image](img/dc2ab4a9e0c573c9a41184b8bb0fe500.png)

再次访问servlet2的URL，发现访问被禁止，如果ACL对servlet2生效，必须建立在servlet1被访问过的前提下。

http://localhost:8080/CVE-2018-1305/servlet1/servlet2/

![image](img/108ea10b89643ed35921bc4ab8188a46.png)

### 漏洞演示

![image](img/4190a5c7c05c72ff1e3492cf60ceb339.png)

因此漏洞的复现仅限于Tomcat启动后，在访问“/servlet1/*”之前先访问了“/servlet1/servlet2/*”页面，若之前存在任何人访问“/servlet1/*”页面，则漏洞不会触发。漏洞危害较高，但是利用条件困难，因此影响范围并不大。

## 参考链接

> http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-1305

> http://blog.nsfocus.net/cve-2018-130-handling/

> https://www.anquanke.com/post/id/99213