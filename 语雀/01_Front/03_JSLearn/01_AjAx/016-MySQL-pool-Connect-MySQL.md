# MySQL-pool-Connect-MySQL

```html
let express = require('express')
let router = express.Router()

let mysql = require('mysql')

router.get('/list', function (req, res, next) {
    let bookList = []

    //1.创建数据库连接池
    let pool = mysql.createPool({
        connectionLimit: 10,
        host: 'localhost',
        user: 'root',
        password: 'root',
        database: 'express_db'
    })

    //2.使用数据库连接池 进行开启一次连接
    pool.getConnection(function (err, connection) {
        
     //3.执行一次查询
    let sql = 'select book_id, book_name, book_price, book_publisher from t_book'
    connection.query(sql, (error, results, fields) => {
        bookList = deepClone(results)
       
        let option = {
            'title': '王乃醒的收藏图书',
            'bookList': bookList
        }
        
        res.render('book/list', option)

        })
    
        //4.释放掉这次连接
        connection.release()
    })
    
  

})

/**
 * 对象深度克隆
 * @param obj
 * @returns {*[]}
 */
function deepClone(obj) {
    let objClone = Array.isArray(obj) ? [] : {}
    if (obj && typeof obj === "object") {
        for (let key in obj) {
            if (obj.hasOwnProperty(key)) {
                obj[key] && typeof obj[key] === "object" ?
                    objClone[key] = deepClone(obj[key]) : objClone[key] = obj[key]
            }
        }
    }

    return objClone
}


module.exports = router

```

