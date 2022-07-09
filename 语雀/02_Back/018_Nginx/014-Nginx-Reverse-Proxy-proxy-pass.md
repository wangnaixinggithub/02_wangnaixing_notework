# Nginx-Reverse-Proxy-proxy-pass

# 1、Nginx Config

```nginx
worker_processes  1;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
	
 
    server {
        listen       80;
        server_name  www.wangnaixing.top;
        location / {
            #配置代理到：http://atguigu.com/
            proxy_pass http://atguigu.com/;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

# 2、Reload Nginx

之前，我们已经把Nginx注册成为系统服务了，所以可以使用这个命令进行刷新配置。

```shell
systemctl reload nginx
```

如果不是，可以使用此命令刷新，Nginx配置。

```
nginx -s reload
```

# 3、Validation Test

访问：`www.wangnaixing.top`时，请求交给Nginx服务器处理，返回`302` 状态码，让浏览器执行重定向操作，跳转到Location的地址值`http://www.atguigu.com`

![image-20220705081041700](014-Nginx-Reverse-Proxy-proxy-pass.assets/image-20220705081041700.png)