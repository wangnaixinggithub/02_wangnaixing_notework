# JS-BOM-navigator-userAgent

- 通过判断`userAgent` 信息来确定，是显示手机端的页面，还是显示网页端的页面。

```html
   <script>
        if ((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
            window.location.href = "../H5/index.html"; //手机
        }
    </script>
```

