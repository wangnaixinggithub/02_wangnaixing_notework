# Use-Express-Create-Project

# 1、Create nodeJs Project

- 一路回车，此时工程目录就会多了`package.json`

```shell
npm init
```

# 2、DownLoad

- 下载 express 核心依赖

```shell
npm install express --save
```

# 3、Open Server

- 项目根目录创建`app.js`

```javascript
const express = require('express')
//1. Create Your BackServer
const app = express()
const port = 8080

//2. Handler / Request
app.get('/', (req, res) => {
    res.send('你好Express')
})

//3. AppServer Listener
app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})
```

# 4、Start

```shell
node app.js
```

![image-20220603150034595](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220603150034595.png)