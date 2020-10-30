# 映像劫持

> 原文：[https://www.zhihuifly.com/t/topic/3510](https://www.zhihuifly.com/t/topic/3510)

## 映像劫持

这个和shift后门差不多，只不过在低版本的windows中，我们可以简单地替换程序，但是在高版本的windows版本中替换的文件受到了系统的保护，所以这里我们要使用另外一个知识点：映像劫持。

"映像劫持"，也被称为"IFEO"（Image File Execution Options）

```
就是Image File Execution Options（其实应该称为"image Hijack"。）是为一些在默认系统环境中运行时可能引发错误的程序执行体提供特殊的环境设定。由于这个项主要是用来调试程序用的，对一般用户意义不大。默认是只有管理员和local system有权读写修改。
PS：来自百度百科 
```

简单来说就是当目标程序被映像劫持时，当我们启动目标程序时，启动的是劫持后的程序而不是原来的程序

操作也很简单，在注册表的`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Option`下添加一个项`sethc.exe`，然后在`sethc.exe`这个项中添加`debugger`键，键值为我们恶意程序的路径，如下图

![image](img/1ca19381b9b05b82e2159e3abb154a12.png)

效果如下

![image](img/44b2e7075dcb951be15797445adb5a1a.png)