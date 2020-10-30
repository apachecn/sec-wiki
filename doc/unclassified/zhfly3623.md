# MAC OS 端微信取证

> 原文：[https://www.zhihuifly.com/t/topic/3623](https://www.zhihuifly.com/t/topic/3623)

# MAC OS 端微信取证

*   1，找到微信数据库文件

```
~/Library/Containers/com.tencent.xinWeChat/Data/Library/Application\ Support/com.tencent.xinWeChat/*/*/Message/*.db 
```

![image](img/f7c71d51c0647d15a4d56ac5d4326221.png)

*   2，从内存中读取密钥

> 打开Mac版微信

> 在终端下下输入 lldb -p $(pgrep WeChat) 进入 lldb 的子shell界面（LLDB是mac下的调试工具）

![image](img/38b302c0f49b4d641a1e34a886afe7ef.png)

> 再输入br set -n sqlite3_key和c

![image](img/a7a30ce34d76ef3e2771a2aa5bbf6021.png)

> 此时登录微信，可能会卡死，但是不影响操作

> 然后在终端输入memory read --size 1 --format x --count 32 $rsi 获取密钥

![image](img/d0c351df37a8148adb5898c255696283.png)

> 处理多余文本

```
f0d402d2800e40e1
b43bb6a0853ed86c
e6351ce21a0447ce
a2f2e81cad1dc3c3 
```

> 这个64位的字符串就是密钥

*   3，读取数据

> 因为微信使用的是sqlite数据库，这里我们使用 DB Browser for SQLite MAC版进行读取

![image](img/79070598f7b1661bd76b2369cb263448.png)

> 选择SQLCipher 3 defaults 密码类型选择为raw key 在密码框输入0x后再输入之前的密钥共64位进行解密

![image](img/80353a260f115d49a3e9f65b0629a163.png)

> 成功获取到聊天记录