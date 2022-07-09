# Vue-@click.prevent

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">

    <!-- 2.  按钮类型为submit的时候，默认会把表单做服务器提交，而.prevent会阻止这一表单提交的默认行为。   底层调用event.preventDefault阻止默认行为 -->
    <form action="www.baidu.com">
        <button type="submit" @click.prevent="submitClick">提交</button>
    </form>
    
    <!--a标签默认的会跳转到href的链接页面，而.prevent会阻止这一表单提交的默认行为。-->
    <a href="https://www.baidu.com" @click.prevent="hrefClick">阻止跳转</a>

</div>
<script>
    const app = new Vue({
        el:"#app",
        methods: {
            submitClick(){
                console.log("提交被阻止了,不会进到这个方法的哈哈哈")
            },
            hrefClick(){

            }


        }

    })
</script>
</body>
</html>
```

