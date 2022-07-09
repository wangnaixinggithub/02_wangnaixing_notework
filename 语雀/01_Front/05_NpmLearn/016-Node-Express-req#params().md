# Node-Express-req#params()

# 1、request.param

> 和SpringMvc的路径占位符很相似。。。

```javascript
const express = require('express')

const app = new express()

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})

app.get('/user/findById/:id/:name',(req, res) =>{
    //req.query 是一个对象，封装了查询字符串的内容。
   console.log(req.params)
})

```

# 2、Validation Test

![img](file:///C:\Users\Administrator.DESKTOP-E0KTJ20\AppData\Roaming\Tencent\Users\827376239\QQ\WinTemp\RichOle\VK~N1HZ4OCIT`88YTZ`Z}}T.png)



