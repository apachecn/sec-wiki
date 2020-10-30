# XXE 防御

> 原文：[https://www.zhihuifly.com/t/topic/3596](https://www.zhihuifly.com/t/topic/3596)

## XXE 防御

使用语言中推荐的禁用外部实体的方法

**PHP：**

```
libxml_disable_entity_loader(true); 
```

**JAVA:**

```
DocumentBuilderFactory dbf =DocumentBuilderFactory.newInstance();
dbf.setExpandEntityReferences(false);

.setFeature(“[http://apache.org/xml/features/disallow-doctype-decl](http://apache.org/xml/features/disallow-doctype-decl)”,true);

.setFeature(“[http://xml.org/sax/features/external-general-entities](http://xml.org/sax/features/external-general-entities)”,false) `.setFeature(“[http://xml.org/sax/features/external-parameter-entities](http://xml.org/sax/features/external-parameter-entities)”,false);` 
```

**Python：**

```
from lxml import etree
xmlData = etree.parse(xmlSource,etree.XMLParser(resolve_entities=False)) 
```