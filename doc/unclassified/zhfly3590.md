# XXE漏洞代码示例

> 原文：[https://www.zhihuifly.com/t/topic/3590](https://www.zhihuifly.com/t/topic/3590)

## **XXE漏洞代码示例**

**JAVA**

JAVA解析XML的方法越来越多，常见有四种，即：DOM、DOM4J、JDOM 和SAX。下面以这四种为例展示XXE漏洞。

1）DOM Read XML

```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {      
        String result="";
        try {
            //DOM Read XML
            DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();     
            DocumentBuilder db = dbf.newDocumentBuilder();                  
            Document doc = db.parse(request.getInputStream());

```
 String username = getValueByTagName(doc,"username");
        String password = getValueByTagName(doc,"password");
        if(username.equals(USERNAME) &amp;&amp; password.equals(PASSWORD)){
            result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",1,username);
        }else{
            result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",0,username);
        }
    } catch (ParserConfigurationException e) {
        e.printStackTrace();
        result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",3,e.getMessage());
    } catch (SAXException e) {
        e.printStackTrace();
        result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",3,e.getMessage());
    }
    response.setContentType("text/xml;charset=UTF-8");
    response.getWriter().append(result);
} 
``` 
```

2)DOM4J Read XML

```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {          
        String result="";
        try {
            //DOM4J Read XML
            SAXReader saxReader = new SAXReader();
            Document document = saxReader.read(request.getInputStream());

```
 String username = getValueByTagName2(document,"username");
        String password = getValueByTagName2(document,"password");

        if(username.equals(USERNAME) &amp;&amp; password.equals(PASSWORD)){
            result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",1,username);
        }else{
            result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",0,username);
        }                

    } catch (DocumentException  e) {
        System.out.println(e.getMessage());
    } 
    response.setContentType("text/xml;charset=UTF-8");
    response.getWriter().append(result);
} 
``` 
```

3)JDOM2 Read XML

```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {             
        String result="";
        try {
            //JDOM2 Read XML    
            SAXBuilder builder = new SAXBuilder();  
            Document document = builder.build(request.getInputStream());

```
 String username = getValueByTagName3(document,"username");
        String password = getValueByTagName3(document,"password");

        if(username.equals(USERNAME) &amp;&amp; password.equals(PASSWORD)){
            result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",1,username);
        }else{
            result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",0,username);
        }

    } catch (JDOMException  e) {
        System.out.println(e.getMessage());
    } 
    response.setContentType("text/xml;charset=UTF-8");
    response.getWriter().append(result);
} 
``` 
```

4)SAX Read XML

```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {      
        //https://blog.csdn.net/u011024652/article/details/51516220
        String result="";
        try {
            //SAX Read XML
            SAXParserFactory factory  = SAXParserFactory.newInstance(); 
            SAXParser saxparser = factory.newSAXParser();  
            SAXHandler handler = new SAXHandler();  
            saxparser.parse(request.getInputStream(), handler);
            //为简单，没有提取子元素中的数据，只要调用parse()解析xml就已经触发xxe漏洞了
            //没有回显  blind xxe
             result = String.format("<result><code>%d</code><msg>%s</msg></result>",0,1);

```
 } catch (ParserConfigurationException e) {
        e.printStackTrace();
        result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",3,e.getMessage());
    } catch (SAXException e) {
        e.printStackTrace();
        result = String.format("&lt;result&gt;&lt;code&gt;%d&lt;/code&gt;&lt;msg&gt;%s&lt;/msg&gt;&lt;/result&gt;",3,e.getMessage());
    }
    response.setContentType("text/xml;charset=UTF-8");
    response.getWriter().append(result);
} 
``` 
```

其他：

0x1:

```
public class XXE {

```
@RequestMapping(value = "/xmlReader", method = RequestMethod.POST)
@ResponseBody
public  String xxe_xmlReader(HttpServletRequest request) {
    try {
        String xml_con = getBody(request);
        System.out.println(xml_con);
        XMLReader xmlReader = XMLReaderFactory.createXMLReader();
        xmlReader.parse( new InputSource(new StringReader(xml_con)) );  // parse xml
        return "ok";
    } catch (Exception e) {
        System.out.println(e);
        return "except";
    }
} 
``` 
```

0x2:

```
 @RequestMapping(value = "/SAXBuilder", method = RequestMethod.POST)
    @ResponseBody
    public  String xxe_SAXBuilder(HttpServletRequest request) {
        try {
            String xml_con = getBody(request);
            System.out.println(xml_con);

```
 SAXBuilder builder = new SAXBuilder();
        org.jdom2.Document document = builder.build( new InputSource(new StringReader(xml_con)) );  // cause xxe
        return "ok";
    } catch (Exception e) {
        System.out.println(e);
        return "except";
    }
} 
``` 
```

0x3:

```
@RequestMapping(value = "/SAXReader", method = RequestMethod.POST)
    @ResponseBody
    public  String xxe_SAXReader(HttpServletRequest request) {
        try {
            String xml_con = getBody(request);
            System.out.println(xml_con);

```
 SAXReader reader = new SAXReader();
        org.dom4j.Document document = reader.read(  new InputSource(new StringReader(xml_con)) ); // cause xxe

        return "ok";
    } catch (Exception e) {
        System.out.println(e);
        return "except";
    }
} 
``` 
```

0x4:

```
 @RequestMapping(value = "/SAXParser", method = RequestMethod.POST)
    @ResponseBody
    public String xxe_SAXParser(HttpServletRequest request) {
        try {
            String xml_con = getBody(request);
            System.out.println(xml_con);

```
 SAXParserFactory spf = SAXParserFactory.newInstance();
        SAXParser parser = spf.newSAXParser();
        parser.parse(new InputSource(new StringReader(xml_con)), new DefaultHandler());  // parse xml

        return "test";
    } catch (Exception e) {
        System.out.println(e);
        return "except";
    }
} 
``` 
```

0x5:

```
 // 有回显的XXE
    @RequestMapping(value = "/DocumentBuilder_return", method = RequestMethod.POST)
    @ResponseBody
    public String xxeDocumentBuilderReturn(HttpServletRequest request) {
        try {
            String xml_con = getBody(request);
            System.out.println(xml_con);

```
 DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        DocumentBuilder db = dbf.newDocumentBuilder();
        StringReader sr = new StringReader(xml_con);
        InputSource is = new InputSource(sr);
        Document document = db.parse(is);  // parse xml

        // 遍历xml节点name和value
        StringBuffer buf = new StringBuffer();
        NodeList rootNodeList = document.getChildNodes();
        for (int i = 0; i &lt; rootNodeList.getLength(); i++) {
            Node rootNode = rootNodeList.item(i);
            NodeList child = rootNode.getChildNodes();
            for (int j = 0; j &lt; child.getLength(); j++) {
                Node node = child.item(j);
                buf.append( node.getNodeName() + ": " + node.getTextContent() + "\n" );
            }
        }
        sr.close();
        System.out.println(buf.toString());
        return buf.toString();
    } catch (Exception e) {
        System.out.println(e);
        return "except";
    }
}

@RequestMapping(value = "/DocumentBuilder", method = RequestMethod.POST)
@ResponseBody
public String DocumentBuilder(HttpServletRequest request) {
    try {
        String xml_con = getBody(request);
        System.out.println(xml_con);

        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        DocumentBuilder db = dbf.newDocumentBuilder();
        StringReader sr = new StringReader(xml_con);
        InputSource is = new InputSource(sr);
        Document document = db.parse(is);  // parse xml

        // 遍历xml节点name和value
        StringBuffer result = new StringBuffer();
        NodeList rootNodeList = document.getChildNodes();
        for (int i = 0; i &lt; rootNodeList.getLength(); i++) {
            Node rootNode = rootNodeList.item(i);
            NodeList child = rootNode.getChildNodes();
            for (int j = 0; j &lt; child.getLength(); j++) {
                Node node = child.item(j);
                // 正常解析XML，需要判断是否是ELEMENT_NODE类型。否则会出现多余的的节点。
                if(child.item(j).getNodeType() == Node.ELEMENT_NODE) {
                    result.append( node.getNodeName() + ": " + node.getFirstChild().getNodeValue() + "\n" );
                }
            }
        }
        sr.close();
        System.out.println(result.toString());
        return result.toString();
    } catch (Exception e) {
        System.out.println(e);
        return "except";
    }
} 
``` 
```

**PHP**

如：

```
<?php
libxml_disable_entity_loader(false);//这个函数原本是禁止加载外部实体,false就是允许
$user1 = $_POST['user1'];
$xmlfile = file_get_contents('php://input'); //接收xml数据
$aa = str_replace('<!ENTITY','**',$xmlfile);
$dom = new DOMDocument();
$dom->loadXML($aa, LIBXML_NOENT | LIBXML_DTDLOAD);
$creds = simplexml_import_dom($dom);
$user = $creds->user;
?>
<!DOCTYPE html>
<head>
<title>08067</title>
<meta name="description" content="slick Login">
<meta name="author" content="MRYE+">
<link rel="stylesheet" type="text/css" href="./xxe/style.css" />
<style type="text/css">
textarea{ resize:none; width:400px; height:200px;margin:10px auto;}
</style>
<script type="text/javascript" src="./xxe/jquery-latest.min.js"></script>
<script type="text/javascript" src="./xxe/placeholder.js"></script>
</head>
<body>
<form id="slick-login" action="./index.php" method="post" >
<label for="user">user</label>
<input type="text" name="user1" class="placeholder" placeholder="user ?">
<input type="submit" value="search">
<textarea  name="comment" form="usrform" class="placeholder" placeholder="......"><?php echo "$user";echo "$user1";?></textarea>
</form>
</body>
</html> 
```

注：该段代码取自某ctf