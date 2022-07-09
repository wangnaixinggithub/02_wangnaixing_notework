# 001-Liunx-ps -ef  grep nginx

使用ps命令 配合grep 管道 可以筛选出来我们需要查询的进程

```shell
# 1、查询nginx进程运行情况
ps -ef | grep nginx

# 2、可以知道 Nginx的主进程 的PID 为 61219

```

![image-20220704200754828](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704200754828.png)