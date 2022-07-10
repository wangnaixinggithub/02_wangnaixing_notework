# Vue-v-model.number

`number`,默认是string类型，使用`number`复制为number类型。

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
      不用number,默认是string类型。
      加上number，赋值为number类型。
     -->
    <h3>修饰符number,默认是string类型，使用number赋值为number类型</h3>
    <input type="number" v-model.number="age">
    <div>{{age}}--{{typeof age}}</div>
    
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

