# v-once

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-v-once指令的使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h2>{{message}}</h2>
    <!--数据只会绑定一次，如数据再改变则不会变动！-->
    <h2 v-once>{{message}}</h2>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"你好啊,王乃醒是帅哥！",
        }
    })
</script>
</body>
</html>
```

