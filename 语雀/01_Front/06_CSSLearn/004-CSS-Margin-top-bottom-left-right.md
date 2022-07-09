# Margin-top-bottom-left-right

- 提供了四个方向操作外边距的方法。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>

.box{
    width: 150px;
    height: 150px;
    /*也提供单独操作四个方向外边距的方法*/
    margin-top: 100px;
    margin-bottom: 100px;
    margin-left: 200px;
    margin-right: 200px;
    
    background-color: red;
}
</style>
<body>
<div class="box">

</div>
</body>
</html>
```

- 如果上下 左右 两边数值相当。可以进行属性简写。上述可改为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>

.box{
    width: 150px;
    height: 150px;
    /*也提供单独操作四个方向外边距的方法*/
    margin: 100px 200px;

    background-color: red;
}
</style>
<body>
<div class="box">

</div>
</body>
</html>
```

