# ShimCache (AppCompatCache)

> 原文：[https://www.zhihuifly.com/t/topic/3604](https://www.zhihuifly.com/t/topic/3604)

## ShimCache (AppCompatCache)

> 用来识别应用程序兼容性问题。缓存数据跟踪文件路径、大小、上次修改时间和是否被执行（取决于操作系统），Windows XP的ShimCache被限制为96条记录，此后所有版本最多保留1024条记录。

**注**：虽然记录在`ShimCache`中的存在但是并不能100%证明文件是被执行过的，但它确实显示了与文件交互的窗口

注册表位置：

**注**：数据加密，重启之后更新

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache 
```

![image](img/6e94ce7424a3aa0dd3647d90dea86c5d.png)

解析工具（C#）：

```
https://github.com/ianxtianxt/AppCompatCacheParser 
```

例子
以csv格式保存的指定路径

```
AppCompatCacheParser.exe --csv . 
```

按上次修改时间排序

```
AppCompatCacheParser.exe --csv . -t 
```

![image](img/cf2cd4998e6c6ce241ff852593633efb.png)

如果我们想离线分析，我们可以用这个项目：https://github.com/ianxtianxt/ShimCacheParser

导出注册表

```
reg export "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache" ShimCache.reg 
```

解析

```
python ShimCacheParser.py -o test.csv -r ShimCache.reg -t 
```

![image](img/abab494a6669b14847f2ed063449081c.png)

结果

![image](img/0399c784c7d3ffc168a95232d94fb5ac.png)