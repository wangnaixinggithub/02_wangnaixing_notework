# Linux-Config-Nginx-Become-System-Server

> 通过脚本的方式，让Nginx成为系统服务，从而不用每次进入Nginx的`sbin`目录，去启动。

# 1、Need Config

```shell
# 1、创建Nginx的服务脚本
vim /usr/lib/systemd/system/nginx.service

# 2、写入脚本内容

#3、重新加载系统服务
systemctl daemon-reload

#4、启动Nginx服务
systemctl start nginx.service

#5、设置Nginx服务为开机自启动
systemctl enable nginx.service
```

# 2、Content

```shell
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```

