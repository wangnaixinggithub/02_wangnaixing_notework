# ElementUI-[dblClickTextArea]

# 1、dblClickTextArea

```vue
<!-- 双击填写 -->
<template>
    <div>
        <div @dblclick="elDblClick">
            <el-tooltip class="item" effect="light" :disabled="disable" content="双击填写" placement="top">
                <el-input
                    v-model="model"
                    :type="type"
                    :readonly="true"
                    :disabled="disable"
                    :autosize="{ minRows: rowsize, maxRows: 10 }"
                    :show-word-limit="true"
                    :maxlength="maxlength" style="padding:1px 0"></el-input>
            </el-tooltip>
        </div>

        <!-- :close-on-click-modal="false"  是否可以通过点击 外部 关闭 Dialog -->
        <!-- :close-on-press-escape="false"  是否可以通过按下 ESC 关闭 Dialog -->
        <el-dialog v-if="showDialog" :title="dialogTitle" :visible.sync="dialogVisible"
                   :close-on-click-modal="false"
                   width="50%" @close="changeView">
			<div>
                <el-input type="textarea" ref="input"
                    v-model="inputValue"
                    :disabled="disable"
                    :autosize="{ minRows: 5, maxRows: 20 }"
                    :show-word-limit="true" :maxlength="maxlength-datalength"></el-input>
            </div>

            <span slot="footer" class="dialog-footer">
                <el-button type="primary" size="small" @click="btnSure_onclick">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
    import dateUtils from "../../api/dateUtils";

    export default {
        name: "dblClickTextArea",
        props:{
            maxlength:{
                type: Number,
                default: 2000
            },
            value: {
                type: String,
                default: ""
            },
            type: {//显示的类型：input 或 textarea;  默认textarea
                type: String,
                default: "textarea"
            },
            inputType: {
                type: String,
                default: "insert"//insert：普通输入; append: 默认 inputValue 为空，往文本域追加内容
            },
            disable: {
                type: Boolean,
                default: false,
            },
            appendSign: {//是否追加签名
                type: Boolean,
                default: false,
            },
            signText: {//appendSign=true时，
                type: String,
                default: ""
            },
            dialogTitle:{//弹出框标题
                type: String,
                default: "输入内容"
            },
            userName:{
                type: String,
                default: ""
            },
            rowsize:{
                type: Number,
                default: 2
            },
        },
        data(){
            return {
				inputValue: "",
                dialogVisible:true,
                showDialog: false,
            }
        },
        computed:{
            model:{
                get(){
                    return this.value;
                },
                set(val){
                    this.$emit("input", val);
                }
            },
            datalength(){
                return this.model ? this.model.length : 0;
            }
        },
        methods:{
            elDblClick(){
                if(this.appendSign && this.disable) return;

                if(this.inputType=="insert"){
                    this.inputValue = this.model;
                }
                if(this.inputType=="append"){
                    this.inputValue = "";
                }
                this.showDialog = true;
                this.dialogVisible = true;
            },
            changeView() {
                this.showDialog = false;
            },
            btnSure_onclick(){
                if(this.inputType=="insert"){
                    this.model = this.inputValue + this.getSignText();
                }
                if(this.inputType=="append"){
                    this.model = this.model ? this.model + "\n" + this.inputValue + this.getSignText() : this.inputValue + this.getSignText();
                }
                if(!this.disable){//可编辑状态才触发保存方法
                    this.$eventBus.$emit("save", false);
                }
                this.changeView();
            },
            getSignText(){
                if(this.appendSign){
                    let text =" —"+this.userName+"_"+dateUtils.newDate();
                    //是否 添加签名
                    let signText = this.$cache.getSignText();
                    if(signText){
                        return signText;
                    }else{
                        return text;
                    }
                }
                return "";
            },
        },
        watch:{
            defaultValue(){
                if(this.defaultValue){
                    this.inputValue = this.defaultValue;
                }
            }
        }
    }
</script>

<style scoped lang="less">
    @import "../../../../common/css/common.less";

	.el-input, .el-date-editor.el-input__inner {
	    width: 50%;
	}
</style>

```

# 2、In Your Business

- 导入

```vue
<template>

</template>

<script>

import dblClickTextArea from "../plugins/DblClickTextArea.vue";
export default {
    name: "wazx",
    components: {
         dblClickTextArea
    },
}
</script>

<style scoped>

</style>

```

- Use

```vue
  <!--06-调度备注-->
                <tr>
                    <td class="td_title_column_align_center">调度备注<span style="color: red">（提示：双击填写信息）</span></td>
                    <td class="td_value_column column_top_bottom_right" colspan="6">
                        <dbl-click-text-area :disable="authority.DDBZ"
                                             :dialogTitle="'调度备注'"
                                             v-model="primaryData.DDBZ"
                                             inputType="append"
                                             :append-sign="true"
                                             :user-name="userInfo.userName"
                                             placeholder="双击填写信息">
                        </dbl-click-text-area>
                    </td>
                </tr>
```

