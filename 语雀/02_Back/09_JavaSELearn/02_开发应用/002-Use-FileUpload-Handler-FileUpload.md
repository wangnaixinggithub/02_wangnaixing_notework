# Use-FileUpload-Handler-FileUpload

```xml
     <dependencies>
        <!--servlet-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        <!--fileupload-->
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.4</version>
        </dependency>
    </dependencies>
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
                            "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>文件上传</title>
</head>
<body>
 	<form action="UploadServlet" method="post" enctype="multipart/form-data">
 		<table width="600px">
 			<tr>
 				<td>上传者</td>
 				<td><input type="text" name="name" /></td>
 			</tr>
 			<tr>
 				<td>上传文件</td>
 				<td><input type="file" name="myfile" /></td>
 			</tr>
 			<tr>
 				<td colspan="2"><input type="submit" value="上传" /></td>
 			</tr>
 		</table>
 	</form>
</body>
</html>
```

```java
@WebServlet(urlPatterns = "/UploadServlet")
public class UploadServlet extends HttpServlet {
   
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=UTF-8");
        try {
            DiskFileItemFactory factory = new DiskFileItemFactory();
            ServletFileUpload fileupload = new ServletFileUpload(factory);

            //1.设置上传配置

            //1.1.设置编码为UTF-8
            factory.setDefaultCharset("UTF-8");
            //1.2.限定文件上传总大小为10M
            fileupload.setSizeMax(10485760);
            //1.3.单个上传最大为10M
            fileupload.setFileSizeMax(10485760);
            //1.4.在内存中超过4M写入文件上传缓存目录
            factory.setSizeThreshold(4096);
            //1.5.设置文件上传缓存目录
            File f = new File("E:\\TempFolder");
            if (!f.exists()) { f.mkdirs(); }
            factory.setRepository(f);


            //2.从请求中解析出文件项集合
            List<FileItem> fileitems = fileupload.parseRequest(req);
            PrintWriter writer = resp.getWriter();

            //3.遍历上传项集合：对表单提交的数据处理，对流文件的数据进行处理
            for (FileItem fileitem : fileitems) {
                if (fileitem.isFormField()) {
                    String name = fileitem.getFieldName();
                    if(name.equals("name")){
                        if(!fileitem.getString().equals("")){
                            String value = fileitem.getString("utf-8");
                            writer.print("上传者：" + value + "<br>");
                        }
                    }
                } else {
                    String oldName = fileitem.getName();
                    if(oldName != null && !oldName.equals("")){

                        //3.1.构造文件上传后保存路径
                        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
                        String format = sdf.format(new Date());
                        String realPath = req.getServletContext().getRealPath("/upload/")+ format;

                        //3.2.构造保存路径文件对象
                        File folder = new File(realPath);
                        if (!folder.exists()){
                            folder.mkdirs();
                        }

                        //3.3给文件重写起名
                        String newName = UUID.randomUUID().toString()+oldName.substring(oldName.lastIndexOf("."));



                        //3.4构造文件
                        File file = new File(folder,newName);



                        //3.5接收文件流
                        InputStream in = fileitem.getInputStream();
                        FileOutputStream out = new FileOutputStream(file);
                        byte[] buffer = new byte[1024];
                        int len;
                        while ((len = in.read(buffer)) > 0)
                            out.write(buffer, 0, len);


                        //4释放资源
                        in.close();
                        out.close();
                        fileitem.delete();
                        String url = req.getScheme() + "://"+req.getServerName()+":"+req.getServerPort()+"/upload/"+format+"/"+newName;
                        resp.getWriter().println(url);
                    }
                }
            }
        } catch (FileUploadException e) {
            
            e.printStackTrace();
        }


    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp);
    }
}

```

![image-20220212181525871](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220212181525871.png)

