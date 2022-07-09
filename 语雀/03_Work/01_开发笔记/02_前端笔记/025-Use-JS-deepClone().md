# Use-JS-deepClone()

> JS对象或者数组的深度克隆

```javascript
const jxdUtils = {
    /**
     * 深度克隆
     * @param 克隆对象
     * @returns {*}
     */
    deepClone: function(obj){
        let objClone = Array.isArray(obj) ? [] : {};

        if(obj && typeof obj==="object"){
            for(let key in obj){
                if(obj.hasOwnProperty(key)){
                    obj[key] && typeof obj[key] ==="object" ?
                        objClone[key] = this.deepClone(obj[key]) :  objClone[key] = obj[key];
                }
            }
        }

        return objClone;
    }
};

export default jxdUtils;

```

