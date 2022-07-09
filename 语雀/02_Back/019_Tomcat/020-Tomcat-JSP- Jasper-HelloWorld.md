# Tomcat-JSP- Jasper-HelloWorld

对于基于JSP 的web应用来说，我们可以直接在JSP页面中编写 Java代码，添加第三方的 标签库，以及使用EL表达式。

但是无论经过何种形式的处理，最终输出到客户端的都是 标准的HTML页面（包含js ，css...），并不包含任何的java相关的语法。 

也就是说， 我们可以把jsp看做是一种运行在服务端的脚本。



Jasper模块是Tomcat的JSP核心引擎，我们知道JSP本质上是一个Servlet。

Tomcat使用 Jasper对JSP语法进行解析，生成Servlet并生成Class字节码，用户在进行访问jsp时，会 访问Servlet，最终将访问的结果直接响应在浏览器端 。

另外，在运行的时候，Jasper还 会检测JSP文件是否修改，如果修改，则会重新编译JSP文件。



# 1、Your Can Do JavaCoding In JSP

```jsp
<%@ page import="java.text.DateFormat" %>
<%@ page import="java.text.SimpleDateFormat" %>
<%@ page import="java.util.Date" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>页面嘿嘿嘿</title>
</head>
<body>
<%
    DateFormat dateFormat = new SimpleDateFormat("yyyy‐MM‐dd HH:mm:ss");
    String format = dateFormat.format(new Date());
%>
Hello , Java Server Page 。。。。
<br/>
<%= format %>
</body>
</html>
```

# 2、Return Browser is HTML Page

```html
<html>
<head>
    <title>$Title$</title>
</head>
<body>

Hello , Java Server Page 。。。。
<br/>
2022‐07‐06 09:31:27
</body>
</html>
```

