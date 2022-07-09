## v-text

> v-text会覆盖DOM元素中的数据，相当于JS的innerHTML方法。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-v-text指令的使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--1.会覆盖啧啧啧,功能类似插值表达式-->
    <h2 v-text="message">，啧啧啧</h2>
    <!--2.不覆盖渍渍渍-->
    <h2>{{message}}，啧啧啧</h2>
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

