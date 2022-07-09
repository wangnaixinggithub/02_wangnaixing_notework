# JS-BOM-offsetWidth-offsetHeight

- 类似CSS元素的`width`  `height`样式。

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
    let width = sonElement.offsetWidth
    let height = sonElement.offsetHeight

    console.log(width) //100
    console.log(height) //100
</script>
</body>
</html>
```



