# UserAssist

> 原文：[https://www.zhihuifly.com/t/topic/3605](https://www.zhihuifly.com/t/topic/3605)

## UserAssist

> 跟踪在资源管理器中打开的可执行文件和完整路径，其中UserAssist保存了windows执行的程序的运行次数和上次执行日期和时间。

注册表位置：

**注**：记录实时更新，数据rot-13加密

```
当前用户：
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist `所有用户：

HKEY_USERS&lt;sid>\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist

`` 
```

![image](img/c455bf7a18c3a79f5d1c504347aab57c.png)

解析工具：https://www.nirsoft.net/utils/userassist_view.html

图形化界面

![image](img/23c687c584689f1d06420951d87b19fc.png)

命令行使用

```
UserAssistView.exe  /stext out.txt //保存文本格式
UserAssistView.exe  /shtml out.txt //保存html格式
UserAssistView.exe  /sxml out.txt  //保存xml格式 
```

![image](img/904c710fb1a60bc9674fe6ac4bd81f3f.png)