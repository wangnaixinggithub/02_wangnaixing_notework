# NodeJs-Express-Body-Parser 

# 1、Download

```shell
npm install express body-parser --save
```

# 2、Need Config

```javascript
const express = require('express')
const path = require('path')
const cors = require('cors')
const bodyParser = require('body-parser')

//1.Import
const session = require('express-session')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()


app.use('/static',express.static('static'))


app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)

//2.Need Config

// 1。For parsing application/json
app.use(express.json());

// 2.For parsing application/x-www-form-urlencoded
app.use(express.urlencoded({ extended:true }));




app.post('/requestParam',(req,resp)=>{
    //将表单的name-value 变为JSON对象 封装进=> req.body
    console.log(req.body)

})

app.post('/jsonParam',(req,resp)=>{
    //将传输的JSONStr 变为JSON对象 封装进 => req.body
    console.log(req.body)
})






app.use(cors)

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})



```

# 3、Validation Test

## 3.1、application/x-www-form-urlencoded

```javascript
app.post('/requestParam',(req,resp)=>{
    //将表单的name-value 变为JSON对象 封装进=> req.body
    console.log(req.body)
    return resp.send({status:403,msg:'访问成功'})

})
```

- 可以看到，成功把表单数据封装进来`req.body`

![image-20220614011126554](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220614011126554.png)

## 3.2、application/json

- 可以看到，成功把JSON数据封装进来`req.body`

```javascript
app.post('/jsonParam',(req,resp)=>{
    //将传输的JSONStr 变为JSON对象 封装进 => req.body
    console.log(req.body)
    return resp.send({status:200,msg:'访问成功'})
})

```

![image-20220614011504178](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220614011504178.png)