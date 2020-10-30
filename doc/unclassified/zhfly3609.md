# Background Activity Moderator (BAM)

> 原文：[https://www.zhihuifly.com/t/topic/3609](https://www.zhihuifly.com/t/topic/3609)

## Background Activity Moderator (BAM)

> BAM是一个控制后台应用程序活动的Windows服务，该服务存在于windows10 version 1709及以后版本中

注册表路径为：

**注**：记录实时更新，数据无加密

```
HKLM\SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID} 
```

记录包含了程序路径和上次执行日期和时间，其中执行日期键值类型为`FILETIME(64bit little Endian)`

![image](img/0ff914f0b758597b88cfa1403bd81b2c.png)

执行时间提取：以`winrar`为例子

![image](img/484017cc1bd354efd7308b0a89088476.png)

把`filetime`转化为`datetime`

```
from __future__ import division
import struct
import sys
from binascii import unhexlify
from datetime import datetime, timedelta

nt_timestamp = struct.unpack("<Q", unhexlify(“dc14dd91be7cd501”))[0]

epoch = datetime(1601, 1, 1, 0, 0, 0)

nt_datetime = epoch + timedelta(microseconds=nt_timestamp / 10) `print(nt_datetime.strftime("%Y/%m/%d %H:%M:%S"))` 
```

output：`2019/10/07 03:23:21`