# SpringMvc-ResponseEntity-Use-FileDown

## 1、文件下载思路

1. 找到文件，做成流。
2. 写入字节数组
3. 包装一个响应报文返回浏览器

## 2、响应报文实体ResponseEntity

```java
package com.wnx.springmvc.controller;

@Controller
public class FileDownController {

    @SneakyThrows
    @RequestMapping("/fileDown")
    public ResponseEntity<byte[]> fileDown(HttpSession session){
        //1.返回图片字节数组
        ServletContext servletContext = session.getServletContext();
        String realPath = servletContext.getRealPath("/static/iamges/1.jpg");
        InputStream inputStream = new FileInputStream(realPath);
        
        //2.创建该文件流大小的字节数组
        byte[] bytes = new byte[inputStream.available()];
        
        //3.把文件流读入字节数组中
        inputStream.read(bytes);
      
        //4告知浏览器，下载附件
        MultiValueMap<String,String> headers = new HttpHeaders();
        headers.add("Content-Disposition","attachment;filename=1.jpg");
        
        //5.返回响应报文
        return new ResponseEntity<byte[]>(bytes, headers, HttpStatus.OK);

    }
}

```

