# JS-BOM-#setInterval()-Handle-Send-Message

- 通过触发器来做出发送验证码，一分钟后才能进行二次发送。

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
手机号码： <input type="number"><button>发送</button>
<script>
    let btnElement = document.querySelector('button')
    btnElement.addEventListener('click',() => {

    btnElement.disabled = true
   let time = 60
    let timer  = setInterval(()=>{
            if (Object.is(time,0)){
                
                clearInterval(timer)
                btnElement.disabled = false;
                btnElement.innerHTML = '发送'
                
            }else {
                
                btnElement.innerHTML = '还剩下' + time + '秒'
                time--
            }
        },1000)
    })
</script>
</body>
</html>
```

