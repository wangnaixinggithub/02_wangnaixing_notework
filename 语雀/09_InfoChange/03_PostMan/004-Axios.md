```javascript
 put: function (url, data = {}) {
        return new Promise((resolve, reject) => {
            axios.put($http.urlPri + url, JSON.stringify(data))
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                });
        });
    },
    post: function (url, data = {}) {
        return new Promise((resolve, reject) => {
            axios.post($http.urlPri + url, JSON.stringify(data))
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                });
        });
    },
    postUrl: function (url, data = {}) {
        return new Promise((resolve, reject) => {
            axios.post(url, JSON.stringify(data))
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                });
        });
    },
    get: function (url, data = {}) {
        return new Promise((resolve, reject) => {
            axios.get($http.urlPri + url, { params: data })
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                });
        });
    },
    getUrl(url, params = {}, responseType) {
        return new Promise((resolve, reject) => {
            axios.get(url, { params, responseType }).then(response => {
                resolve(response.data)
            }, err => {
                reject(err)
            })
        })
    },
    delete: function (url, data = {}) {
        return new Promise((resolve, reject) => {
            axios.delete($http.urlPri + url, { params: data })
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                })
        });
    }
```

