# v-if、v-else、v-else-if

v-if用于条件判断，判断Dom元素是否需要显示。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-v-if的使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h2 v-if="isFlag">isFlag为true显示这个</h2>
    
    <h2 v-show="isShow">isShow为true是显示这个</h2>
  
    
    <div v-if="age<18">小于18岁未成年</div>
    <div v-else-if="age<60">大于18岁小于60岁正值壮年</div>
    <div v-else>大于60岁,暮年</div>
    
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            isFlag:true,
            isShow:false,
            age:66
        }

    })
</script>
</body>
</html>
```

