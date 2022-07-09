# NodeJs-Express-Use-Express-Session

# 1、Download

```shell
npm install express-session
```

# 2、Need Config

```javascript
const express = require('express')
const path = require('path')
const cors = require('cors')
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

//2.Doing Config
app.use(session({
    secret:'Wangnaixing Secret Key',
    resave:false,
    saveUninitialized:true
}))




app.use((err,req,resp,next) =>{
    console.log(`发生错误=>${err.message}`)
})


app.use(cors)
app.use()
app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})



```



# 3、In Your Business

```java
const express = require('express')
const path = require('path')
const cors = require('cors')
const session = require('express-session')
const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))
const app = new express()
app.use('/static',express.static('static'))
app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)
app.use(session({
    secret:'Wangnaixing Secret Key',
    resave:false,
    saveUninitialized:true
}))


//3.In Your Business
    
/**
 * 登录认证的接口
 */
app.post('/api/login',(req,resp)=>{

    let {username,password} = req.body


    if (!Object.is(username,'wanganixing') || Object.is(password,'root')){
        return resp.send({status:403,msg:'登录失败'})
    }
    //1.获取Session对象
     let session =  req.session

    //2.将用户的登录状态，存到Session中
    session.user = req.body
    session.islogin = true

    resp.send({status:200,msg:'登录成功'})
})
/**
 * 登录成功后，模拟取出当前用户的用户名
 */
app.get('/api/getUserName',(req,resp)=>{
    //1.判断用户是否登录
    if (!resp.session.islogin){
        return resp.send({status:403,msg:'用户没有登录'})
    }

    //2.从Session中取出用户名
    return resp.send({status:200,msg:'成功访问',data:{username:resp.session.user.username}})
})



app.use((err,req,resp,next) =>{
    console.log(`发生错误=>${err.message}`)
})

app.use(cors)

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})



```

# 4、Validation Test

1、登录成功后，当前用户会被存入Session中，并返回`Set-Cookie`的响应头给客户端。

![image-20220614012529774](031-NodeJs-Express-Use-Express-Session.assets/image-20220614012529774.png)

2、当客户端再去请求该服务起其他资源时，会把`Set-Cookie`的内容写入请求头和当前请求一同给服务器处理。



