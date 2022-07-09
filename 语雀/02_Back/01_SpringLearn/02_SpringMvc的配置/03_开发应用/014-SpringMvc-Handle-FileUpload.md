# SpringMvc-Handle-FileUpload

```xml
        <!--fileupload-->
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.4</version>
        </dependency>
```

# 1、One File Upload

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>

<form action="/upload" method="post" enctype="multipart/form-data">
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
    @PostMapping("/upload3")
    @ResponseBody
    public void upload3(@RequestParam("myFile") MultipartFile[] files, HttpServletRequest request){

        //1.构造文件上传后保存路径
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
        String format = sdf.format(new Date());
        String realPath = request.getServletContext().getRealPath("/upload/")+ format;

        //2.构造保存路径文件对象
        File folder = new File(realPath);
        if (!folder.exists()){
            folder.mkdirs();
        }
        for (MultipartFile file : files) {
            //3.给文件重写起名
            String oldName = file.getOriginalFilename();
            String newName = UUID.randomUUID().toString()+oldName.substring(oldName.lastIndexOf("."));

            //4.执行文件上传
            try {
                file.transferTo(new File(folder,newName));
                System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName);

            } catch (IOException e) {
                e.printStackTrace();
            }


        }

    }
```



![image-20220215091744352](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220215091744352.png)

![image-20220215092030912](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220215092030912.png)

![image-20220215092207017](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220215092207017.png)

# 2、More File Upload(Two Ways)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>

<form action="/upload3" method="post" enctype="multipart/form-data">
    <table width="600px">
        <tr>
            <td>上传者</td>
            <td><input type="text" name="name" /></td>
        </tr>
        <tr>
            <td>上传文件</td>
            <td><input type="file" name="myFile" multiple /></td>
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
@PostMapping("/upload3")
    @ResponseBody
    public void upload3(@RequestParam("myFile") MultipartFile[] files, HttpServletRequest request){

        //1.构造文件上传后保存路径
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
        String format = sdf.format(new Date());
        String realPath = request.getServletContext().getRealPath("/upload/")+ format;

        //2.构造保存路径文件对象
        File folder = new File(realPath);
        if (!folder.exists()){
            folder.mkdirs();
        }
        for (MultipartFile file : files) {
            //3.给文件重写起名
            String oldName = file.getOriginalFilename();
            String newName = UUID.randomUUID().toString()+oldName.substring(oldName.lastIndexOf("."));

            //4.执行文件上传
            try {
                file.transferTo(new File(folder,newName));
                System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName);

            } catch (IOException e) {
                e.printStackTrace();
            }


        }

    }
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>

<form action="/upload2" method="post" enctype="multipart/form-data">
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
 @PostMapping("/upload2")
    @ResponseBody
    public void upload2(@RequestParam("myFile") MultipartFile file,
                        @RequestParam("myFile2") MultipartFile file2,
                        @RequestParam("myFile3") MultipartFile file3,
                        HttpServletRequest request){

            //1.构造文件上传后保存路径
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
            String format = sdf.format(new Date());
            String realPath = request.getServletContext().getRealPath("/upload/")+ format;



            //2.构造保存路径文件对象
            File folder = new File(realPath);
            if (!folder.exists()){
                folder.mkdirs();
            }

            //3.给文件重写起名
            String oldName1 = file.getOriginalFilename();
            String newName1 = UUID.randomUUID().toString()+oldName1.substring(oldName1.lastIndexOf("."));

            //4.执行文件上传
            try {
                file.transferTo(new File(folder,newName1));
                System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName1);

            } catch (IOException e) {
                e.printStackTrace();
            }


        //3.给文件重写起名
        String oldName2 = file2.getOriginalFilename();
        String newName2 = UUID.randomUUID().toString()+oldName2.substring(oldName2.lastIndexOf("."));

        //4.执行文件上传
        try {
            file.transferTo(new File(folder,newName2));
            System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName2);

        } catch (IOException e) {
            e.printStackTrace();
        }




        //3.给文件重写起名
        String oldName3 = file3.getOriginalFilename();
        String newName3 = UUID.randomUUID().toString()+oldName3.substring(oldName3.lastIndexOf("."));

        //4.执行文件上传
        try {
            file.transferTo(new File(folder,newName3));
            System.out.println(request.getScheme() + "://"+request.getServerName()+":"+request.getServerPort()+"/upload/"+format+"/"+newName3);

        } catch (IOException e) {
            e.printStackTrace();
        }


    }
```

