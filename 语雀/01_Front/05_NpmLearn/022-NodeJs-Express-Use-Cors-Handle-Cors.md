# Node-Express-Use-Cors-Handle-Cors

# 1、Download

```shell
npm install cors 
```

# 2、Import

```shell
const cors = require('cors')
```

# 3、Use It

```javascript
const express = require('express')
const path = require('path')
//1.Import
const cors = require('cors')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()


app.use('/static',express.static('static'))




app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)


app.use((err,req,resp,next) =>{
    console.log(`发生错误=>${err.message}`)
})

//2.Use It
app.use(cors)
app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})



```

