# Use-Express-Router-Handle-Request

# 1、mappering => method

```javascript
// user.js
let express = require('express');
let router = express.Router();


router.get('/add', function(req, res, next) {
  res.send('创建用户信息');
});

router.post('/delete',(req,resp)=>{
  resp.send('删除用户信息')
})


router.post('/update',(req,resp)=>{
  resp.send('更新用户信息')
})

router.post('/findAll',(req,resp)=>{
  resp.send('查询全部用户信息')
})




module.exports = router;

```

# 2、In App.js Config

```javascript
let createError = require('http-errors');
let express = require('express');
let path = require('path');
let cookieParser = require('cookie-parser');
let logger = require('morgan');

let indexRouter = require('./routes/index');

//1.Import userRouter
let usersRouter = require('./routes/users');

let app = express();


app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');

app.use(logger('dev'));

app.use(express.json());
app.use(express.urlencoded({ extended: false }));

app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
//2.Use usersRouter
app.use('/user', usersRouter);


app.use(function(req, res, next) {
  next(createError(404));
});


app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;

```

