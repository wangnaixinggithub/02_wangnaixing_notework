# Use-JavaSE-Handler-FileDownload

> 如果是中文，需要URL转码器

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@page import="java.net.URLEncoder"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>文件下载</title>
</head>
<body>
		<a href="${pageContext.request.contextPath}/DownloadServlet?filename=a.jpg">英文文件下载测试</a>

	    <a href="${pageContext.request.contextPath}/DownloadServlet?filename=风景.jpg">中文文件下载测试</a>
</body>
</html>
```

```java
@WebServlet("/DownloadServlet")
public class DownloadServlet  extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.设置响应类型 统一编码
        resp.setContentType("text/html;charset=utf-8");
        resp.setCharacterEncoding("utf-8");
        req.setCharacterEncoding("utf-8");

        //2.获取文件名
        String filename = req.getParameter("filename");
        String agent = req.getHeader("user-agent");
        
        
        filename = URLDecoder.decode(filename, "UTF-8"); //浏览器会对中文进行UTF-8编码，所以我们想得到中文文件名，必须进行解码
        
        String folder = "/download/";




        //3.通知浏览器，以附件下载方式打开
        resp.addHeader("Content-Type", "application/octet-stream");
        resp.addHeader("Content-Disposition", "attachment;filename="+ URLEncoder.encode(filename,"UTF-8")); //浏览器会对响应头含中文部分进行UTF-8编码，所以我们必须对中文名称进行UTF-8编码。


        //4.读取并转化为流写入
        InputStream in = getServletContext().getResourceAsStream(folder+filename);
        OutputStream out = resp.getOutputStream();


        byte[] buffer = new byte[1024];
        int len;
        while ((len = in.read(buffer)) != -1) {
            out.write(buffer, 0, len);
        }

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }
}

```

