# 注册表自启动项

> 原文：[https://www.zhihuifly.com/t/topic/3511](https://www.zhihuifly.com/t/topic/3511)

## 注册表自启动项

`MSF`的`Persistence`模块利用的就是写注册表自启动项来实现的，一般自启动项是这两个键：`Run`和`RunOnce`，两者的区别如下

1.  Run：该项下的键值即为开机启动项，每一次随着开机而启动。
2.  RunOnce：RunOnce和Run差不多，唯一的区别就是RunOnce的键值只作用一次，执行完毕后就会自动删除

常见注册表启动项键的位置：

用户级

```
\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce 
```

系统级

```
\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run
\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnce 
```

修改一下：

![image](img/21aa878c7099c3da3a3823afd4743d09.png)

执行结果：

![image](img/11955af77f0724fd052f13cf5524a051.png)