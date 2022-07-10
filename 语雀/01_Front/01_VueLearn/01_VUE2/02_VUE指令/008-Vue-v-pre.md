# v-pre

 有时候我们期望直接输出`{{message}}`这样的字符串，而不是被`{{}}`语法转化的message的变量值，此时我们可以使用`v-pre`标签。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-v-pre指令的使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--会直接输出 {{message}},相当于让模型绑定失效-->
    <h2 v-pre>{{message}}</h2>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"王乃醒是帅哥！！"
        }
    })
</script>
</body>
</html>
```

