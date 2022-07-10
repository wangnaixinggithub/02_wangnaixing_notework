# v-html

在某些时候我们不希望直接输出`<a>http://www.baidu.com'>百度一下</a>`这样的字符串，而输出被HTML自己转化的超链接。此时可以使用`v-html`。

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
	<!--不使用v-html,Vue会把数据模型当成字符串直接解析-->
    <h2>{{url}}</h2>
   <!-- 使用v-html，Vue会把数据模型当成网页标签做解析！-->
    <h2 v-html="url"></h2>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            url:"<a href='http://www.baidu.com'>百度一下</a>"
        }
    })
</script>
</body>
</html>
```

