# NodeJs-Express-Access-Static-Resource

# 1、Need Config

```java
const express = require('express')
const path = require('path')

const userRouter = require(path.join(__dirname,'/router/user'))
const productRouter = require(path.join(__dirname,'/router/product'))
const orderRouter = require(path.join(__dirname,'./router/order'))

const app = new express()
app.use(userRouter)
app.use(productRouter)
app.use(orderRouter)

 //路径映射
app.use('/static',express.static('static'))

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})

```



![image-20220613090339147](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220613090339147.png)

# 2、Access Path

```javascript
http://localhost:8080/static/js/1.js
http://localhost:8080/static/css/index.css
```



