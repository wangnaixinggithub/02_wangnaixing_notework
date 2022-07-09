# NodeJs-Express-Router

- 定义处理对应的UrlMapper的HandlerMethod

```javascript
const express = require('express')

const app = new express()

app.listen(8080, () => {
    console.log("Express server listening on port 8080")
})

app.get('/user/findById',(req, res) =>{
    console.log('GET/userList...')
})

app.post('/user/add',(req, res) =>{
    console.log('POST /user/add')
})

app.delete('user/delete',(req,res) =>{
    console.log('DELETE /user/delete')
})

app.put('/user/update',(req, res) =>{
    console.log('PUT /user/update')
})
```

```javascript
http://localhost:8080/user/findById GET

http://localhost:8080/user/add  POST

http://localhost:8080/user/delete DELETE

http://localhost:8080/user/update UPDATE
```

