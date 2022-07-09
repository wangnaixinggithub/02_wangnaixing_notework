# JS-String-Handle-Utils

# string.utils.js

```javascript
String.prototype.getByteLen = function() {
    var len = 0;
    for (var i=0; i<this.length; i++)
    {
        if ((this.charCodeAt(i) & 0xff00) != 0)
            len ++;
        len ++;
    }
    return len;
};

/**
 * 删除左右两端的空格
 * @returns {string}
 */
String.prototype.trim=function() {
    return this.replace(/(^\s*)|(\s*$)/g,'');
};

/**
 * 删除左边的空格
 */
String.prototype.ltrim=function() {
    return this.replace(/(^\s*)/g,'');
};

/**
 * 删除右边的空格
 */
String.prototype.rtrim=function() {
    return this.replace(/(\s*$)/g,'');
};

```

