# Use-JS-Date-Calc

# DateUtils

```javascript
const DateUtils = {

    /**
     * 计算时间长度（天）
     * @param startDate 开始时间
     * @param endDate 结束时间
     * @returns
     */
    calculateDate: function (startDate, endDate) {
        if (typeof startDate === "string") {
            startDate = Date.parse(startDate);
        }
        if (typeof endDate === "string") {
            endDate = Date.parse(endDate);
        }

        if (!this.checkDate(startDate, endDate)) {
            return "";
        }
        let hm = endDate - startDate;
        if (Number.isNaN(hm)) {
            hm = (new Date(endDate) - new Date(startDate));
        }

        let hours = Math.ceil(hm / 3600 / 1000);
        let days = Math.floor(hours / 24);//天数
        hours = hours % 24;
        let result = `${days}天${hours}小时`;
        if (days < 1) {
            result = `${hours}小时`;
        }
        return result;
    },

    /**
     *  判断时间大小是否合理
     * @param startDate 开始时间
     * @param endDate 结束时间
     * @returns  结束时间 >= 开始时间
     */
    checkDate: function (startDate, endDate) {
        if (typeof startDate === "string") {
            startDate = Date.parse(startDate);
        }
        if (typeof endDate === "string") {
            endDate = Date.parse(endDate);
        }
        if (startDate && endDate) {
            return endDate >= startDate;
        }
        return false;
    },

    /**
     * 格式化后的 当前时间
     */
    newDate: function () {
        return this.formatDate(new Date());
    },


    getUuid: function () {
        var s = [];
        var hexDigits = "0123456789abcdef";
        for (var i = 0; i < 36; i++) {
            s[i] = hexDigits.substr(Math.floor(Math.random() * 0x10), 1);
        }
        s[14] = "4";
        s[19] = hexDigits.substr((s[19] & 0x3) | 0x8, 1);
        s[8] = s[13] = s[18] = s[23] = "-";
        var uuid = s.join("");
        return uuid;
    },

    /**
     * 格式化日期
     * @param date
     * @param pattern, 默认"yyyy-MM-dd hh:mm:ss"
     * return string
     */
    formatDate: function (date, pattern) {
        if (date == null || date == "") {
            return;
        }
        if (typeof date === "string") {
            date = new Date(date);
        }
        if (!pattern)
            pattern = "yyyy-MM-dd hh:mm:ss";

        let result = "";
        let o = {
            "M+": date.getMonth() + 1,                 //月份
            "d+": date.getDate(),                    //日
            "h+": date.getHours(),                   //小时
            "m+": date.getMinutes(),                 //分
            "s+": date.getSeconds(),                 //秒
            "q+": Math.floor((date.getMonth() + 3) / 3), //季度
            "S": date.getMilliseconds()             //毫秒
        };
        if (/(y+)/.test(pattern)) {
            result = pattern.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
        }
        for (let k in o) {
            if (new RegExp("(" + k + ")").test(result)) {
                result = result.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
            }
        }
        return result;
    },

    /**
     * 增加分钟数
     * @param date
     * @param minutes
     */
    addMinutes: function (date, minutes) {
        if (date && minutes) {
            date = new Date(date);//转为 时间对象
            date = this.formatDate(new Date(date.valueOf() + minutes * 60 * 1000)); // 添加 分钟数,然后格式化
        }
        return date;
    },

    /**
     * 减少分钟数
     * @param date
     * @param minutes
     */
    reduceMinutes: function (date, minutes) {
        if (date && minutes) {
            date = new Date(date);//转为 时间对象
            date = this.formatDate(new Date(date.valueOf() - minutes * 60 * 1000)); // 添加 分钟数,然后格式化
        }
        return date;
    }
}


export default DateUtils;

```

