# （CVE-2020-1957）Apache Shiro 权限绕过漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2822](https://www.zhihuifly.com/t/topic/2822)

# （CVE-2020-1957）Apache Shiro 权限绕过漏洞

## 一、漏洞简介

Apache Shiro 1.5.2之前版本中存在安全漏洞。攻击者可借助特制的请求利用该漏洞绕过身份验证。

## 二、漏洞影响

Apache Shiro < 1.5.2

## 三、复现过程

### 环境搭建

明白上文的内容，漏洞复现就很容易了，复现环境代码主要参考网上的开源[demo](https://github.com/lenve/javaboy-code-samples/tree/master/shiro/shiro-basic)。

*   下载demo代码 [shiro-basic](https://github.com/lenve/javaboy-code-samples/tree/master/shiro/shiro-basic)。
*   导入idea
*   Shiro版本1.4.2

```
 <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-web</artifactId>
        <version>1.4.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
        <version>1.4.2</version>
    </dependency> 
```

*   修改ShiroConfig配置文件，添加authc拦截器的拦截正则

```
 @Bean
    ShiroFilterFactoryBean shiroFilterFactoryBean() {
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        ...
        ...
        //map.put("/*", "authc");
        map.put("/hello/*", "authc"); 
        bean.setFilterChainDefinitionMap(map);
        return bean;
    } 
```

*   修改路由控制器方法

```
@GetMapping("/hello/{currentPage}")
    public String hello(@PathVariable Integer currentPage) {
        return "hello";
} 
```

*   启动应用

### 漏洞复现

访问`/hello/1`接口，可以看到被authc拦截器拦截了，将会跳转到登录接口进行登录。

![image](img/2617a5894ea2dc959b32be6ea31af0e8.png)

访问/hello/1/，成功绕过authc拦截器，获取到了资源。

![image](img/496099b558a0ccf28f7a89d73f2ced52.png)

### ≤1.5.1版本绕过

观察1.5.2版本中新添加的[测试用例](https://github.com/apache/shiro/commit/3708d7907016bf2fa12691dff6ff0def1249b8ce)

![image](img/b327e396e8cb8472bb3b49dd30206db1.png)

切换测试版本到1.5.1中，然后从中上面的测试用例提取payload进行绕过。

在1.5.1版本中，添加`/`还是会直接跳转到登录。

![image](img/b9eea619555bf8697de05bec490b2fbe.png)

绕过payload，`/fdsf;/../hello/1`，成功绕过。

![image](img/b8a6280671522b6a2e4f13c76423f5ee.png)

问题同样可以定位到getChain函数中对于requestURI的获取中，如下图所示，`this.getPathWithinApplication(request)`获取的requestURI为`/fdsf` ，而不是我们输入的`/fdsf;/../hello/1`，从而导致后面的URI路径模式匹配返回False，从而再次绕过了shiro拦截器。

![image](img/cb749e8a157335ab013f3c7ba3fbfae3.png)

getPathWithinApplication函数中会调用WebUtils (org.apache.shiro.web.util)中的getRequestUri函数获取RequestUri。

```
public static String getRequestUri(HttpServletRequest request) {
        String uri = (String)request.getAttribute("javax.servlet.include.request_uri");
        if (uri == null) {
            uri = request.getRequestURI();
        }

```
 return normalize(decodeAndCleanUriString(request, uri));
} 
``` 
```

RequestUri函数中最终调用decodeAndCleanUriString函数对URI进行清洗。

```
private static String decodeAndCleanUriString(HttpServletRequest request, String uri) {
      uri = decodeRequestString(request, uri);
      int semicolonIndex = uri.indexOf(59);//获取;号的位置
      return semicolonIndex != -1 ? uri.substring(0, semicolonIndex) : uri;
  } 
```

如果URI中存在`;`号的话，则会删除其后面的所有字符。`/fdsf;/../hello/1/`最终也就变成了`/fdsf`。

![image](img/ad54ee09f8649991639eaef67a61e41f.png)

## 参考链接

> https://blog.riskivy.com/shiro-%e6%9d%83%e9%99%90%e7%bb%95%e8%bf%87%e6%bc%8f%e6%b4%9e%e5%88%86%e6%9e%90%ef%bc%88cve-2020-1957%ef%bc%89/