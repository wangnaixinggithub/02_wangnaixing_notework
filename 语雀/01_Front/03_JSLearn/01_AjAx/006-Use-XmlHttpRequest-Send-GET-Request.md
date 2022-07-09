# Use-XmlHttpRequest-Send-GET-Request

# 1、Send Asynchronous Request

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
    //1.Create XMLHttpRequest
    const xhr = new XMLHttpRequest();
    //2.Send Request
    xhr.open('GET', 'http://localhost:3000/book/insert?bookId=1&bookName=Java&bookPrice=101.10')
    xhr.send();

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

# 2、Express Server Handle

```javascript
let express = require('express');

let router = express.Router();

router.get('/insert',(request,response)=>{
	//支持跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.send('成功添加图书！')
})

module.exports = router;
```

![image-20220603163609318](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220603163609318.png)