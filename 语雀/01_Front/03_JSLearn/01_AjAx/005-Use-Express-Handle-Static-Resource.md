# Express-Handle-Static-Resource

# 1、Need Config



```javascript
let createError = require('http-errors');
let express = require('express');
let path = require('path');
let cookieParser = require('cookie-parser');
let logger = require('morgan');

let indexRouter = require('./routes/index');
let usersRouter = require('./routes/users');

let app = express();


app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');

app.use(logger('dev'));

app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.use(cookieParser());
//静态资源 => public 目录。
app.use('/static',express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/user', usersRouter);


app.use(function(req, res, next) {
  next(createError(404));
});


app.use(function(err, req, res, next) {
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;

```

![image-20220603160751699](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220603160751699.png)

```
http://localhost:3000/static/images/689398.jpg
http://localhost:3000/static/javascripts/my.js
http://localhost:3000/static/stylesheets/style.css
```

