# NodeJs-Express-Module-Router

# 1、Router Module

> 路由模块化

![image-20220612180932699](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220612180932699.png)

```javascript
//order.js
const express = require('express')
let router = express.Router()

router.get('/order/findAll', (req, res)=>{

})

router.post('/order/add', (req, res)=>{

})

router.delete('/order/delete', (req, res)=>{

})

router.put('/order/update', (req, res)=>{

})
module.exports = router
```

```javascript
//product.js
const express = require('express')
let router = express.Router()

router.get('/product/findAll', (req, res)=>{

})

router.post('/product/add', (req, res)=>{

})

router.delete('/product/delete/:id', (req, res)=>{

})

router.put('/product/update', (req, res)=>{

})

module.exports = router
```

```javascript

const express = require('express')
let router = express.Router()

router.get('/users/findAll', (req, res)=>{

})

router.post('/user/add', (req, res)=>{

})

router.delete('/user/delete/:id', (req, res)=>{

})

router.put('/user/update', (req, res)=>{

})

module.exports = router
```

# 2、app#use()

> 注册路由

```javascript
const express = require('express')
const path = require('path')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()
app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})


```

