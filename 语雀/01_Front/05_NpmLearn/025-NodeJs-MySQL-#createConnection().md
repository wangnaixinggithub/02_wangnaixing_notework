# NodeJs-MySQL-#createConnection()

# 1、Download

```shell
npm install mysql
```



# 2、Use It

```javascript
const express = require('express')
let router = express.Router()
const mysql  = require('mysql');




router.get('/user/findAll', (req, resp)=>{

    //1.创建本地MySQL数据库连接对象
    let connection = mysql.createConnection({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    });

    //2.开启连接
    connection.connect();

    //3.执行一次SQL查询
    let sql = 'SELECT * from t_user'
    connection.query(sql,  (error, results, fields) => {
        console.log(results)

        //[
        //   RowDataPacket { id: 1, nickname: '张三', age: 25, sex: '男' },
        //   RowDataPacket { id: 2, nickname: '李四', age: 26, sex: '女' },
        //   RowDataPacket { id: 3, nickname: '赵四', age: 28, sex: '男' },
        //   RowDataPacket { id: 4, nickname: '王乃醒', age: 26, sex: '男' },
        //   RowDataPacket { id: 5, nickname: '黄洁莹', age: 25, sex: '男' }
        // ]

    });

    //4.关闭连接
    connection.end();



    resp.send('执行成功！')
})





module.exports =outer
```

# 3、Handle Access Exception

- 异常得控制台打印，MySQL框架不支持当下`MySQL8`的加密方式，Node还不支持。

```shell
[SELECT ERROR] - ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
```

- MySQL中执行此两条SQL,可解决。

```sql
alter user 'root'@'localhost' identified with mysql_native_password by 'root';
flush privileges;
```



# 