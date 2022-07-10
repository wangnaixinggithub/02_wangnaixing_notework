# Mustache

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-vue的插值表达式</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--1.插值表达式支持字符串相加-->
    <h2>{{myName + myDesc}}</h2>
    <!--2.插值表达式非数据模型的字符串做插入-->
    <h2>{{myName + "-" + myDesc}}</h2>
    <!--3.取出多个数据模型-->
    <h2>{{myName}} {{myDesc}}</h2>
    <!--4.支持数量 +2 -->
    <h2>{{count + 2}}</h2>
    <!--5.支持JavaScript语法-->
    <h2>{{message.split('').reverse().join('')}}</h2>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"你好啊 张三丰",
            myName:"王乃醒",
            myDesc:"是大帅哥！！",
            count:100

        }
    })
</script>
</body>
</html>
```

