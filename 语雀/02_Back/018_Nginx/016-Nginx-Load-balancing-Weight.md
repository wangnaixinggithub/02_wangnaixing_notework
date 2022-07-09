# Nginx-Load-balancing-Weight

> 配置权重越大，则该服务器就得到更多的请求。一遍用于两台服务器带宽不一样，带宽高，我们通常加权重。

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
  #1、权重越大，请求交给他处理的机会越多。
   upstream httpd {
	server 192.168.206.136:80 weight=1;
    server 192.168.206.137:80 weight=2;
	server 192.168.206.139:80 weight=8;
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
请求：http://192.168.206.133/  
会轮询到  http://192.168.206.136 http://192.168.206.137 http://192.168.206.139 其中 139 轮询到的机会最大！
```

 	