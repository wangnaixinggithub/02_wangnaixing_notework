# Nginx-Load-balancing-down

> 让服务器下线，不提供服务了。可以配置down

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
  #1、down的server将不会参与负载均衡了
   upstream httpd {
	  server 192.168.206.136:80 weight=1;
      server 192.168.206.137:80 weight=2;
	  server 192.168.206.139:80 weight=8 down;
    }
 
    server {
        listen       80;
        server_name  www.wangnaixing.top;
        location / {
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

```shell
systemctl reload nginx
```

如果不是，可以使用此命令刷新，Nginx配置。

```shell
nginx -s reload
```

# 3、Validation Test

```properties
请求：http://192.168.206.138/ 
只会请求到 http://192.168.206.136  http://192.168.206.137
```

 	