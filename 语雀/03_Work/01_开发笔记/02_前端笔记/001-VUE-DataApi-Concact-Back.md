# VUE-DataApi-Concact-Back

# 1、dataApi.js

```javascript
import $http from "@/common/utils/http";
import formData from "@/common/api/data/formData.js";


const getUserInfo = ()=> $http.get('user/info');
const ddzxSave = (formData) => $http.post("azsbjxlc/ddzx", formData);
const validateData = (atvdId, formData) => {return $http.post(`azsbjxlc/validate/${atvdId}`, formData);}
const initDefaultData = (recordId, atvdId) => $http.post(`azsbjxlc/default-value/${recordId}/${atvdId}`);
const sendBackToPms = (recordId, ywId, msg) => {
    let param = {
        "formId": recordId,
        "ywId": ywId,
        "msg": msg
    }
    return $http.post(`azsbjxlc/sendBackToPms`, param);
}
const getFormObjData = (prcdModelId, recordId) => formData.getFormData(prcdModelId, {queryParam: {"PRIMARY_KEY": recordId} });
const save = (prcdModelId, param) => formData.save(prcdModelId, param);


const dataApi = {
    getUserInfo,
    getFormObjData,
    save,
    ddzxSave,
    validateData,
    initDefaultData,
    sendBackToPms，
}
export default dataApi;
```

> 在你的VUE组件中使用~ ~ ~ ~ ~

```java
<script>
   //导入dataApi.js
import dataApi from "../api/dataApi.js";
	 
export default {
    name: "add",
      data(){
          return {
          	  user: {}
          },
     
       methods:{ 
      	async init(){
            let self = this;
            
            //使用dataApi实例调用getUserInfo()的方法
            this.user = await dataApi.getUserInfo();
        }
      },
          
     beforeMount() {
       	  this.init();
      }
</script>
```



# 2、http.js

```javascript
import axios from 'axios';

axios.defaults.withCredentials=true;
axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';


let requestInterceptor = axios.interceptors.request.use(
 
    config => {
       
        config.headers = {
            'Content-Type': 'application/json'
        };
        
        let url = config.url;
       
        //1.GET Method handle
        if (config.method === 'get' && config.params) {
            url += '?'
            let keys = Object.keys(config.params)
            for (let key of keys) {
                if(typeof(config.params[key]) === 'object'){
                    url += `${key}=${encodeURIComponent(JSON.stringify(config.params[key]))}&`
                }else{
                    url += `${key}=${encodeURIComponent(typeof(config.params[key]) == "undefined"?"":config.params[key])}&`
                }
            }
            url = url.substring(0, url.length - 1)
            config.params = {}
        }
    
        config.url = url;

        return config;
    },
    err => {
      
        return Promise.reject(err);
    }
);


let responseInterceptor = axios.interceptors.response.use(response => {
 	
   return response
}, error => {

  let {status, data} = (error && error.response) || {}
  switch (status) {
         
    case 401: //401无授权，redirect => Login.html
      let err = data && data.error
      if (process.env.NODE_ENV === 'development' && err === 'unauthorized' && !window.location.href.includes('platform.html#/login')) {
        window.location.href = './platform.html#/login?service=' + btoa(encodeURIComponent(window.location.href))
      }
      break
          
    default:
      return Promise.reject(error)
  }
})


let $http = {
   
    urlPri : process.env.API_ROOT,
    model: process.env.NODE_ENV,
    accounTreeUrl: process.env.ACCOUN_TREE,
    requestInterceptor: requestInterceptor,
    responseInterceptor: responseInterceptor,
  
    //PUT请求
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
   
    //POST请求
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
   
    //POST请求，URL完全自定义
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
   
    //GET请求
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
   
    //GET请求，完全自定义
    getUrl(url, params = {}, responseType) {
        return new Promise((resolve, reject) => {
            axios.get(url, { params, responseType }).then(response => {
                resolve(response.data)
            }, err => {
                reject(err)
            })
        })
    },
   
    //DELETE请求
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
};

export default $http;

```

# 3、fromData.js

```javascript
import http from "../request"

/**
 *  表单统一数据接口API
 */
export default {
    /*** 获取表单数据 **/
    getFormData(formModelId, params) {
        return http.get("form-data/" + formModelId, params);
    },
    /*** 保存/更新表单数据 **/
    save(formModelId, params) {
        return http.post("form-data/" + formModelId, params);
    },
    /*** 根据表单模型ID获取配置项 **/
    getConfig(formModelId, params) {
        return http.post("form-data/" + formModelId + "/config/", params);
    },
}

```

