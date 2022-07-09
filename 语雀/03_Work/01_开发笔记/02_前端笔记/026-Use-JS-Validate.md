# Use-JS-Validate

# validate.js

```javascript
/**
 * 表单验证
 */
import String from "@/common/utils/string.utils.js";

// 手机校验规则
let phoneReg = /^1[3|4|5|7|8|9][0-9]\d{8}$/

// 座机、传真校验规则
let telOrFaxReg = /^(\d{3,4}-)?\d{7,8}$/

// 邮箱校验规则
let emailReg = /^[a-z0-9]+([._\\-]*[a-z0-9])*@([a-z0-9]+[-a-z0-9]*[a-z0-9]+.){1,63}[a-z0-9]+$/

// 纯数字校验
let numberReg = /^\d+$|^\d+[.]?\d+$/


let validate = {
    _blank(value) {
        let temp = value.trim();
        if(temp.length != value.length) {
            return true;
        }

        return false;
    },
    
    /**
     * 手机号验证
     * @param rule
     * @param value
     * @param callback
     */
    mobileNumber(rule, value, callback) {
        if(validate._blank(value)) {
            callback(new Error("输入内容有空格！"));
            return;
        }

        const isPhone = phoneReg.test(value)
        value = Number(value) //转换为数字
        if (typeof value === "number" && !isNaN(value)) {//判断是否为数字
            value = value.toString() //转换成字符串
            if (value.length < 0 || value.length > 12 || !isPhone) { //判断是否为11位手机号
                callback(new Error("手机号码位数不对，请仔细核对"))
            } else {
                callback()
            }
        } else {
            callback(new Error("请输入正确的手机号码格式 如:138xxxx5678"))
        }
    },
    
    /**
     * 身份证验证
     * @param rule
     * @param value
     * @param callback
     */
    idCard(rule, idCard, callback) {
        if(validate._blank(idCard)) {
            callback(new Error("输入内容有空格！"));
            return;
        }

        var regIdCard = /^(^[1-9]\d{7}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])\d{3}$)|(^[1-9]\d{5}[1-9]\d{3}((0\d)|(1[0-2]))(([0|1|2]\d)|3[0-1])((\d{4})|\d{3}[Xx])$)$/;

        if (regIdCard.test(idCard)) {
            if (idCard.length == 18) {
                var idCardWi = new Array(7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2);
                var idCardY = new Array(1, 0, 10, 9, 8, 7, 6, 5, 4, 3, 2);
                var idCardWiSum = 0;
                for (var i = 0; i < 17; i++) {
                    idCardWiSum += idCard.substring(i, i + 1) * idCardWi[i];
                }

                var idCardMod = idCardWiSum % 11;
                var idCardLast = idCard.substring(17);

                if (idCardMod == 2) {
                    if (idCardLast == "X" || idCardLast == "x") {
                        callback();
                    } else {
                        callback(new Error("身份证号码错误！"));
                    }
                } else {
                    if (idCardLast == idCardY[idCardMod]) {
                        callback();
                    } else {
                        callback(new Error("身份证号码错误！"));
                    }
                }
            }
        } else {
            callback(new Error("身份证格式不正确!"));
        }
    }
};

export default {

    /**
     * 允许电话内容为空
     * @param rule
     * @param value
     * @param callback
     */
    mobileNumberAllowBlank(rule, value, callback) {
        if(!value) {
            callback();
            return;
        }

        validate.mobileNumber.call(this, rule, value, callback);
    },
    
    /**
     * 不允许内容为空，有空格
     * @param rule
     * @param value
     * @param callback
     */
    mobileNumber: function (rule, value, callback) {

        if(!value) {
            callback(new Error("电话号码不能为空！"));
            return;
        }

        validate.mobileNumber.call(this, rule, value, callback);
    },
    
    /**
     * 身份验证允许为空
     * @param rule
     * @param value
     * @param callback
     */
    idCardAllowBlank(rule, value, callback) {
        if(!value) {
            callback();
            return;
        }
        validate.idCard.call(this, rule, value, callback);
    },
    
    /**
     * 身份验证不能为空
     * @param rule
     * @param value
     * @param callback
     */
    idCard: function (rule, value, callback) {
        if (!value) {
            callback(new Error("身份证不能为空！"))
            return;
        }
        validate.idCard.call(this, rule, value, callback);
    }
};




```

