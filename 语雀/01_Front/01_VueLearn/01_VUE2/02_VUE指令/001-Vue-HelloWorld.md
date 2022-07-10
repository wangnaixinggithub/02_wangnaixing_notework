# Hello Vue

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HelloVuejs</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
     <h1>{{user.name}}</h1>
     <h1>{{user.age}}</h1>
</div>
<script>
    const name = '王乃醒'
    const age = 26
    let user = {name,age}

   let app = new Vue(
       {
           el:'#app',
           data:{
             user:user
           }
       }

   )
</script>
</body>
</html>
```

