# CSS-Position

# 1、Default Case

- 对于盒子的嵌套，默认是没有边距进行的，就是子盒子会复用父盒子的边。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>

.box{
    width: 300px;
    height: 300px;
    border: 1px solid red;
}
.box1{
    width: 50px;
    height:50px;
    border: 1px solid dodgerblue;
}
</style>
<body>
<div class="box">
    <div class="box1">

    </div>
</div>
</body>
</html>
```

![image-20220616182319552](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616182319552.png)



# 2、Use position

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>

.box{
    /*1.设置父容器位置为相对的*/
    position: relative;
    width: 300px;
    height: 300px;
    border: 1px solid red;
}
.box1{
    /*2.设置子容器位置为绝对的*/
    position: absolute;

    /*3.通过left right top bottom 可对子盒子进行移动*/
    top:100px;
    left: 100px;
    bottom: 100px;
    right: 100px;

    width: 50px;
    height:50px;

    border: 1px solid dodgerblue;
}
</style>
<body>
<div class="box">
    <div class="box1">

    </div>
</div>
</body>
</html>
```



![image-20220616182722778](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616182722778.png)