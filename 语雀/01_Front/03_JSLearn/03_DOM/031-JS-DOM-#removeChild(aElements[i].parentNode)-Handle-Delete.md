# JS-DOM-#removeChild(aElements[i].parentNode)-Handle-Delete

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

        li a {
            float: right;
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
   let ulElement = document.querySelector('ul')

   buttonElement.onclick = () =>{
      let liElement = document.createElement('li')
      liElement.innerHTML = textAreaElement.value + '<a href="javascript:;">删除</a>'
       ulElement.insertBefore(liElement,ulElement.children[0])

       let aElements = document.querySelectorAll('a')
       for (let i = 0; i < aElements.length; i++) {
           aElements[i].onclick = () =>{
               ulElement.removeChild(aElements[i].parentNode)
           }
       }


   }
</script>
</body>

</html>
```

