# Vue-v-model.lazy

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-v-model修饰符的使用</title>
</head>
<body>
<div id="app">
    <h2>v-model修饰符</h2>
    <!--
     不用lazy,默认情况是实时更新数据。
     加上lazy，从输入框失去焦点，只有按下enter都会更新数据
     -->
    <input type="text" v-model.lazy="message">
    <div>{{message}}</div>
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

