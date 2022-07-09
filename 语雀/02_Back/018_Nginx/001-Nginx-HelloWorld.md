# Nginx-HelloWorld

- 是高性能的HTTP服务器

- 是反向代理服务器 作负载均衡

# 1、正向代理

比如：VPN,代理客户端的请求，由他去请求服务器，把响应结果再给客户端。

![image-20220703165719376](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703165719376.png)

# 2、反向代理

比如很多大型网站内部：接受客户端的请求，有Nginx决定交给哪个服务器处理。

![image-20220703165818747](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703165818747.png)

# 3、动静分离

静态文件直接访问Nginx就可以了，如果是动态资源再请求Web服务。

![image-20220703170756737](001-Nginx-HelloWorld.assets/image-20220703170756737.png)