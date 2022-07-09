# NodeJs-MySQL-#query()-Do-Create-Operation

# 1、One Way

```java
router.get('/user/insert', (req, resp)=>{
    let connection = mysql.createConnection({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    });

    connection.connect()
    const user = {
        id:null,
        nickname:'张飞',
        age:26,
        sex:'男'
    }
    const sql = 'insert into t_user (id, nickname, age, sex) values(?,?,?,?)'
    connection.query(sql,[user.id,user.nickname,user.age,user.sex],(error,results)=>{
        if (Object.is(results.affectedRows,1)){
            console.log('插入数据成功！')
        }
    })

    connection.close()

    resp.send('Order add 被执行了')
})
```

# 2、Two Way,Easy

- 如果插入对象和`sql`描述插入字段意义对应。可进一步简写。

```java
router.get('/user/insert', (req, resp)=>{
    let connection = mysql.createConnection({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    });

    connection.connect()
    const user = {
        id:null,
        nickname:'关羽',
        age:24,
        sex:'男'
    }
    //SQL写法变化了！！！
    const sql = 'insert into t_user  SET ?'
    connection.query(sql,user,(error,results)=>{
        if (Object.is(results.affectedRows,1)){
            console.log('插入数据成功！')
        }
    })

    connection.close()

    resp.send('Order add 被执行了')
})
```

