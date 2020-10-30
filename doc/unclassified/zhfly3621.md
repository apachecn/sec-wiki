# Windows 端微信取证

> 原文：[https://www.zhihuifly.com/t/topic/3621](https://www.zhihuifly.com/t/topic/3621)

# Windows 端微信取证

*   1，获取聊天记录数据库文件

> 一般在C:\Users\Administrator\Documents\WeChat Files********\Msg
> **********为微信id

> MicroMsg.db就是聊天记录文件

![image](img/e6a096e4dbfa7a2faf8a00fc6dd401dd.png)

*   2，从内存中读取密钥

> 打开OllyDBG然后打开微信，使用OllyDBG附加微信登录进程

![image](img/a6cba84cc628fcba5328d3e7ca5c7ae5.png)

> 在菜单中选择可执行模块

![image](img/2b53ae2dcb98bce5e3a67b21596a5f38.png)

> 从中找到wechatwin模块

![image](img/10f6a2b8633265569db7f9f8812aa034.png)

> 在插件中选择搜索ASCII

![image](img/f2288ee92e09e67265f37da12029ac98.png)

> 在中文字符串里面查找DBFactory::encryptDB

![image](img/a1388c10f4cc614e149bd0bc7bb23f98.png)

> 查找到后选择

![image](img/c67cc4fd6eaaa7d891ec04d9ac0f9037.png)

> 双击进去，会定位到所在位置

![image](img/344d8e550626faa4da7f2d32b0f7869f.png)

> 向下查找TEST EDX,EDX，就是用来对比密码的汇编语言

![image](img/f77eac7ffa3ba3effa887e6a980dd912.png)

> 双击设置断点，会被标红

![image](img/cb9f253ea20311fe482a9aa08c2430c3.png)

> 然后运行，并登录微信，等待寄存器里edx的值不全为0

> 右键edx数据，选择数据窗口跟随

![image](img/e928cf7f778be3272a366497798fe6ed.png)

> 跟随后的数据前32位就是密钥

![image](img/490e7e43fd805fa994ca141085d8973d.png)

> 即56 5A 90 E8 03 90 49 AD B8 89 46 D3 A3 0B 1D 9F 80 0B A8 DB 66 5B 4F 5C 84 E5 41 D6 38 9E 59 D1

*   3，使用vs编译程序进行解密

![image](img/70cdd73962a1a92155fce928b724efa8.png)

![image](img/aa1948483022310eefd3ae239349505b.png)

*   4，读取数据

> 使用sqlcipher直接读取数据

![image](img/2500d6c3364cc09e6fb81e63b7c77fa7.png)