# SpringBoot-ThrowError-give-Static-Page

> 除了可以根据@ControllerAdvice来统一处理异常之外，还可以通过提供静态页面的方式来处理异常

- 比如在resource目录下的static目录下新建error文件夹，专门放发生异常时对应处理处理的静态页面。可以通过HTTP状态码来命名，比如说，状态码404就对应error文件夹下的404.html,同理状态码500对应error文件夹下500.html。

<img src="121-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E5%AF%B9%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E7%9A%84%E5%8F%A6%E4%B8%80%E7%A7%8D%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E9%9D%99%E6%80%81%E5%BC%82%E5%B8%B8%E9%A1%B5%E9%9D%A2.assets/image-20220327195651961.png" alt="image-20220327195651961"  />

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