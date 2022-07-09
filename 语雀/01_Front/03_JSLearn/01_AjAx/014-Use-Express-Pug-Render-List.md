# Use-Express-Pug-Render-List

# 1、Express Server

```javascript
var express = require('express');
var router = express.Router();

router.get('/list', function(req, res, next) {
    
    //1.假设这是数据库查询到的后端数据
    let book1 = {'bookId':1,'bookName':'Java','bookPrice':39.9,'bookPublisher':'人民日报出版社'}
    let book2 = {'bookId':2,'bookName':'Python','bookPrice':99.9,'bookPublisher':'光明日报出版社'}
    let book3 = {'bookId':3,'bookName':'Linux','bookPrice':78.9,'bookPublisher':'机械工业出版社'}

    let bookArray =  new Array(book1,book2,book3)

    let option = {
        'title':'王乃醒的收藏图书',
        'bookList':bookArray
    }
    
    //2.执行渲染
    res.render('book/list', option);
});

module.exports = router;

```

# 2、Pug render

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
                        td=item.bookId
                        td=item.bookName
                        td=item.bookPrice
                        td=item.bookPublisher
```

![](014-Use-Express-Pug-Render-List.assets/image-20220604125716086.png)