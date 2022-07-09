# MySQL-Connect-MySQL

> https://github.com/mysqljs/mysql

# 1、Download

```
npm install mysql
```

# 2、Use It

```javascript
let express = require('express');
let router = express.Router();
//Need Import@@=_=
let mysql  = require('mysql');

router.get('/list', function(req, res, next) {

    //1.创建数据库连接
    let connection = mysql.createConnection({
        host     : 'localhost',
        user     : 'root',
        password : 'root',
        database : 'express_db'
    });

    //2.进行开启一次连接
    connection.connect();

    let bookList = []

    //3.执行一次查询
    let sql = 'select book_id, book_name, book_price, book_publisher from t_book'
    connection.query(sql,  (error, results, fields) =>{

        bookList =  deepClone(results)

        let option = {
            'title':'王乃醒的收藏图书',
            'bookList':bookList
        }


        res.render('book/list', option);

    });




    //4.关闭一次连接
    connection.end();


});

/**
 * 对象深度克隆
 * @param obj
 * @returns {*[]}
 */
function deepClone(obj){
    let objClone = Array.isArray(obj) ? [] : {};
    if(obj && typeof obj==="object"){
        for(let key in obj){
            if(obj.hasOwnProperty(key)){
                obj[key] && typeof obj[key] ==="object" ?
                    objClone[key] = deepClone(obj[key]) :  objClone[key] = obj[key];
            }
        }
    }

    return objClone;
}


module.exports = router;

```

```jade
html
    head
        title 图书列表
    body
        h3  #{title}
        div
            table(border=1)
                tr
                    td 图书ID
                    td 图书名称
                    td 图书价格
                    td 图书出版社
                each item in bookList
                    tr
                        td=item.book_id
                        td=item.book_name
                        td=item.book_price
                        td=item.book_publisher
```

![image-20220604135332504](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604135332504.png)

![image-20220604135349194](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220604135349194.png)