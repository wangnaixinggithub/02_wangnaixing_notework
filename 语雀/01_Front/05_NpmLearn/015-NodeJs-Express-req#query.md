# NodeJs-Express-req#query

# 1、request.query

```javascript
const express = require('express')

const app = new express()

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})

app.get('/user/findById',(req, res) =>{
    //req.query 是一个对象，封装了查询字符串的内容。
   console.log(req.query)
})


```

# 2、Validation Test

```
http://localhost:8080/user/findById?id=100
http://localhost:8080/user/findById?id=100&username=zhangsan&age=30
```

![image-20220612171810350](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220612171810350.png)