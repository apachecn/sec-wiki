# XXE 审计函数

> 原文：[https://www.zhihuifly.com/t/topic/3589](https://www.zhihuifly.com/t/topic/3589)

## XXE审计函数

XML解析一般在导入[配置](http://www.liuhaihua.cn/archives/tag/%E9%85%8D%E7%BD%AE)、数据传输接口等场景可能会用到，涉及到XML文件处理的场景可查看XML解析器是否禁用外部实体，从而判断是否存在XXE。部分XML解析接口（常见漏洞出现函数）如下：

**JAVA**

```
javax.xml.parsers.DocumentBuilderFactory;
javax.xml.parsers.SAXParser
javax.xml.transform.TransformerFactory
javax.xml.validation.Validator
javax.xml.validation.SchemaFactory
javax.xml.transform.sax.SAXTransformerFactory
javax.xml.transform.sax.SAXSource
org.xml.sax.XMLReader
DocumentHelper.parseText
DocumentBuilder
org.xml.sax.helpers.XMLReaderFactory
org.dom4j.io.SAXReader
org.jdom.input.SAXBuilder
org.jdom2.input.SAXBuilder
javax.xml.bind.Unmarshaller
javax.xml.xpath.XpathExpression
javax.xml.stream.XMLStreamReader
org.apache.commons.digester3.Digester
rg.xml.sax.SAXParseExceptionpublicId 
```

**PHP**

使用不安全的XML解析函数，SimpleXMLElement、simplexml_load_string函数解析body