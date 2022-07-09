# JS-DOM-selectstart-No-Choose-Context

- 通过绑定`selectstart`事件，禁用默认的文本选中默认行为，来达成，不给用户选中文字的效果

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

    </style>
</head>

<body>
<h1>这一段文字不能被选中！！！</h1>

<script>
    let h1Element = document.querySelector('h1')
    h1Element.addEventListener('selectstart',(e)=>{
        //禁用文本选中
        e.preventDefault()
    })
</script>
</body>

</html>
```

