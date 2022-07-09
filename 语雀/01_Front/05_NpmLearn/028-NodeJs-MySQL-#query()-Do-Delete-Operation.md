# NodeJs-MySQL-#query()-Do-Delete-Operation

```java
router.get('/order/delete', (req, res)=>{
    let connection = mysql.createConnection({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    })

    let deleteId = 1
    let sql = 'delete from  t_user   where id = ? '
    connection.connect()

    connection.query(sql, deleteId, (error,results)=>{
        if (Object.is(results.affectedRows,1)){
            console.log('删除数据成功！')
        }
    })
    connection.close()

})
```

