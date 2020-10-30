# Emlog 6.0 后台暴力破解

> 原文：[https://www.zhihuifly.com/t/topic/2912](https://www.zhihuifly.com/t/topic/2912)

# Emlog 6.0 后台暴力破解

## 一、漏洞简介

Emlog博客系统默认后台登陆地址为http://域名/admin/login.php而后台登陆时，错误情况下，验证码未刷新，导致可暴力破解登陆管理员账号低危漏洞，但是在emlog5.3.1和6.0测试版本均存在 ...

## 二、漏洞影响

emlog5.3.1和6.0

## 三、复现过程

![image](img/df7c7f94763de29cf493f2fb9fdf23c7.png)

访问

```
http://0-sec.org:81/admin/ 
```

![image](img/487fe951e5d4eefb811fb8b2f0069c99.png)

已知管理员用户名为：admin（可在前端文章页寻找作者用户名）

![image](img/7906aa5e0320fc35ea113c058c1a0e3c.png)

登陆后台

![image](img/8e2e17391e3410b47cdabfdd1852a5bb.png)

随便输入admin admin123 qdiwx，点击登陆

然后burpsuite抓包

![image](img/a7c17757ddbef107c5092a69a072f3a3.png)

CTRL+I尝试暴力破解：

![image](img/527367e8d51b246db0042122360a62d8.png)

![image](img/789aac49d41f6da62b27c1a052f02e5b.png)

![image](img/3df5d95f5cfa9b15a0283bb17c281cb2.png)

成功爆破出密码，所以再次验证：验证码没消除会话，导致可暴力破解漏洞的存在

## 四、参考链接

> http://www.dyboy.cn/post-900.html