# V-for

# 8.1 v-for for Arrays

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-for遍历数组</title>
</head>
<body>
<div id="app">
    <!-- 1.遍历过程没有使用索引（下标值） -->
    <ul>
        <li v-for="item in names" :key="index" >{{item}}</li>
    </ul>

    <!-- 2.遍历过程有使用索引（下标值） -->
    <ul>
        <li v-for="(item,index) in names"  :key="index" >{{index+":"+item}}</li>
    </ul>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            names:["王乃醒","王乃醒2","王乃醒3"]
        }
    })
</script>
</body>
</html>
```

# 8.2 v-for for Object

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-v-for遍历对象</title>
</head>
<body>
<div id="app">
    
    <!-- 1.遍历过程没有使用key-->
    <!-- 格式为：key-value -->
    <ul>
        <li v-for="(value,key) in user" >{{key+"-"+value}}</li>
    </ul>

    <!-- 2.遍历过程使用索引和key-->
    <!-- 格式为：key-value-index -->
    <ul>
        <li v-for="(value,key,index) in user" >{{key+"-"+value+"-"+index}}</li>
    </ul>
    

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            user:{
                id:1,
                name:"王乃醒",
                height:140,
                age:25
            }
        }
    })
</script>
</body>
</html>
```

