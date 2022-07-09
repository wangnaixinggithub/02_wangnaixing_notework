# Python-Requests

# 1、Download

```shell
pip install redis  -i http://pypi.doubanio.com/simple/ --trusted-host pypi.doubanio.com
```

# 2、get()

- 发起一次get请求。类似的API还有`post()` `get()` `delete()` `put()`

```python
import requests


def main():
    
    response = requests.get('http://httpbin.org/get')
    
    print(type(response))  # <class 'requests.models.Response'>


if __name__ == '__main__':
    main()

```

