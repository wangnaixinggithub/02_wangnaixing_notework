# Use-JS-Handle-Array

# array.tools.js

```javascript
"use strict";

/**
 * 清空数据
 */
Array.prototype.clean = function() {
    this.splice(0,this.length);
};

/**
 * 根据元素的ID查找对象
 * @param ele
 * @returns {number}
 */
// Array.prototype.findIndex = function(ele) {
//     for(let index=0; index< this.length; index++) {
//         if(this[index].id == ele.id) {
//             return index;
//         }
//     }
//
//     return -1;
// };

Array.prototype.delete=function(delIndex){
    var temArray=[];
    for(var i=0;i<this.length;i++){
        if(i!=delIndex){
            temArray.push(this[i]);
        }
    }
    return temArray;
};
export default Array;

```

