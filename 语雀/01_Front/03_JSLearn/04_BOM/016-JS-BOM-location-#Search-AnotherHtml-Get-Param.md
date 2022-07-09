# JS-BOM-location-#Search-AnotherHtml-Get-Param

- 通过`location`对象的`search`属性，传递到另一个页面获取。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
<form action="index.html">
  用户名： <input type="text" name="username">
  <input type="submit" value="登录">
</form>
</body>
</html>
```

- 优化：去掉？=号分隔成字符串数组。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
</head>
<body>
<div></div>
<script>

    //获取URL的查询字符串
    let search = location.search.substring(1)

    //获取用户名
    let username = search.split("=")[1]

    let divElement = document.querySelector('div')

    divElement.innerText = username + "欢迎您！"
    divElement.style.color = 'red'

</script>
</body>
</html>
```

