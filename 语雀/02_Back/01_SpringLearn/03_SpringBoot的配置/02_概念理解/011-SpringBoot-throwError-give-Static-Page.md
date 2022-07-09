# SpringBoot-ThrowError-give-Static-Page

> 除了可以根据@ControllerAdvice来统一处理异常之外，还可以通过提供静态页面的方式来处理异常

- 比如在resource目录下的static目录下新建error文件夹，专门放发生异常时对应处理处理的静态页面。可以通过HTTP状态码来命名，比如说，状态码404就对应error文件夹下的404.html,同理状态码500对应error文件夹下500.html。

![image-20220327195651961](D:/my_workspace/01_my_notebook/%E8%AF%AD%E9%9B%80/01_SpringLearn/03_SpringBoot%E7%9A%84%E9%85%8D%E7%BD%AE/01_%E5%BC%80%E5%8F%91%E9%85%8D%E7%BD%AE/image-20220327195651961.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTTP状态码为404的异常处理</title>
</head>
<body>
<h1>系统中没有此资源</h1>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTTP状态码为500的异常处理</title>
</head>
<body>
<h1>系统发生了内部异常</h1>
</body>
</html>
```

- 当服务出现异常时，系统就会对应调到这些页面。