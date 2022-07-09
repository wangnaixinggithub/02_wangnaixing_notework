# SpringMVC-文件上传

# 1、单文件上传

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>

<form th:action="@{/upload}" method="post" enctype="multipart/form-data">
    <table width="600px">
        <tr>
            <td>上传者</td>
            <td><input type="text" name="name" /></td>
        </tr>
        <tr>
            <td>上传文件1</td>
            <td><input type="file" name="myFile"  /></td>
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
   @SneakyThrows
    @PostMapping("/upload")
    @ResponseBody
    public void upload(@RequestParam("myFile") MultipartFile file, HttpServletRequest request) {
        //1.构造文件上传后保存路径
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
        String format = sdf.format(new Date());
        String realPath = request.getServletContext().getRealPath("/upload/") + format;
      
        //2.构造保存路径文件对象
        File folder = new File(realPath);
        if (!folder.exists()) {
            folder.mkdirs();
        }
        //3.给文件重写起名
        String oldName = file.getOriginalFilename();
        String newName = UUID.randomUUID().toString() + oldName.substring(oldName.lastIndexOf("."));
      
        //4.执行文件上传
        file.transferTo(new File(folder, newName));
        
        //5.输出浏览器可访问路径
        System.out.println(request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort()+request.getContextPath() + "/upload/" + format + "/" + newName);

    }
```

```properties
实际保存路径realPath = D:\my_workspace\spring-learn\out\artifacts\16_springmvc_upload_war_exploded\upload\2022\03\06
```

# 2、多文件上传

## 1、每个都接收

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>文件上传2</title>
</head>
<body>

<form th:action="@{/upload2}" method="post" enctype="multipart/form-data">
    <table width="600px">
        <tr>
            <td>上传者</td>
            <td><input type="text" name="name" /></td>
        </tr>
        <tr>
            <td>上传文件1</td>
            <td><input type="file" name="myFile"  /></td>
        </tr>
        <tr>
            <td>上传文件2</td>
            <td><input type="file" name="myFile2"  /></td>
        </tr>
        <tr>
            <td>上传文件2</td>
            <td><input type="file" name="myFile3"  /></td>
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
 @SneakyThrows
    @PostMapping("/upload2")
    @ResponseBody
    public void upload2(@RequestParam("myFile") MultipartFile file,
                        @RequestParam("myFile2") MultipartFile file2,
                        @RequestParam("myFile3") MultipartFile file3,
                        HttpServletRequest request){
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
        String format = sdf.format(new Date());
        String realPath = request.getServletContext().getRealPath("/upload/")+ format;
        File folder = new File(realPath);
        if (!folder.exists()){
            folder.mkdirs();
        }
        String oldName1 = file.getOriginalFilename();
        String newName1 = UUID.randomUUID().toString()+oldName1.substring(oldName1.lastIndexOf("."));
        //1.执行文件file1上传
        file.transferTo(new File(folder,newName1));
        System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName1);
        //2.执行另一个文件file2上传
        String oldName2 = file2.getOriginalFilename();
        String newName2 = UUID.randomUUID().toString()+oldName2.substring(oldName2.lastIndexOf("."));
        file.transferTo(new File(folder,newName2));
        System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName2);
        //3.执行另一个文件file3上传
        String oldName3 = file3.getOriginalFilename();
        String newName3 = UUID.randomUUID().toString()+oldName3.substring(oldName3.lastIndexOf("."));
        file.transferTo(new File(folder,newName3));
        System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName3);
    }
```

## 2、数组遍历

```java
    @SneakyThrows
    @PostMapping("/upload3")
    @ResponseBody
    public void upload3(@RequestParam("myFile") MultipartFile[] files, HttpServletRequest request){
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
        String format = sdf.format(new Date());
        String realPath = request.getServletContext().getRealPath("/upload/")+ format;
        File folder = new File(realPath);
        if (!folder.exists()){
            folder.mkdirs();
        }
        //循环遍历多文件各个上传
        for (MultipartFile file : files) {
            String oldName = file.getOriginalFilename();
            String newName = UUID.randomUUID().toString()+oldName.substring(oldName.lastIndexOf("."));
            file.transferTo(new File(folder,newName));
             System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName);
        }

    }
```



