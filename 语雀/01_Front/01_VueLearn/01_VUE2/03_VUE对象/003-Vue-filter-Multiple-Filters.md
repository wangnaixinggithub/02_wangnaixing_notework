# Vue-filter-Multiple-Filters

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
    <!--过滤器可以串联地进行调用-->
    <h1>{{message|capitalize|maxLength}}</h1>

</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
          message:"wangnaxing Is XXXXXXXXXXXXXXXXXXXXXXX"
        },
        filters:{
            //过滤字符串，使得字符串首字母大写
            capitalize(str){
                return str.charAt(0).toUpperCase()+str.slice(1)
            },
            //限制字符串最大长度
            maxLength(str){
               if (str.length <= 10)  return str
               return str.slice(0,11) + '...'
            }
        }

    })
</script>
</body>
</html>
```

