# CSS-text-align

# 1、Default Case

- 默认情况下，在盒子里面的文字是水平排列的，如果盒子内的文字超过盒子宽度，那么默认他又会再起一行继续排列。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
.box1{
    width: 150px;
    height: 150px;
    border: 1px solid red;
}
</style>

<body>
<div class="box1">
    淘宝二维码
</div>
</body>
</html>
```

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616195405236.png" alt="image-20220616195405236" style="zoom:80%;" /><img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616195507994.png" alt="image-20220616195507994" style="zoom:80%;" />

# 2、text-align

- 我们可以通过`text-align` 设置文本的对齐方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
.box1{
    width: 150px;
    height: 150px;
    /*文本居中显示*/
    text-align: center;
    border: 1px solid red;

}
</style>

<body>
<div class="box1">
    淘宝二维码
</div>
</body>
</html>
```

![image-20220616195957128](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616195957128.png)