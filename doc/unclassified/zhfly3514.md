# Logon Scripts

> 原文：[https://www.zhihuifly.com/t/topic/3514](https://www.zhihuifly.com/t/topic/3514)

## Logon Scripts

`Logon Scripts`优先于av先执行，我们可以利用这一点来绕过av的敏感操作拦截

注册表路径为：`HKEY_CURRENT_USER\Environment`，创建一个键为：`UserInitMprLogonScript`，其键值为我们要启动的程序路径

![image](img/7cf9d4b7e18fc56e48dcf730903b3293.png)

效果如下：

![image](img/5adecb351dcf218c6a5d32092bb303fd.png)