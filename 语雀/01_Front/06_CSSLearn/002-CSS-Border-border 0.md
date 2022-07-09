# CSS-Border-`border: 0`

> 用于清空边框样式

# 1、Defalut Case

- 默认情况下，input输入库是有边框的。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
 
</style>

<body>
<input class="inputClass">
</body>
</html>
```

![image-20220616183905007](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616183905007.png)

# 2、Use Border:0

- 而我们通过`border:0`，就能达到情况边框样式的目的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    .inputClass{
        width: 300px;
        height: 30px;
        /*此时就没有边框了。。*/
        border: 0;
    }
</style>

<body>
<input class="inputClass">
</body>
</html>
```

