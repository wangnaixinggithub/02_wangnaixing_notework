# Nginx-Load-balancing-backup

> 声明某台Server成为备用机。可以配置backup

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
  #1、backup的server将成为备用机，如果 192.168.206.136 不提供服务了就会调用备用机取用
   upstream httpd {
	  server 192.168.206.136:80 weight=1;
      server 192.168.206.137:80 weight=2 backup;
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
请求：http://192.168.206.138 如跳转到http://192.168.206.136机器上。
如果此时，我停止http://192.168.206.136 的服务。
那么备用机的机制就会启用,此时请求就被分配给 http://192.168.206.137 执行。
```

 	