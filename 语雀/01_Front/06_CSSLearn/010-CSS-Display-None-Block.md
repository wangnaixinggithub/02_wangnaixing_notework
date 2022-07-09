# CSS-Display-None-Block

# 1、Default Case

- 默认情况下，body标签存在属性`display:bloak`，而div作为body标签的子标签，继承并拥有了该属性。使得我们能再浏览器上看到。

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
    border: 2px solid red;
}
</style>

<body>
<div class="box1"></div>
</body>
</html>
```

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616194017098.png" alt="image-20220616194017098" style="zoom:80%;" />

# 2、display:none

- 这样就可以让红色的盒子隐藏起来了。元素只是隐藏了，他还是存在。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
.box1{
    /*让盒子消失*/
    display: none;
    width: 150px;
    height: 150px;
    border: 2px solid red;
}
</style>

<body>
<div class="box1"></div>
</body>
</html>
```

![image-20220616194204244](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616194204244.png)