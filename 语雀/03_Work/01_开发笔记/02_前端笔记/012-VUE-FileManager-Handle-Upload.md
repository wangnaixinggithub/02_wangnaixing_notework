# VUE-FileManager-Handle-Upload

# 1、Import

```javascript
<script>
import fileManager from "@/common/plugins/fileManager/FileManager.vue";
export default {
      components: {fileManager},
}
</script>
```

```vue
<tr>
     <td class="td_title_column">附件</td>
     <td class="td_value_column column_top_bottom_right" colspan="5">
       <file-manager id="FJ"
             :multiple=true
             :action="'file/upload'"
             :accept="'*'"
             :fileSize=50 
             :style="'width:100%;'"
             :recordId="primaryData.ID"
             :column="'FJ'"
             :disabled="showUpload">
      </file-manager>
    </td>
</tr>
```

```properties
- multiple 支持上传多文件吗？
- action 上传地址
- accept 接收上传的文件类型，*表示任意。
- fileSize 文件大小
- style 修改样式
- recordId 记录ID
- column 记录字段名称
- disabled 是否禁用他
```

可以使用计算属性，计算文件上传组件什么时候可用。

```javascript
  export default {		
		computed:{
    		showUpload(){
                    let self =this;
                    if(self.urlParams.atvdID.includes("TCSQ") && self.urlParams.status=="A"){
                        return false;
                    }else{
                        return true;
                    }
                },
        }
}              
```

# 2、FileManager.vue

```vue
<template>
    <div>
        <div :style="styleSheet">
            <el-input 
                      readonly 
                      v-model="inputText" 
                      v-if="uploadBtnOnly==false" 
                      size="mini" 
                      style="float: left;width: 80%;">
            </el-input>
            <el-button type="primary" 
                       @click="outerVisible = true" 
                       size="mini" 
                       style="float:left;">
                {{btnText}}
             </el-button>
            <div style="clear: both;"></div>
        </div>
       
        <el-dialog 
              title="文件管理器" 
              :visible.sync="outerVisible" 
              width="30%" 
              :close-on-click-modal="closeOnClickModal" 
              :before-close="closeDialog" 
              :append-to-body='true'>
            
           <el-dialog 
                  width="20%" 
                  title="文件上传" 
                  :visible.sync="innerVisible"  
                  append-to-body 
                  :close-on-click-modal="closeOnClickModal" 
                  top="23vh">
                
                <uploader ref="uploader" 
                          :multiple="true" 
                          :action="action" 
                          :accept="accept" 
                          :fileSize="fileSize" 
                          :recordId="recordId" 
                          :column="column" 
                          @uploadHandler="uploadHandler" 
                          :fileVerify="fileVerify" 
                          :fileVerifyHandler="fileVerifyHandler">
                </uploader>
            </el-dialog>
              
            <div style="width:100%;height:300px;">
                <div style="width:100%;height:20px;text-align:left;">
                    <el-button type="primary" class="el-icon-upload2" size="small" :disabled="disabled" @click="innerVisible=true">上传</el-button>
                    <el-button type="primary" class="el-icon-download" size="small" @click="downloadFile">下载</el-button>
                    <el-divider direction="vertical"></el-divider>
                    <el-link icon="el-icon-delete" @click="deleteFile" :disabled="disabled">删除</el-link>
                </div>
                
                <div style="width:100%;height:280px;">
                    <el-table @selection-change="selectedChange" ref="multipleTable" :data="fileList" tooltip-effect="dark" style="width: 100%;margin-top:22px;" height="260px" empty-text="文件列表为空">
                        <el-table-column type="selection" width="40">
                        </el-table-column>
                        <el-table-column label="文件名" prop="fileName">
                        </el-table-column>
                    </el-table>
                </div>
                
            </div>
              
            <span slot="footer" class="dialog-footer">
                
            <el-button @click="close">关 闭</el-button>
        </span>
              
        </el-dialog>
    </div>
</template>
<script>
import Uploader from '@/common/plugins/upload/Uploader.vue'
import nconsole from '@/common/utils/console.js'

export default {
  name: 'FileManager',
  components: {
    Uploader
  },
  props: {
    styleSheet: {
      type: String,
      default: 'width: 100%'
    },
    btnText: {
      type: String,
      default: '...'
    },
    multiple: {
      type: Boolean,
      default: false
    },
    disabled: {
      type: Boolean,
      default: true
    },
    action: {
      type: String,
      default: 'file/upload'
    },
    accept: {
      type: String,
      default: '*'
    },
    fileSize: {
      type: Number,
      default: 2
    },
    column: {
      type: String,
      default: ''
    },
    recordId: {
      type: String,
      default: ''
    },
    uploadBtnOnly: {
      type: Boolean,
      default: false
    },
    failureTips: {
      type: Boolean,
      default: true
    },
    fileVerify: {
      type: Boolean,
      default: false
    },
    fileVerifyHandler: {
      type: Function
    },
    classId: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      inputText: '',
      outerVisible: false,
      innerVisible: false,
      closeOnClickModal: false,
      fileListUrl: '',
      fileList: [],
      selectedIds: '',
      multipleSelection: []
    }
  },
  mounted () {
    if (this.recordId) {
      this.getFileList()
    }
  },
  watch: {
    recordId (newVal, oldVal) {
      newVal && this.getFileList()
    },
    outerVisible (newVal, oldVal) {
      newVal && this.getFileList()
    }
  },
  methods: {
    getFileList () {
      this.$http.get('file/list', {
        recordId: this.recordId,
        column: this.column
      }).then(response => {
        this.fileList = response
        this.loadInputText()
      }).catch(err => {
        nconsole.log(err)
      })
    },
    selectedChange (selection) {
      this.selectedIds = ''
      for (let i = 0; i < selection.length; i++) {
        this.selectedIds += selection[i].fileId + ','
      }
      if (this.selectedIds.length > 0) {
        this.selectedIds = this.selectedIds.substr(0, this.selectedIds.length - 1)
      }
      this.multipleSelection = selection
    },
    downloadFile () {
      if (this.selectedIds == null || this.selectedIds === '') {
        this.$message({
          message: '请选择需要下载的文件',
          type: 'warning'
        })
        return
      }
      let ids = this.selectedIds.split(',')
      if (ids.length > 1) {
        this.$message({
          message: '目前仅支持单文件下载',
          type: 'warning'
        })
        return
      }
      location.href = this.$http.urlPri + 'file/download?fileId=' + this.selectedIds
    },
    deleteFile () {
      this.$http.delete('file/remove', {
        fileId: this.selectedIds
      }).catch(err => {
        nconsole.log(err)
      })
      let val = this.multipleSelection
      if (val) {
        val.forEach(val => {
          this.fileList.forEach((v, i) => {
            if (val.fileId === v.fileId) {
              this.fileList.splice(i, 1)
            }
          })
        })
      }
      // 清除选中状态
      this.$refs.multipleTable.clearSelection()
      this.loadInputText()
    },
    uploadHandler (response, state) {
      this.$refs.uploader.fileList = []
      if (state === 'success') {
        this.$message({
          message: '上传成功',
          type: 'success'
        })
        if (response.data && response.data.length > 0) {
          for (let i = 0; i < response.data.length; i++) {
            if (response.data[i] != null && (response.data[i].fileId != null || response.data[i].fileId !== '')) {
              this.fileList.push(response.data[i])
            }
          }
          this.$eventBus.$emit('successHandler', this.fileList)
          this.innerVisible = false
          this.loadInputText()
        }
      } else {
        if (this.failureTips === true) {
          this.$message({
            message: '上传失败,请检查网络是否正常',
            type: 'warning'
          })
        } else {
          this.$eventBus.$emit('failedHandler', response)
        }
      }
    },
    loadInputText () {
      this.inputText = ''
      for (let i = 0; i < this.fileList.length; i++) {
        if (typeof (this.fileList[i].fileName) !== 'undefined' && this.fileList[i].fileName !== '') {
          this.inputText += this.fileList[i].fileName + ';'
        }
      }
      if (this.inputText.length > 0) {
        this.inputText = this.inputText.substr(0, this.inputText.length - 1)
      }
      this.$eventBus.$emit('getFileNames', this.recordId, this.inputText, this.fileList, this.column, this.classId)
    },
    closeDialog (done) {
      this.$eventBus.$emit('getFileNames', this.recordId, this.inputText, this.fileList, this.column, this.classId)
      done()
    },
    close () {
      this.$eventBus.$emit('getFileNames', this.recordId, this.inputText, this.fileList, this.column, this.classId)
      this.outerVisible = false
    }
  }
}
</script>
<style lang="less" scoped>
    /deep/ .el-table th>.cell{
        text-align: center;
    }

    /deep/ .el-form--inline .el-form-item{
        margin: 0;
    }

    /deep/ .el-button--mini{
        padding: 7px 8px;
    }

    /deep/ .el-button--primary.is-disabled{
        color: #FFF;
        background-color: #a0cfff;
        border-color: #a0cfff;
    }
</style>

```

# 3、console.js

```javascript

export default {
    info() {
        console.info.apply(console,arguments);
    },
    log() {
        console.info.apply(console,arguments);
    },
    error() {
        console.error.apply(console,arguments);
    },
    warn() {
        console.warn.apply(console,arguments);
    }
}

```

# 4、Uploader.vue

```vue
<template>
    <el-upload
        class="uploader"
        ref="upload"
        :multiple="multiple"
        :action="action"
        :on-change="onChange"
        :on-remove="onRemove"
        :file-list="fileList"
        :auto-upload="false">
        <el-button slot="trigger" size="small" type="primary" class="el-icon-circle-plus-outline">选取文件</el-button>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload" :disabled="!fileList.length" class="el-icon-upload">上传</el-button>
        <div slot="tip" class="el-upload__tip">{{getTipMsg()}}</div>
    </el-upload>
</template>
<script>
    import axios from 'axios';
    import { Loading } from 'element-ui';

    export default {
        name: "uploader",
        props: {
            multiple: {
                type: Boolean,
                default: false
            },
            action: {
                type: String,
                default: ""
            },
            accept: {
                type: String,
                default: "*"
            },
            fileSize: {
                type: Number,
                default: 2
            },
            column: {
                type: String,
                default: ""
            },
            recordId: {
                type: String,
                default: ""
            },
            fileVerify: {
                type: Boolean,
                default:false
            },
            fileVerifyHandler: {
                type: Function
            }
        },
        data() {
            return {
                uploadForm: new FormData(),
                fileList: [],
                uploadUrl:""
            };
        },
        mounted() {
            this.uploadUrl = this.$http.urlPri + this.action;
        },
        methods: {
            beforeUpload(file) {
                if(this.recordId == null || this.recordId == ""){
                    this.$message({
                        message: 'recordId(当前业务的主键Id)是必须提供的参数!',
                        type: 'warning'
                    });
                    return false;
                }
                if(this.column == null || this.column == ""){
                    this.$message({
                        message: 'column(当前业务的附件字段名称)是必须提供的参数!',
                        type: 'warning'
                    });
                    return false;
                }
                let isMatchExtension = false;
                const test = file.name.substring(file.name.lastIndexOf('.') + 1);
                if (this.accept != "*") {
                    let extensions = this.accept.split(",");
                    for (let index in extensions) {
                        if (extensions[index] === test) {
                            isMatchExtension = true;
                            break;
                        }
                    }
                } else {
                    isMatchExtension = true;
                }
                let isMatchFileSize = file.size / 1024 / 1024 < this.fileSize
                if (!isMatchExtension) {
                    this.$message({
                        message: '上传文件只能是' + this.accept + '格式!',
                        type: 'warning'
                    });
                }
                if (!isMatchFileSize) {
                    this.$message({
                        message: '上传文件大小不能超过 ' + this.fileSize + 'MB!',
                        type: 'warning'
                    });
                }
                return isMatchExtension && isMatchFileSize
            },
            submitUpload() {
                let loadingInstance = Loading.service({ fullscreen: true,text: "正在上传，请稍候……"});
                let that = this;
                let isAllowed = true;
                if(this.fileList.length == 0){
	                this.$message({
		                message: '请选择需要上传的文件',
		                type: 'warning'
	                });
	                return false;
                }
                //@TODO 自定义校验文件
                if(that.fileVerify === true){
                    if(that.fileVerifyHandler && typeof(that.fileVerifyHandler) === "function"){
                        let verifyStatus = that.fileVerifyHandler(this.fileList);
                        if(verifyStatus === false){
                            return false;
                        }
                    }
                }
                this.uploadForm.append("recordId",this.recordId);
                this.uploadForm.append("column",this.column);
                for (let i = 0; i < this.fileList.length; i++) {
                    if (this.beforeUpload(this.fileList[i])) {
                        this.uploadForm.append("file", this.fileList[i].raw);
                    } else {
                        isAllowed = false;
                        break;
                    }
                }
                if (isAllowed) {
                    axios.post(this.uploadUrl, this.uploadForm, {
                        headers: {
                            'Content-Type': 'multipart/form-data'
                        }
                    })
                        .then(function (response) {
                            that.$emit("uploadHandler",response,"success");
                            that.$refs['upload'].clearFiles();
                            that.$nextTick(() => {
                            	that.resetDataForm();
                                loadingInstance.close();
                            });
                        })
                        .catch(function (error) {
                            that.$emit("uploadHandler",error,"fail");
                            that.$nextTick(() => {
	                            that.resetDataForm();
                                loadingInstance.close();
                            });
                        });
                }else{
                    that.$nextTick(() => {
	                    that.resetDataForm();
                        loadingInstance.close();
                    });
                }
            },
            onChange(file, fileList) {
                this.fileList = fileList;
            },
            onRemove(file, fileList) {
                this.fileList = fileList;
            },
            resetDataForm(){
            	this.uploadForm.delete("recordId");
            	this.uploadForm.delete("column");
            	this.uploadForm.delete("file");
            },
            getTipMsg() {
                if (this.accept == "*") {
                    return '文件不能超过 ' + this.fileSize + 'MB!';
                } else {
                    return '文件支持' + this.accept + '格式,且不能超过 ' + this.fileSize + 'MB!';
                }
            }
        }
    }
</script>
<style lang="less" scoped>
    .el-upload__tip {
        font-size: 12px;
        font-weight: bold;
    }

    /deep/ .el-upload--text {
        width: auto;
        height: auto;
        overflow: inherit;
        border: none;
    }
</style>

```

