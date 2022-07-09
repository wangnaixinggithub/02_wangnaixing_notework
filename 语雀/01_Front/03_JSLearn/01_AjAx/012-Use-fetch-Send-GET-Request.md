# Use-fetch-Send-GET-Request

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
    fetch('http://localhost:3000/book/insert?bookId=1&bookName=Java&bookPrice=101.10').then(res=>{
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

