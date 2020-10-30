# 计划任务

> 原文：[https://www.zhihuifly.com/t/topic/3620](https://www.zhihuifly.com/t/topic/3620)

### 计划任务

计划任务往往是一个值得关注的点也是一个容易忽略的点。站在攻击的角度来讲，计划任务可以持久化后门、执行指定恶意程序等等；站在防御、取证的角度来讲，计划任务执行之后，系统是不会自动删除的（重启或者注销也是不会删除的），这就可能导致入侵者会忽略把计划任务清除，这就有利于我们的取证

列出所有任务

```
cmd：
schtasks.exe `powershell：

Get-ScheduledTask` 
```

根据文件夹列出所有任务
powershell:

```
Get-ScheduledTask -TaskPath "\" 
```

根据任务名查看任务的详情信息

```
schtasks.exe /query /v /tn test /fo list 
```

![image](img/476436ee7177a534e338c507272afb65.png)