# JS-DOM-#removeChild()

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
<button>删除</button>
<ul>
    <li>熊大</li>
    <li>熊二</li>
    <li>光头强</li>
</ul>
<script>
    let buttonElement = document.querySelector('button')
    let ulElement = document.querySelector('ul')

    buttonElement.onclick = () => {

        if (Object.is(0,ulElement.children.length)){
            console.log('我进来了')
           buttonElement.disabled = true
        }else {
            ulElement.removeChild(ulElement.children[0])
        }
    }
</script>
</body>

</html>
```

