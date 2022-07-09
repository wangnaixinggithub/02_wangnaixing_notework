# NodeJs-Express-Middleware-Implement-Preprocessing

# 1、Global Middleware

- 使用全局路由中间件

```javascript
const express = require('express')
const path = require('path')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()


app.use('/static',express.static('static'))


//1.声明全局路由中间件
const mw1 = (req,res,next) => {
    console.log('这是一个简单的中间件函数1。。。。')
    next()
}
const mw2 = (req,res,next) =>{
    console.log('这是一个简单的中间件函数2。。。。')
    next()
}
const mw3 = (req,res,next) =>{
    console.log('这是一个简单的中间件函数3。。。。')
    next()
}

//2.在路由注册之前，先注册路由中间件。
app.use(mw1)
app.use(mw2)
app.use(mw3)


app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})



```

- 效果是，所有的请求，先走中间件的流程。

```javascript
router.get('/order/findAll', (req, resp)=>{
    console.log('Order findAll 被执行了')
    resp.send('Order findAll 被执行了')
})
```

```javascript
http://localhost:8080/order/findAll
```

![image-20220613093745125](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220613093745125.png)



# 2、Part Middleware

- 使用局部路由中间件，和指定的业务方法绑定

```javascript
const express = require('express')
let router = express.Router()


//1.声明局部路由中间件
const mw1 = (req,res,next) => {
    console.log('这是一个简单的中间件函数1。。。。')
    next()
}
const mw2 = (req,res,next) =>{
    console.log('这是一个简单的中间件函数2。。。。')
    next()
}
const mw3 = (req,res,next) =>{
    console.log('这是一个简单的中间件函数3。。。。')
    next()
}


//2.和指定路由进行绑定操作
router.get('/order/findAll',mw1,mw2,mw3, (req, resp)=>{
    console.log('Order findAll 被执行了')
    resp.send('Order findAll 被执行了')
})

router.post('/order/add', (req, res)=>{

})

router.delete('/order/delete', (req, res)=>{

})

router.put('/order/update', (req, res)=>{

})
module.exports = router
```

- 使用局部路由，和整个路由绑定

```javascript
const express = require('express')
let router = express.Router()


//1.声明局部路由中间件
const mw1 = (req,res,next) => {
    console.log('这是一个简单的中间件函数1。。。。')
    next()
}
const mw2 = (req,res,next) =>{
    console.log('这是一个简单的中间件函数2。。。。')
    next()
}
const mw3 = (req,res,next) =>{
    console.log('这是一个简单的中间件函数3。。。。')
    next()
}

//2.和整个路由实例进行绑定，必须在下面业务方法前 => 绑定
router.use(mw1)
router.use(mw2)
router.use(mw3)

router.get('/order/findAll', (req, resp)=>{
    console.log('Order findAll 被执行了')
    resp.send('Order findAll 被执行了')
})

router.get('/order/add', (req, resp)=>{
    console.log('Order add 被执行了')
    resp.send('Order add 被执行了')
})

router.delete('/order/delete', (req, res)=>{

})

router.put('/order/update', (req, res)=>{

})



module.exports = router
```

- 错误路由的定义和注册

```javascript
const express = require('express')
const path = require('path')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()


app.use('/static',express.static('static'))




app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)

//错误路由的定义和注册
app.use((err,req,resp,next) =>{
    console.log(`发生错误${err.message}`)
})
app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})


const express = require('express')
const path = require('path')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()


app.use('/static',express.static('static'))




app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)

//错误路由的定义和注册
app.use((err,req,resp,next) =>{
    console.log(`发生错误=>${err.message}`)
})


app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})


```

- 测试验证

```javascript
router.get('/order/findAll', (req, resp)=>{
    //制造自定义的异常
    console.log('Order findAll 被执行了')
    
    throw new Error('这是王乃醒定义的自定义异常哦！！！')

    resp.send('Order findAll 被执行了')
})
```

- 调用接口，可以发现，业务方法抛出异常之后，会进入到错误路由中间件中，执行他的逻辑。
