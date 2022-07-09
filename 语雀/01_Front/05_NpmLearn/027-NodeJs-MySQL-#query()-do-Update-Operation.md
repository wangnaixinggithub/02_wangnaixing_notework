# NodeJs-MySQL-#query()-do-Update-Operation

```javascript
router.get('/order/update', (req, resp)=>{
    let connection = mysql.createConnection({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    });
    const user = {
        id:1,
        nickname:'刘备',
        age:24,
        sex:'男'
    }
    connection.connect()
    let sql = 'update t_user set age = ?,nickname = ?,sex = ? where id = ? '


    connection.query(sql,[user.age,user.nickname,user.sex,user.id],(err,result)=>{
        if (Object.is(result.affectedRows,1)){
            console.log('更新数据成功！')
        }
    })
    
    connection.close()


})

```

- 如果修改对象和`sql`描述插入字段一一对应。可进一步简写。

```javascript

router.get('/order/update', (req, resp)=>{
    let connection = mysql.createConnection({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    });
    
    const user = {
        id:3,
        nickname:'刘备',
        age:26,
        sex:'男'
    }
    
    connection.connect()
    let sql = 'update  t_user set ?  where id = ? '


    connection.query(sql,[user,user.id],(err,result)=>{
        if (Object.is(result.affectedRows,1)){
            console.log('修改数据成功！')
        }
    })
    connection.close()


})

```

