# IOS 端微信取证

> 原文：[https://www.zhihuifly.com/t/topic/3624](https://www.zhihuifly.com/t/topic/3624)

# IOS 端微信取证

*   1，通过备份获取数据库文件

> 使用iTunes备份整机数据

![image](img/2993b847babeca8685159368dbe93b49.png)

> 在文件夹中查找备份文件

> 文件路径在隐藏文件下，所以先显示隐藏文件 Command + Shift + .

> 路径为

```
/Users/username/资源库/Application Support/MobileSync/Backup/ 
```

![image](img/0ddae190b48f9177a47a591a4a94e519.png)

在文件夹里查找Manifest.db

![image](img/10faa7fc53d5329d8e041d2fd1bbb511.png)

> Manifest.db里面记录了各个 app 备份文件的存放状况，但数据太多，这里用sql语句查询下

![image](img/07da5c65eb425a441905af14967c6069.png)

> MM.sqlite就是微信的主要数据库

> 用fileID搜索文件

![image](img/3cf708fee1137361edecec07f507d514.png)

> 将文件复制出来，并把后缀改为db

*   2，读取数据

> 因为iOS微信数据库未加密，这里我们使用 DB Browser for SQLite MAC版直接读取

![image](img/9ebb3cef70709aef7df370e65e6829ce.png)

> 成功读到微信聊天记录