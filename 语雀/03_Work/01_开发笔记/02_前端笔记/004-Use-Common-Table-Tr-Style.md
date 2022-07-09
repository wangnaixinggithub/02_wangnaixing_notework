# Use-Common-Table>Tr-Style

# 1、common.less

```less
@el-table-border-color:#EBEEF5;
@el-table-background-color:#FFF;

@el-table-header-background-color:#FFF;
@el-table-header-text-color:#516b8c;
@el-table-header-text-align:center;
@el-table-header-text-weight:bold;

@border-color:rgb(222,222,222);
@titleColumn-bg-color:rgb(244,245,247);
@valueColumn-bg-color:rgb(255,255,255);
@titleColumn-font-color:black;
@valueColumn-font-color:black;
@border-split-size:1px;
@font-size:14px;

/******Element-Table Start**********/
/deep/ .el-table td, .el-table th.is-leaf{
    border-bottom:1px solid @el-table-border-color;
}

/deep/ .el-table--border td, .el-table--border th, .el-table__body-wrapper .el-table--border.is-scrolling-left~.el-table__fixed{
    border-right:1px solid @el-table-border-color;
}

/deep/ .el-table--border th{
    border-right: 1px solid @el-table-header-background-color;
}

/deep/ .el-table th, .el-table tr{
    background-color:@el-table-background-color;
}

/deep/ .el-table th{
    background-color:@el-table-header-background-color;
    text-align: @el-table-header-text-align;
}

/deep/ .el-table thead{
    color:@el-table-header-text-color;
    font-weight: @el-table-header-text-weight;
}

/deep/ .el-table .cell, .el-table th div, .el-table--border td:first-child .cell, .el-table--border th:first-child .cell{
    text-align: center;
}

/deep/ .el-input__inner {
    background-color: @valueColumn-bg-color;
}

/deep/ .el-input.is-disabled .el-input__inner{
    color:black;
}

/deep/ .el-textarea.is-disabled .el-textarea__inner{
    color:black;
}

/deep/ .el-checkbox__input.is-disabled+span.el-checkbox__label{
    color:black;
}

/******Element-Table End**********/
/******Element Input******/
/deep/ .el-input__prefix, .el-input__suffix{
    top:7px;
}

/deep/ .el-input__prefix .el-icon-time{
    position: absolute;
    top: -13px;
}

/deep/ .el-input__inner{
    height: 30px;
    line-height: 30px;
}
/*****Element-collaspe******/
/deep/ .el-collapse-item__header{
    background: rgb(233,236,241);
}
/******Common Css Start*******************/
.column{
    vertical-align: middle;
    font-size: @font-size;
    font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
    padding: 10px;
}

.title_column{
    background-color: @titleColumn-bg-color;
    text-align: center;
    color:@titleColumn-font-color ;
    border: 1px solid @border-color;
    font-weight: 700;
}

.value_column{
    background-color: @valueColumn-bg-color;
    color: @valueColumn-font-color;
}

.td_title_column{
    background-color: @titleColumn-bg-color;
    vertical-align: middle;
    text-align: right;
    font-size: @font-size;
    color: @titleColumn-font-color;
    border: 1px solid @border-color;
    font-weight: 700;
    padding-right: 12px;
}

.td_title_column_align_center{
    background-color: @titleColumn-bg-color;
    vertical-align: middle;
    text-align: center;
    font-size: @font-size;
    color: @titleColumn-font-color;
    border: 1px solid @border-color;
    font-weight: 700;
}

.td_value_column{
    vertical-align: middle;
    background-color: @valueColumn-bg-color;
    font-size: @font-size;
    color: @valueColumn-font-color;
    padding: 10px;
}

.column_border{
    border: 1px solid @border-color;
}

.column_left{
    border-left: 1px solid @border-color;
}

.column_top{
    border-top: 1px solid @border-color;
}

.column_top_right{
    border-top: 1px solid @border-color;
    border-right:1px solid @border-color;
}

.column_top_bottom{
    border-top: 1px solid @border-color;
    border-bottom: 1px solid @border-color;
}

.column_top_bottom_right{
    border-top: 1px solid @border-color;
    border-bottom: 1px solid @border-color;
    border-right: 1px solid @border-color;
}

.column_bottom{
    border-bottom: 1px solid @border-color;
}

.column_bottom_right{
    border-bottom: 1px solid @border-color;
    border-right: 1px solid @border-color;
}

.column_bottom_right_left{
    border-bottom: 1px solid @border-color;
    border-right: 1px solid @border-color;
    border-left: 1px solid @border-color;
}

.column_right{
    border-right: 1px solid @border-color;
}

/*****************button*******************/
/deep/ .el-button--primary {
    background: #2364A6;
    border-color: #2364A6;
}

/deep/ .el-button--primary:focus, /deep/.el-button--primary:hover  {
    background: #FFFFFF;
    color: black;
    border-color: #2364f6;
}

/deep/ .el-pagination.is-background .el-pager li:not(.disabled).active {
    background: #2364A6;
    border-color: #2364A6;
}
/******Common Css End*******************/

```

# 2、import

```css
<style scoped lang="less">
@import "../../../common/css/common.less";

.el-table td, .el-table th {
    padding: 0;
    min-width: 0;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    text-overflow: ellipsis;
    vertical-align: middle;
    position: relative;
    text-align: left;
}
.el-date-editor.el-input, .el-date-editor.el-input__inner {
    width: 100%;
}
.el-select{
    width: 100%;
}
</style>
```

# 3、Use In Your VUE

```vue
<template>
<div>
    <div class="container-fluid" style="padding-left: 0;padding-right: 0;">
        <table style="width: 100%; border-collapse:collapse;">
            <colgroup>
                <col width="12.6%"></col>
                <col width="20.6%"></col>
                <col width="12.6%"></col>
                <col width="20.6%"></col>
                <col width="12.6%"></col>
                <col width="20.6%"></col>
            </colgroup>
            <tbody>
            <tr>
                <td class="td_title_column">申请单编号<span style="color: red;"> *</span></td>
                
                <td class="td_value_column column_top_bottom">
                    <el-input id="SQDBH" 
                              v-model="primaryData.SQDBH"
                              :show-word-limit="true" :maxlength="30">
                    </el-input>
                </td>
                
                <td class="td_title_column">申请单位<span style="color: red;"> *</span></td>
                <td class="td_value_column column_top_bottom_right">
                    <el-input id="SQDW"  
                              v-model="primaryData.SQDW" 
                              :show-word-limit="true" 
                              :maxlength="30"
                              placeholder="双击点选">
                    </el-input>
                </td>
                
                <td class="td_title_column">提交时间<span style="color: red;"> *</span></td>
                <td class="td_value_column column_top_bottom_right">
                    <el-date-picker
                        id="TJSJ"
                        v-model="primaryData.TJSJ"
                        type="datetime"
                        format="yyyy-MM-dd HH:mm:ss"
                        value-format="yyyy-MM-dd HH:mm:ss"
                        placeholder="选择日期时间">
                    </el-date-picker>
                </td>
                
            </tr>
            </tbody>
        </table>
    </div>
</div>
</template>
```

