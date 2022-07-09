# Use-Axios-Send-GET-Request

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Axios-GET</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>

document.getElementById('btn1').onclick = () => {
    axios.get('http://localhost:3000/book/insert?bookId=1&bookName=Java&bookPrice=101.10').then(response=>{
        console.log(response)
    })
}
</script>

</body>
</html>
```

```js
let express = require('express');

let router = express.Router();

router.get('/insert',(request,response)=>{

    response.setHeader('Access-Control-Allow-Origin', '*')
    response.send('成功添加图书！')
})

module.exports = router;
```

![image-20220604103257936](010-Use-Axios-Send-GET-Request.assets/image-20220604103257936.png)
