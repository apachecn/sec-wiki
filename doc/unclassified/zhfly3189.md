# Tomcat getPathInfo()的处理

> 原文：[https://www.zhihuifly.com/t/topic/3189](https://www.zhihuifly.com/t/topic/3189)

### Tomcat getPathInfo()的处理

和getServletPath()函数的处理是一样的，同样是返回前面经过Tomcat解析处理后的MappingData类对象中其中一个属性值，这里是获取的pathInfo属性值并直接返回：

![image](img/9f76ba47d95fcb53636aa0a97a7e5b46.png)