# Nginx-Implementation-Load-balancing

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
  #1、声明服务器组 实现负载均衡
   upstream httpd {
	server 192.168.206.134:80;
	server 192.168.206.135:80;
    ｝
 
    server {
        listen       80;
        server_name  www.wangnaixing.top;
        location / {
           # 2、开启反向代理到服务器组执行轮询
            proxy_pass  http://httpd;
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

```
systemctl reload nginx
```

如果不是，可以使用此命令刷新，Nginx配置。

```
nginx -s reload
```

# 3、Validation Test

```
请求：http://192.168.206.133/  
会轮询到 192.168.206.134:80;  192.168.206.135:80;
```

![image-20220706004622889](015-Nginx-Implementation-Load-balancing.assets/image-20220706004622889.png)

![image-20220706004631078](015-Nginx-Implementation-Load-balancing.assets/image-20220706004631078.png)