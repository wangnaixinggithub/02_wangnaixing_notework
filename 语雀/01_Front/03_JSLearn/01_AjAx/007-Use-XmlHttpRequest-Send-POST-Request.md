# 006-Use-XmlHttpRequest-Send-POST-Request

# 1、application/x-www-form-urlencoded

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-GET</title>
</head>
<style>
    .box1{
        height: 300px;
        width: 300px;
        border: 1px solid #00B7FF;
    }
</style>
<body>
<button id="btn1">添加图书</button>
<div class="box1">

</div>
<script>
let btnEle = document.getElementById('btn1');
let boxEle = document.getElementsByClassName('box1')[0]
btnEle.onclick = () => {
    const xhr = new XMLHttpRequest();
    
    xhr.open('post', 'http://localhost:3000/book/insert',true)
    
    //1.SET ContentType
    xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded')
   
    //2. RequestParam Create
    xhr.send(`booId=1&bookName=${encodeURIComponent('Java入门到精通')}&bookPrice=39.9&bookPublish=${encodeURIComponent('人民日报出版社')}`)

    //3.Handle Response
    xhr.onreadystatechange = () =>{
        if(xhr.readyState === 4){
            if(xhr.status >=200 && xhr.status < 300){
                boxEle.innerHTML = xhr.response
            }
        }
    }
}
</script>
</body>
</html>
```

```javascript
let express = require('express');

let router = express.Router();

router.post('/insert',(request,response)=>{

    response.setHeader('Access-Control-Allow-Origin', '*');
    response.send('成功添加图书！')
})

module.exports = router;
```

# 2、Other Way，Use FormData

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-GET</title>
</head>
<style>
    .box1{
        height: 300px;
        width: 300px;
        border: 1px solid #00B7FF;
    }
</style>
<body>
<button id="btn1">添加图书</button>
<div class="box1">

</div>
<script>
let btnEle = document.getElementById('btn1');
let boxEle = document.getElementsByClassName('box1')[0]
btnEle.onclick = () => {
    const xhr = new XMLHttpRequest();

    xhr.open('post', 'http://localhost:3000/book/insert',true)
    let formData = new FormData()

    formData.append('bookId','1')
    formData.append('bookName',encodeURIComponent('Java入门到精通'))
    formData.append('bookPrice','39.9')
    formData.append('bookPublish',encodeURIComponent('人民日报出版社'))

    xhr.send(formData)

    //3.Handle Response
    xhr.onreadystatechange = () =>{
        if(xhr.readyState === 4){
            if(xhr.status >=200 && xhr.status < 300){
                boxEle.innerHTML = xhr.response
            }
        }
    }
}
</script>
</body>
</html>
```

