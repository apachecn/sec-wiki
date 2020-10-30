# Powershell基础命令

> 原文：[https://www.zhihuifly.com/t/topic/3632](https://www.zhihuifly.com/t/topic/3632)

#powershell递归寻址注册表

```
$key = Get-Item HKLM:\Software\Microsoft\PowerShell\1
$key.Name
HKEY_LOCAL_MACHINE\Software\Microsoft\PowerShell\1 `$key | Format-List ps*` 
```