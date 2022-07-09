# JS-DOM-Use-addEventListener-Handle-li-Contrl

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

    </style>
</head>

<body>
<ul>
    <li>知否知否，点我应有弹框在手！</li>
    <li>知否知否，点我应有弹框在手！</li>
    <li>知否知否，点我应有弹框在手！</li>
    <li>知否知否，点我应有弹框在手！</li>
    <li>知否知否，点我应有弹框在手！</li>
</ul>

<script>
//1.利用事件委托特性，点击每一个li,会冒泡给ul父亲执行，触发点击监听    
document.addEventListener('click',(e)=>{
    e.target.style.backgroundColor = 'pink'
})
</script>
</body>

</html>
```

