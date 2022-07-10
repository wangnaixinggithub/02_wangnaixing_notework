# Vue-filters-Can-Accept-Method-Param

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
    <!--输入过滤器形参-->
    <h1>{{message|capitalize|maxLength(13)}}</h1>

</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
          message:"wangnaxing Is XXXXXXXXXXXXXXXXXXXXXXX"
        },
        filters:{
            capitalize(str){
                return str.charAt(0).toUpperCase()+str.slice(1)
            },
            //过滤器可以接受参数
            maxLength(str,length = 10){
               if (str.length <= length)  return str
               return str.slice(0,length) + '...'
            }
        }

    })
</script>
</body>
</html>
```

