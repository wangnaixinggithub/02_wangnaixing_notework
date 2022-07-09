# Install-Redis-Software

# 1、Download And Installation

```shell
docker pull redis:5.0	
```

# 2、Start

```shell
docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-d redis:5.0 redis-server --appendonly yes	
```

