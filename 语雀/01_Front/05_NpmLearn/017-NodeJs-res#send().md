# NodeJs-res#send()

> 等价于SpringMvc的Handle方法的返回值。指代响应给浏览器的东西。

```java
const express = require('express')
const fs = require('fs')
const path = require('path')

const app = new express()

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})

//1.返回HTML文档
app.get('/user/sendHtml',(req, res) =>{
    res.setHeader('Content-Type', 'text/html')
    fs.readFile(path.join(__dirname,'/1.html'),'UTF-8',(err,data) => {
         res.send(data)
    })
})


//2.返回文本
app.get('/user/sendText',(req, res) =>{
    res.setHeader('Content-Type', 'text/plain')

    res.send('王乃醒是大帅哥！')
})

//3.返回JSON
app.get('/user/sendJsonObj',(req, res) =>{
    res.setHeader('Content-Type', 'text/json')
    let obj = {
        id: 1,
        name:'王乃醒',
        age:26,
        desc:'王乃醒是大帅哥！'
    }
    res.send(obj)
})

//4.返回JSON数组
app.get('/user/sendJsonArray',(req, res) =>{
    res.setHeader('Content-Type', 'text/json')
    function User(id,name,age,desc) {
        this.id = id
        this.name = name
        this.age = age
        this.desc = desc
    }
    let user1 = new User(1,'王乃醒',26,'王乃醒是大帅哥！')
    let user2 = new User(2,'黄洁莹',24,'黄洁莹是大美女！')

    let userArr = [user1,user2]

    res.send(userArr)
})




```

