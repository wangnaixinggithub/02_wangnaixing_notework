# NodeJs-Express-Use-Express-JWT

# 1、Download

```shell
npm install jsonwebtoken express-jwt
```

```shell
npm install express-jwt
```

# 2、Need Config

```javascript
const express = require('express')
const path = require('path')
const cors = require('cors')
const bodyParser = require('body-parser')
const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))
const app = new express()

//1.Import
const jwt = require('jsonwebtoken') //导入生成JWT字符串的包
const {expressjwt} = require('express-jwt') //导入用于客户端发送过来的JWT字符，解析还原成JSON对象的包


//2.定义密钥
const secretKey = 'WangNaiXing is so Handsome!' //使用密钥加密和解密



/**
 * 登录认证的接口
 */
app.post('/api/login',(req,resp)=>{
    let {username,password} = req.body

    if (!Object.is(username,'wangnaixing') || !Object.is(password,'root')){
        return resp.send({status:403,msg:'登录失败'})
    }
    //1.验证成功，根据用户名生成JWT
    let jwtStr = jwt.sign({username:username},secretKey,{expiresIn: 60 * 60 * 240})

    resp.send({status:200,msg:'登录成功',data:{token:jwtStr}})
})

//2、配置Jwt解析操作，此时Express会自动对Authorization头内容使用密钥进行解析回JSON对象，并封装到req.user中
app.use(
    expressjwt({
        secret: secretKey,
        credentialsRequired: true,
        algorithms: ["HS256"],
    }).unless({ path: ["/api"] })
)

/**
 * 登录成功后，模拟取出当前用户的用户名
 */
app.get('/api/getUserName',(req,resp)=>{
    //1.获取当前登录的用户信息,并取出用户名。
    let {username} = req.user

    return resp.send({status:200,msg:'成功访问',data:{username:username}})
})


//3.处理客户端发送过来的Token字符串过期或者不和法的情况，会产生一个解析失败的错误。
app.use((err,req,resp,next) =>{
    //Token解析失败的错误
    if (Object.is(err.name,'UnauthorizedError')){
        return resp.send({status:401,msg:'无效的token'})
    }
    //其他原因错误
    return resp.send({status:500,msg:'未知错误'})

})




app.use(cors)
app.use('/static',express.static('static'))
app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)
app.use(express.json());
app.use(express.urlencoded({ extended:true }))

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})



```

# 3、Client operation

## 1、Save localStorage And sessionStorage

```java
if (response.data.Token) {
    window.localStorage.setItem('user_token', response.data.token)
    window.sessionStorage.setItem('user_token',response.data.token)
}
```

## 2、Send Request Carry Authorization

- axios

```javascript
const user_token = window.localStorage.getItem('user_token');
request.headers['Authorization'] = `Bearer ${user_token}`;
```

- Jquery

```java
// jquery提供了一个在请求之前设置Authorization的方法
$.ajaxSettings.beforeSend = function (xhr, request) {
    const user_token = window.localStorage.getItem('user_token')
    xhr.setRequestHeader('Authorization', `Bearer ${user_token}`)
}
```

