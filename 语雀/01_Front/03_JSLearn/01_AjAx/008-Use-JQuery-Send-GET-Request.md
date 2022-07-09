# Use-JQuery-Send-GET-Request

# 1、One Way

- 写在URL后面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-GET</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>

    $(document).ready(() =>{

        $("#btn1").click(() => {

            $.get('http://localhost:3000/book/insert?bookId=1&bookName=Java&bookPrice=101.10', (response,status,xhr)=>{

                console.log(response)

                console.log(status)

                console.log(xhr)
            })


        })
    })

</script>

</body>
</html>
```

# 2、Two Way

- 以查询字符串的形式写入方法参数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-GET</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>

    $(document).ready(() =>{

        $("#btn1").click(() => {

            $.get('http://localhost:3000/book/insert','bookId=1&bookName=Java&bookPrice=101.10', (response,status,xhr)=>{

                console.log(response)

                console.log(status)

                console.log(xhr)
            })


        })
    })

</script>

</body>
</html>
```

# 3、Three Way

- 以JS对象的形式。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-GET</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>

    $(document).ready(() =>{

        $("#btn1").click(() => {
            let requestParam = {
                'bookId':1,
                'bookName':'Java',
                'bookPrice':101.10
            }
            $.get('http://localhost:3000/book/insert',requestParam, (response,status,xhr)=>{

                console.log(response)

                console.log(status)

                console.log(xhr)
            })


        })
    })

</script>

</body>
</html>
```



```javascript
let express = require('express');

let router = express.Router();

router.get('/insert',(request,response)=>{

    response.setHeader('Access-Control-Allow-Origin', '*')
    response.send('成功添加图书！')
})

module.exports = router;
```

