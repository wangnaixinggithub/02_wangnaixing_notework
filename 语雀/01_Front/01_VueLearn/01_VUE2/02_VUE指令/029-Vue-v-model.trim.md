# Vue-v-model.trim

`trim`用于 自动过滤用户输入的首尾空白字符

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-v-model修饰符的使用</title>
</head>
<body>
<div id="app">
    <!--
     不用trim,默认是会把空格也用了
     用了trim,去空格.
    -->
    <input type="text" v-model.trim="name">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"zzz",
            age:18,
            name:"ttt"
        },
    })
</script>
</body>
</html>
```

