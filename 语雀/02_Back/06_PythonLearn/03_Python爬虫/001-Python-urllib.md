# Python-urllib

- urllib 是Python 自带的爬虫。可以爬取网站信息/

# 1、urlopen()

- 获取此请求的响应对象。

```python
from urllib.request import *


def main():
    response = urlopen("http://httpbin.org/get")  # getResponseObject

    print(type(response))  # <class 'http.client.HTTPResponse'>

    print(response.getcode())  # 200


if __name__ == '__main__':
    main()

```

# 2、getcode()

- 获取响应状态码

```java
from urllib.request import *


def main():
      response = urlopen("http://httpbin.org/get")
    print(response.getcode()) # 200


if __name__ == '__main__':
    main()

```

# 3、getheaders()

- 获取全部响应头,得到封装请求头的元组列表。

```python
from urllib.request import *


def main():
    response = urlopen("http://httpbin.org/get")
    headers = response.getheaders()
    for header in headers:
        print(header)


if __name__ == '__main__':
    main()

```

```python
('Content-Length', '274')
('Access-Control-Allow-Credentials', 'true')
('Access-Control-Allow-Origin', '*')
('Connection', 'keep-alive')
('Content-Type', 'application/json')
('Date', 'Sun, 29 May 2022 09:23:39 GMT')
('Keep-Alive', 'timeout=4')
('Proxy-Connection', 'keep-alive')
('Server', 'gunicorn/19.9.0')
```

- 获取指定的响应头

```python
from urllib.request import *


def main():
    response = urlopen('https://www.runoob.com')
    print(response.getheader('Content-Type')) # 获取指定响应头
    print(response.getheader('Date'))


if __name__ == '__main__':
    main()

```

# 4、read()

- 获取响应体的内容

```java
from urllib.request import *


def main():
    response = urlopen('https://www.runoob.com')
    responseBody = response.read()
    
    print(responseBody)


if __name__ == '__main__':
    main()

```

```python
from urllib.request import *


def main():
    response = urlopen('https://www.runoob.com')
    responseBody = response.read(300) # 获取部分内容

    print(responseBody)


if __name__ == '__main__':
    main()

```

```python
from urllib.request import *


def main():
    response = urlopen('https://www.runoob.com')
    responseBody = response.readline() # 只获取响应体第一行

    print(responseBody)


if __name__ == '__main__':
    main()

```

```python
from urllib.request import *


def main():
    response = urlopen('https://www.runoob.com')

    responseBody = response.readlines()

    for line in responseBody: # 逐行读取ResposeBody
        print(line)


if __name__ == '__main__':
    main()

```

```shell
from urllib.request import *


def main():
    response = urlopen('https://www.runoob.com')

    f = open('test.html', 'wb')  # 把内容写入test.html
    f.write(response.read())
    f.close()


if __name__ == '__main__':
    main()

```

# 5、POST Request

- 发起一次Post的请求

```python
from urllib.parse import urlencode
from urllib.request import urlopen


def main():
    # 1.构建查询字符串 dict => string
    query_string = urlencode({"id": 1, "name": '王乃醒', 'age': 26})
    
    # 2.string => bytes
    data = bytes(query_string, encoding='UTF-8')

    # 3. 发POST请求
    response = urlopen(url='http://httpbin.org/post', data=data)

    # 4.读取响应信息
    response_body = response.read()

    print(response_body.decode('UTF-8'))


if __name__ == '__main__':
    main()

```

```json
{
    
  "args": {}, 
    
  "data": "", 
    
  "files": {}, 
    
  "form": {
    "age": "26", 
    "id": "1", 
    "name": "\u738b\u4e43\u9192"
  }, 
    
  "headers": {
    "Accept-Encoding": "identity", 
    "Content-Length": "44", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "Python-urllib/3.9", 
    "X-Amzn-Trace-Id": "Root=1-6293334b-1d280cab6d11e091255d41fe"
  }, 
    
  "json": null, 
    
  "origin": "61.144.103.63", 
    
  "url": "http://httpbin.org/post"
}
```

- 或者采用创建一个Request对象，这样的请求可以实现高度定制。

```python
import urllib
from socket import socket
from urllib.parse import urlencode
from urllib.request import urlopen


def main():
    # 1.构建Request Param
    url = 'http://httpbin.org/post'
    host = 'httpbin.org'
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3314.0 Safari/537.36 SE 2.X MetaSr 1.0",
        "Host": "httpbin.org"
    }
    param = bytes(urllib.parse.urlencode({
        "id": 1,
        "name": '王乃醒'
    }), 'UTF-8')
    method = 'POST'

    # 2. 创建Request
    request = urllib.request.Request(url=url, data=param, headers=headers, origin_req_host=host, method=method)

    # 3.发起请求

    response = urlopen(request)

    resp_body = response.read().decode('UTF-8')
    print(resp_body)


if __name__ == '__main__':
    main()

```

```python
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "id": "1", 
    "name": "\u738b\u4e43\u9192"
  }, 
  "headers": {
    "Accept-Encoding": "identity", 
    "Content-Length": "37", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3314.0 Safari/537.36 SE 2.X MetaSr 1.0", 
    "X-Amzn-Trace-Id": "Root=1-62933ad1-501f92c6539cf037233683ca"
  }, 
  "json": null, 
  "origin": "202.105.66.173", 
  "url": "http://httpbin.org/post"
}
```

