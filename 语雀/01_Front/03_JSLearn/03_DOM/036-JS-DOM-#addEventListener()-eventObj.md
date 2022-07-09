# JS-DOM-#addEventListener()-eventObj

- 事件对象

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
<button id="btn">按钮</button>
<script>
let buttonElement = document.getElementById('btn')
    buttonElement.addEventListener('click',eventObj => {
        console.log(eventObj)
        //当前被点击的目标
        console.log(eventObj.target)
    })

</script>
</body>

</html>
```

