# （CVE-2017-3506）Weblogic反序列化漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3222](https://www.zhihuifly.com/t/topic/3222)

# （CVE-2017-3506）Weblogic反序列化漏洞

## 一、漏洞简介

## 二、漏洞影响

Oracle WebLogic Server10.3.6.0.0

Oracle WebLogic Server12.1.3.0.0

Oracle WebLogic Server12.2.1.1.0

Oracle WebLogic Server12.2.1.2.0

## 三、复现过程

https://github.com/ianxtianxt/CVE-2017-3506

![image](img/ab2c68304d3076be10393b6cf23623b0.png)

```
[*]          漏洞编号  CVE-2017-3506 & CVE-2017-10271
[*]          漏洞详情  https://www.secfree.com/article-635.html
[*]          作者邮箱  Bearcat@secfree.com

[*] 使用方法:

[*]          单个检测  -u [http://Host](http://Host):Port

[*]          批量检测  -f url.txt

[*]          上传木马  -s [http://Host](http://Host):Port /wls-wsat/CoordinatorPortType11 shell.jsp

[*] wls-wsat组件路径：

```
 /wls-wsat/CoordinatorPortType
        /wls-wsat/CoordinatorPortType11
        /wls-wsat/ParticipantPortType
        /wls-wsat/ParticipantPortType11
        /wls-wsat/RegistrationPortTypeRPC
        /wls-wsat/RegistrationPortTypeRPC11
        /wls-wsat/RegistrationRequesterPortType
        /wls-wsat/RegistrationRequesterPortType11 
``` 
```