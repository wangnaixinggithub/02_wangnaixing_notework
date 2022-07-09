# 

# 1、测试登录以后返回Token

# 1、case1:登录成功



输入参数：

```json
{
  "password": "123456",
  "username": "test"
}
```

响应结果：

```json
{
  "code": 200,
  "message": "操作成功",
  "data": {
    "tokenHead": "Bearer ",
    "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0IiwiY3JlYXRlZCI6MTY1NzA5NDIwMjA1OCwiZXhwIjoxNjU3Njk5MDAyfQ.c4d_DzOJD8oT0UkTly3wAnJVbSAZruDQF_bK6rHAD3mNB9YykUJITfSPnPgMsVE8drq1xzB9ZNXS6f7UY-vo6A"
  }
}
```

# 2、case2：登录失败

输入参数：

```json
{
  "password": "1234567",
  "username": "test"
}
```

响应结果：

```json
{
  "code": 500,
  "message": "密码不正确",
  "data": null
}
```

