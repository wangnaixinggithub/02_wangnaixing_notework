# Use-Axios-Send-POST-Request

# 1、One Way

- 发送JSON 请求参数。请求数据必须是JSON.

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
    let requestParam = {
        'bookId':1,
        'bookName':'Java',
        'bookPrice':101.10
    }
    axios.post('http://localhost:3000/book/insert',JSON.stringify(requestParam)).then(response=>{
        console.log(response)
    })
}
</script>

</body>
</html>
```

# 2、Two Way

- 发送FromData参数，类似表单POST请求，请求参数会被自动转换为JSON

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
    let requestParam = {
        'bookId':1,
        'bookName':'Java',
        'bookPrice':101.10
    }

    let formData = new FormData()

    formData.append('bookId','1')
    formData.append('bookName','Java')
    formData.append('bookPrice','101.10')

    axios.post('http://localhost:3000/book/insert',formData).then(response=>{
        console.log(response)
    })
}
</script>

</body>
</html>
```

#   3、Three Way

- 发送POST 表单请求，请求参数会自动转换为JSON.

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
    let requestParam = {
        'bookId':1,
        'bookName':'Java',
        'bookPrice':101.10
    }
	
    //表单请求需要设置请求头类型
    let config = {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'},
    }
    axios.post('http://localhost:3000/book/insert',requestParam,config).then(response=>{
        console.log(response)
    })
}
</script>

</body>
</html>
```

![image-20220604102750466](011-Use-Axios-Send-POST-Request.assets/image-20220604102750466.png)
