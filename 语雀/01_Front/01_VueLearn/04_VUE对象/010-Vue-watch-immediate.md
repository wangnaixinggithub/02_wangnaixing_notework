# Vue-watch-immediate

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>
<body>
<div id="app">
    <h2>{{userName}}</h2>
</div>
<script>
    const app = new Vue({
        el:"#app",

        create:{

        },
        data:{
          userName:''

        },
        watch:{
            userName:{
                handler(newValue){
                    
                    if (newValue !== '') return
                    
                    //2、从后端得到了userName赋值给数据模型
                    axios.get('http://localhost:8080/getUserName').then(res=>{
                        this.userName =  res.data.data
                    })
                },
                // 1、表示页面初次渲染好之后，就立即触发当前的 watch 侦听器
                immediate: true
            }


        }
    })
</script>
</body>
</html>
```

