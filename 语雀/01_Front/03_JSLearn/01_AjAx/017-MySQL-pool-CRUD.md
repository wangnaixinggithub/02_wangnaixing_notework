# MySQL-pool-CRUD

# 1、findById

```javascript
router.get('/findById',(req, res) => {


    let pool = mysql.createPool({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    })

    pool.getConnection(function (err, connection) {
        //假设前端获取
        let bookId = 1

        let sql = 'select book_id, book_name, book_price, book_publisher from t_book where book_id = '+connection.escape(bookId)
        connection.query(sql, (error, results, fields) => {
            res.send(results)
        })


        connection.release()
    })


})

```

![image-20220604141244491](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604141244491.png)

# 2、Insert

```javascript
router.get('/insert',(req, res) => {
	
    //假设前端得到
    let bookInsertParam = {
        'book_id':null,
        'book_name':'MySQL',
        'book_price':48.64,
        'book_publisher':'广州出版社',
    }

    let pool = mysql.createPool({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    })
    pool.getConnection(function (err, connection) {

        let sql = 'insert into t_book set ? '
        connection.query(sql,bookInsertParam, (error, results, fields) => {
            res.send(results)

        })

        connection.release()
    })

})
```



![image-20220604141752601](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604141752601.png)

![image-20220604141844433](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604141844433.png)

# 3、Update

```javascript
router.get('/update',(req, res) => {

    let bookId = 3

    let param = ['MySQL2','广州出版社2',48.22,3]

    let pool = mysql.createPool({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    })
    pool.getConnection(function (err, connection) {

        let sql = 'update t_book set book_name = ?,book_publisher = ?,book_price = ? where book_id = ?'
        connection.query(sql,param, (error, results, fields) => {
            res.send(results)

        })

        connection.release()
    })

})
```

![image-20220604145007297](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604145007297.png)

# 4、Delete

```javascript
router.get('/delete',(req, res) => {

    let pool = mysql.createPool({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    })

    pool.getConnection(function (err, connection) {

        let sql = 'delete from t_book where book_id = 3'
        connection.query(sql, (error, results, fields) => {
            res.send(results)

        })

        connection.release()
    })

})
```

![image-20220604145833158](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604145833158.png)