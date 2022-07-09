# JS-async-await-Handle-Promise-Question

> 使用async  await 解决异步回调的问题

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
                //1、在方法上使用 async
              async  handler(newValue){
                   if (newValue !== '') return
                  
                  //2、在axios请求上 加 awati 关键字 就可以直接得到响应数据
                  let response =  await  axios.get('http://localhost:8080/getUserName')
                  console.log(response)

                },
            
                immediate: true
            }


        }
    })
</script>
</body>
</html>
```

Result:

![image-20220707122429853](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220707122429853.png)