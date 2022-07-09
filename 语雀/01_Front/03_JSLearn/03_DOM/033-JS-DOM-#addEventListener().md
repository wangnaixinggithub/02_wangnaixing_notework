# JS-DOM-#addEventListener()

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        table {
            width: 500px;
            margin: 100px auto;
            border-collapse: collapse;
            text-align: center;
        }

        td,
        th {
            border: 1px solid #333;
        }

        thead tr {
            height: 40px;
            background-color: #ccc;
        }
    </style>
</head>

<body>
<button id="btn">这是一个按钮</button>
<script>
    let btnElement = document.getElementById('btn')
    btnElement.addEventListener('click',()=>{
        alert('使用事件监听绑定事件')
    })


</script>
</body>

</html>
```

