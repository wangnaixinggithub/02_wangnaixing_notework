# Vue-filters-HelloWorld

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
    <!--使用过滤器-->
    <h1>{{message|capitalize}}</h1>

</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
          message:"wangnaxing"
        },
        methods: {
            handleClick(){
                console.log("哈哈哈")
            },

        },
        filters:{
            //过滤字符串，使得字符串首字母大写
            capitalize(str){
                return str.charAt(0).toUpperCase()+str.slice(1)
            }
        }

    })
</script>
</body>
</html>
```

