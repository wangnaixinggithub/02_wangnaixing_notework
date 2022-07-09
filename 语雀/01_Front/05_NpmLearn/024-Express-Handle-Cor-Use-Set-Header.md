# Express-Handle-Cor-Use-Set-Header

# 1、Default Cors

> 跨域，被浏览器方面认为，这是一个不安全的行为。所以有了许多限制固定。

- 1、默认情况下，CORS 仅支持向客户端向服务器发送如下的9个`请求头`：

```properties
Accept、Accept-Language、Content-Language、
DPR、Downlink、Save-Data、
Viewport-Width、Width 、Content-Type （值仅限于 text/plain、multipart/form-data、application/x-www-form-urlencoded 三者之一）
```

- 2、默认情况下，CORS 仅支持客户端发起 GET、POST、HEAD 请求。
- 3、会把请求进行分类，分为简单请求和预检请求。

```properties
简单请求:

同时满足以下两大条件的请求，就属于简单请求：
请求方式：GET、POST、HEAD 三者之一
HTTP 头部信息不超过以下几种字段：无自定义头部字段、Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width 、Content-Type（只有三个值application/x-www-form-urlencoded、multipart/form-data、text/plain）

预检请求:
只要符合以下任何一个条件的请求，都需要进行预检请求：
 
 1、请求方式为 GET、POST、HEAD 之外的请求 Method 类型
 
 2、请求头中包含自定义头部字段
 
 3、向服务器发送了 application/json 格式的数据
 
 
简单请求的特点：客户端与服务器之间只会发生一次请求。

预检请求的特点：客户端与服务器之间会发生两次请求，OPTION 预检请求成功之后，才会发起真正的请求。

```

# 2、Update Default Cors

> 我们可以修改默认的跨域行为。

```javascript
router.put('/order/update', (req, resp)=>{

    //1.允许请求的源域 可以是任何IP，等价于此请求开启了跨域
    resp.setHeader('Access-Control-Allow-Origin', '*')
    
    //2.设置：客户端可以额外的向服务器 发送 Content-Type X-Custom-Header 这两个请求头
    resp.setHeader('Access-Control-Allow-Headers', 'Content-Type,X-Custom-Header')

    //3.设置：允许所有的Http请求方法
    resp.setHeader('Access-Control-Allow-Methods','*')
    
})

```

