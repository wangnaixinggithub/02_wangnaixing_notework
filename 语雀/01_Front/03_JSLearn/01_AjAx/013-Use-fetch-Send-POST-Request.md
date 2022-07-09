# Use-fetch-Send-POST-Request

# 1、One Way

- 发送携带JSON的POST请求

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Fetch-POST</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>


<script>
document.getElementById('btn1').onclick = () => {

    let requestParam = {
        'bookId':1,
        'bookName':'Java',
        'bookPrice':101.10
    }

    let config = {
        method: 'POST',
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
        body:JSON.stringify(requestParam)
    }



        fetch('http://localhost:3000/book/insert',config).then(res=>{
        console.log(res)

       return res.text()

    }).then(res=>{
        console.log(res)
    })
}


</script>

</body>
</html>
```

# 2、Two Way

- 创建Request对象。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>F-GET</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>


<script>
document.getElementById('btn1').onclick = () => {

    let requestParam = {
        'bookId':1,
        'bookName':'Java',
        'bookPrice':101.10
    }


    const onceRequest = new Request('http://localhost:3000/book/insert', {
        method: 'POST',
        body:JSON.stringify(requestParam),
    });


    fetch(onceRequest).then(res=>{
        console.log(res)

       return res.text()

    }).then(res=>{
        console.log(res)
    })
}


</script>

</body>
</html>
```

```json
let express = require('express');

let router = express.Router();

router.post('/insert',(request,response)=>{

    response.setHeader('Access-Control-Allow-Origin', '*')
    response.send('成功添加图书！')
})

module.exports = router;
```

![image-20220604114849449](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604114849449.png)