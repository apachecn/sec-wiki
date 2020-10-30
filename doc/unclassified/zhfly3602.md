# Program-Telemetry Event Log

> 原文：[https://www.zhihuifly.com/t/topic/3602](https://www.zhihuifly.com/t/topic/3602)

## Program-Telemetry Event Log

> win7及以上存在，在应用程序启动时为应用程序处理应用程序兼容性缓存请求

文件夹中的位置：`C:\Windows\System32\winevt\Logs\Microsoft-Windows-Application-Experience%4Program-Telemetry.evtx`

![image](img/f11e1c692ad7c0b1ab27fc2735a35a58.png)

在Windows事件查看器的位置：`Applications and Services Logs\Microsoft\Application-Experience\Program-Telemetry`，如图

![image](img/ae66153c224d14545051358b0b638ee9.png)

日志获取：

```
wevtutil qe /f:text Microsoft-Windows-Application-Experience/Program-Telemetry 
```

Event IDs：500/505

![image](img/bd1d77a89fbde6e121edfbcccdc4daf8.png)