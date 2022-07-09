# v-cloak

>  有时候因为加载延时问题，例如卡掉了，数据没有及时刷新，就造成了页面显示从`{{message}}`到message变量“你好啊”的变化，这样闪动的变化，会造成用户体验不好。此时需要使用到`v-cloak`的这个标签。在VUE解析之前，div属性中有`v-cloak`这个标签，在VUE解析完成之后，v-cloak标签被移除。简单，类似div开始有一个CSS属性`display:none;`，加载完成之后，CSS属性变成`display:block`，元素显示出来。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-v-cloak指令的使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<!--不会显示从 {{message}} -> 王乃醒是帅哥的过程 -->
<div id="app" v-cloak>
    <h2 v-cloak>{{message}}</h2>
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

