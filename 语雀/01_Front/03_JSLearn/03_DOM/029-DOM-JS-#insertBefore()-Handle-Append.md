# DOM-JS-#insertBefore()-Handle-Append

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

        body {
            padding: 100px;
        }

        textarea {
            width: 200px;
            height: 100px;
            border: 1px solid pink;
            outline: none;
            resize: none;
        }

        ul {
            margin-top: 50px;
        }

        li {
            width: 300px;
            padding: 5px;
            background-color: rgb(245, 209, 243);
            color: red;
            font-size: 14px;
            margin: 15px 0;
        }
    </style>
</head>

<body>
<textarea name="" id=""></textarea>
<button>发布</button>
<ul>

</ul>
<script>
    let textAreaElement = document.querySelector('textarea')
    let buttonElement = document.querySelector('button')
    let ulElement = document.querySelector('ul');


    buttonElement.onclick = () =>{
        if (!textAreaElement.value){
            alert('请输入数据')
        }else {
            let liElement = document.createElement('li')
            liElement.innerText = textAreaElement.value
            ulElement.insertBefore(liElement,ulElement.children[0])
        }

    }
</script>
</body>

</html>
```

