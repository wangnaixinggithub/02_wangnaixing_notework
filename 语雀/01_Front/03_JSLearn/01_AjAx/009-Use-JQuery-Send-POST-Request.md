# Use-JQuery-Send-POST-Request

# 1、One Way

- 可以接受请求参数是一个JSON对象，内部会转换为请求体。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-POST</title>

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
            
            $.post('http://localhost:3000/book/insert',requestParam, (response,status,xhr)=>{

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

- 可以接受参数是JSON字符串。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-POST</title>

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

            $.post('http://localhost:3000/book/insert',JSON.stringify(requestParam), (response,status,xhr)=>{

                console.log(response)

                console.log(status)

                console.log(xhr)
            },'JSON')


        })
    })

</script>

</body>
</html>
```

# 3、Three Way

- 可以接受参数是FormData的字符串形式。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ajax-POST</title>

</head>
<body>
<h1>Hello</h1>
<button id="btn1">按钮</button>
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script>

    $(document).ready(() =>{

        $("#btn1").click(() => {


            let formData = new FormData()

            formData.append('bookId','1')
            formData.append('bookName','Java')
            formData.append('bookPrice','101.10')

            $.post('http://localhost:3000/book/insert',JSON.stringify(formData), (response,status,xhr)=>{

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

![](009-Use-JQuery-Send-POST-Request.assets/image-20220604103951337.png)