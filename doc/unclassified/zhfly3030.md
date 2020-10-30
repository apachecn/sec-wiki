# （CVE-2019-19886）ModSecurity 拒绝服务漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3030](https://www.zhihuifly.com/t/topic/3030)

# （CVE-2019-19886）ModSecurity 拒绝服务漏洞

## 一、漏洞简介

2020年1月20日，Trustwave SpiderLabs公开了其维护的开源WAF引擎ModSecurity的1个拒绝服务（DoS）漏洞，漏洞编号为：CVE-2019-19886。此漏洞影响ModSecurity的3.0到3.0.3版本。

## 二、漏洞影响

ModSecurity 3.0到3.0.3版本

## 三、复现过程

### 漏洞分析

通过Github项目的releases跟踪: https://github.com/SpiderLabs/ModSecurity/releases

![image](img/e0827f396255e21c386da66f4db6043c.png)

发现了airween的一个pr: https://github.com/SpiderLabs/ModSecurity/pull/2201

被官方采纳并归并到了 `v3/master`

![image](img/3265f34fb32b22e44f32600449c1f881.png)

来看看他究竟修改了什么: https://github.com/SpiderLabs/ModSecurity/pull/2201/commits/c7ad6c7613de3c1ac2ad1b4055f0ded22b522027

存在漏洞的代码如下：

![image](img/035b3b450a22a815109eb2289479bb84.png)

可以看见这里将Cookie的值带入代码，先以 `;` 符号进行分割，进入for循环，再以 `=` 符号进行分割赋值，而后使用正常数组下标的表达方式进入功能中，那么问题出在了哪里？

我们再来看修复的代码：

![image](img/6206225f288e3524b6f7f3ddd30e6e8b.png)

简单阅读代码和注释就能了解到，它就是处理两种非正常的Cookie值: `Cookie: ;;foo=bar` \ `Cookie: =bar;`

换句话说就是ModSecurity原来在处理Cookie内容的时候过于信任用户的输入，这也是安全的一个大忌讳: **一切用户的输入都是不可信的** ！

为什么会拒绝服务呢？可以理解为数组越界，类似:

![image](img/2f0d5bf3a31d492ea46ac50916881882.png)

### 漏洞复现

很明显，通过漏洞跟踪已经知道了漏洞攻击方式那就是构建恶意(畸形)的 `Cookie` 头

Curl命令测试: `curl -s -H 'Cookie: =test' 'http://test/'`

![image](img/3bc17f886421838474c63264133d1f17.png)

根据奇安信CERT的描述: `不断向服务器发送此类请求将使工作线程反复崩溃` ，那我们可以重复这个请求造成拒绝服务效果。

## 参考链接

> https://gh0st.cn/archives/2020-01-24/1