# v-model

# 1、input

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-v-model的基本使用</title>
</head>
<body>
<div id="app">
    <input type="text" v-model="message">{{message}}
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"王乃醒"
        }
    })
</script>
</body>
</html>
```

v-model双向绑定，既输入框的value改变，对应的message对象值也会改变，修改message的值，input的value也会随之改变。无论改变那个值，另外一个值都会变化。

# 2、radio

 radio单选框的`name`属性是互斥的，如果使用v-model，可以不使用`name`就可以互斥。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-v-model结合radio类型</title>
</head>
<body>
<div id="app">
    <label for="male">
        <input type="radio" id="male"  value="男" v-model="sex">男
    </label>
    <label for="female">
        <input type="radio" id="female"  value="女" v-model="sex">女
    </label>
    <div>你选择的性别是：{{sex}}</div>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            sex:"男"
        },
    })
</script>

</body>
</html>
```

# 3、checkbox

 checkbox可以结合v-model做单选框，也可以多选框。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-v-model结合checkbox类型</title>
</head>
<body>
<div id="app">
    <!-- 1.checkbox多选框 多选框取值为布尔值：选中为true 不选中为false -->
    <h2>单选框</h2>
    <label for="agree">
        <input type="checkbox" id="agree" v-model="isAgree">同意协议
    </label>
    <div>你选择的结果是：{{isAgree}}</div>
    <button :disabled="!isAgree">下一步</button>
    <!-- 2.checkbox多选框 
			当选中多选框其中一项时，为true,把value值赋值数据模型hobbies
-->
    <h2>多选框</h2>
    <label :for="item" v-for="(item, index) in oriHobbies" :key="index">
        <input type="checkbox" name="hobby" :value="item" :id="item" v-model="hobbies">{{item}}
    </label>
    <div>你的爱好是：{{hobbies}}</div>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            isAgree:false, //1.checkbox单选框
            hobbies:[], // 2.checkbox多选框
            oriHobbies:["篮球","足球","羽毛球","乒乓球"] //2.多选框数据模型
        },
    })
</script>
</body>
</html>
```

1. checkbox结合v-model实现单选框，定义变量`isAgree`初始化为`false`，点击checkbox的值为`true`，`isAgree`也是`true`。
2. checkbox结合v-model实现多选框，定义数组对象`hobbies`，用于存放爱好，将`hobbies`与checkbox对象双向绑定，此时选中，一个多选框，就多一个true，`hobbies`就添加一个对象。

# 4、select

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-v-model结合select类型</title>
</head>
<body>
<div id="app">
    <!-- select单选 -->
    <select name="fruit" v-model="fruit">
        <option value="苹果">苹果</option>
        <option value="香蕉">香蕉</option>
        <option value="西瓜">西瓜</option>
    </select>
    <h2>你选择的水果是：{{fruit}}</h2>

    <!-- select多选 -->
    <select name="fruits"  v-model="fruits" multiple>
        <option v-for="(item,index) in fruitList" :key="index" :value="item" :id="item">{{item}}</option>
    </select>
    <h2>你选择的水果是：{{fruits}}</h2>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            fruit:"苹果", //1.单选数据模型
            fruits:[], //2.多选数据模型
            fruitList:["苹果","香蕉","西瓜","哈密瓜"] //2.select数据模型
        },
    })
</script>
</body>
</html>
```









