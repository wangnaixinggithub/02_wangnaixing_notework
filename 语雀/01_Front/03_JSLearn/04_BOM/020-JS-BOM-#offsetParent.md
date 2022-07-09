# JS-BOM-#offsetParent

- 获取到偏移父元素

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .father {
            /*加入定位*/
            position: relative;
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 150px;
        }


        .son {
            width: 100px;
            height: 100px;
            background-color: purple;
            margin-left: 45px;
        }
    </style>
</head>

<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    let sonElement = document.querySelector('.son')
    //获取到父盒子元素，如果父盒子没有定位则获取到Body元素
    let fatherElement =  sonElement.offsetParent
    console.log(fatherElement)
</script>
</body>
</html>
```

