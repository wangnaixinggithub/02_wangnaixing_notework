# v-show

 v-if看似和v-show实现一样的效果，但是内部v-show只是用css将操作的元素隐藏显示，而v-if是新增和删除元素。v-show只是操作元素的style属性display，都会被创建。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-v-show的使用</title>
</head>
<body>
<div id="app">
    <h2 v-show="isFlag">v-show只是操作元素的style属性display</h2>
    <h2 v-if="isFlag">v-if是新增和删除dom元素</h2>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            isFlag:true
        }
    })
</script>
</body>
</html>
```

