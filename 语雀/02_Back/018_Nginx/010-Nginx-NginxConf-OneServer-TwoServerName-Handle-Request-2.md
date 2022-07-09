# Nginx-NginxConf-OneServer-TwoServerName-Handle-Request-2

> 一个Server服务器，支持配置多个server_name域名。

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
            root   /www/project;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

	
    server {
        listen       80;
    	# 一个Server服务器，支持配置多个server_name域名。
        server_name  video.wangnaixing.top vod.wangnaixing.top;

        location / {
            root   /www/video;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }


}
```

# 2、Liunx Config

在根目录下，创建`www` 网站目录，创建`project` `video` 文件夹，分别提供index.html

```shell
# 1、Linux 根目录下创建 www 文件夹
mkdir www
# 2、进入 www
cd www
# 3、 创建 video 文件夹
mkdir video
# 4、创建 video 文件夹
mkdir project

#5、放行80端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
#6、重启防火墙
systemctl restart firewalld.service
```

```html
<!--/www/project/index.html -->
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>This is My BackProject!!!</p>
</body>
</html>
```

```html
<!--/www/video/index.html -->
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>This is My VideoProject!!!</p>
</body>
</html>
```

# 3、Reload Nginx

之前，我们已经把Nginx注册成为系统服务了，所以可以使用这个命令进行刷新配置。

```shell
systemctl reload nginx
```

如果不是，可以使用此命令刷新，Nginx配置。

```shell
nginx -s reload
```



# 4、Validation Test

- 访问video/index.html

```properties
video.wangnaixing.top
```

![image-20220705064942186](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705064942186.png)

- 访问video/index.html

```properties
vod.wangnaixing.top
```

![image-20220705064955273](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705064955273.png)