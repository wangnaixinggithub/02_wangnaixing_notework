# JS-BOM-Use-location-#href-handle-Return-Index

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
</head>
<body>
<button>点击</button>
<div></div>
<script>
   let buttonElement = document.querySelector('button')
   let divElement = document.querySelector('div');
   buttonElement.addEventListener('click',() => {
       location.href = 'https://www.baidu.com'
   })
   let time = 5 
   setInterval(() =>{


       if (Object.is(0,time)){
           location.href = 'https://www.baidu.com'
       }else {
           divElement.innerHTML = '您将在' + time + '秒钟之后跳转到首页'
           time--
       }

   },1000)
</script>
</body>
</html>
```

