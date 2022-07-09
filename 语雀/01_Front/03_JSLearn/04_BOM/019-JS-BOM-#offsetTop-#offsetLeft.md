# JS-BOM-#offsetTop-#offsetLeft

- 获取盒子边距，可以通过offsetTop offsetLeft属性，和CSS的margin很类似。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /*清空原来的默认边距样式*/
        * {
            margin: 0;
            padding: 0;
        }

        .father {
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 150px;
        }
    </style>
</head>

<body>
<div class="father"></div>
<script>
   //获取偏移量
    let fatherElement = document.querySelector('.father')
    let topOffset =  fatherElement.offsetTop
    let leftOffset = fatherElement.offsetLeft
    
    console.log(topOffset) //150
    console.log(leftOffset) //150
</script>
</body>
</html>
```

- 如果是盒子嵌套的情况，父盒子没有定位，就把body作为标准。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        /*清空原来的默认边距样式*/
        * {
            margin: 0;
            padding: 0;
        }

        .father {
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
   //获取偏移量
    let sonElement = document.querySelector('.son')
    let topOffset =  sonElement.offsetTop
    let leftOffset = sonElement.offsetLeft

    console.log(topOffset) //150
    console.log(leftOffset) //  fatherDiv没有定位 则把Body为标准, 195
</script>
</body>
</html>
```

- 加入父盒子定位后，才是我们想得到的margin值。

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
    let topOffset =  sonElement.offsetTop
    let leftOffset = sonElement.offsetLeft

    console.log(topOffset) //0
    console.log(leftOffset) //  fatherDiv有定位 则把fatherDiv为标准, 45
</script>
</body>
</html>
```

