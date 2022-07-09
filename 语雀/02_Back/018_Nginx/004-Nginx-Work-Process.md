# Nginx-Work-Process

Master进程负责读取`nginx.conf`配置，开启多个`Worker`子进程 ,由`Worker`进行接收请求，回馈响应。

![image-20220704112608995](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704112608995.png)