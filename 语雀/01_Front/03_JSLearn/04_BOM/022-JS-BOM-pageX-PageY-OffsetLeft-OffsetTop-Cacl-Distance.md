# JS-BOM-pageX-PageY-OffsetLeft-OffsetTop-Cacl-Distance

- 计算鼠标在盒子中的距离

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            background-color: pink;
            margin: 200px;
        }
    </style>
</head>

<body>
<div class="box"></div>
<script>
    let boxElement = document.querySelector('.box')
    boxElement.addEventListener('mousemove',(e)=>{
        //在整个页面的距离 - 盒子的距离
        let x = e.pageX - boxElement.offsetLeft;
        let y = e.pageY - boxElement.offsetTop;
        boxElement.innerHTML = 'x坐标是' + x + ' y坐标是' + y;
    })
</script>
</body>
</html>
```

