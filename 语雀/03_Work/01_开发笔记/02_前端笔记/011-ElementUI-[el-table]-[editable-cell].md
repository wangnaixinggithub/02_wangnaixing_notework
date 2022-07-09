# ElementUI-[el-table]-[editable-cell]

> 这种应用场景在于：填写主表、以及填写关联子表数据提交到数据库保存。

# 1、colspan

表格行tr里面的td使用属性 `colspan="5"`，合并5个单元格。再使用一个表格即可。

```vue
 <td class="td_value_column column_top_bottom_right" colspan="5" id="czxkTable">
    <el-table id="CZGZXK" required :data="formData.czxkData">
              <el-table-column  class="column" type="index"  width="100"  label="序号"></el-table-column>
            
             <el-table-column class="column"label="值班员">
                     <editable-cell :show-input="row.editMode" 
                                           slot-scope="{row}" 
                                           v-model="row.ZBY"
                                           :show-word-limit="true" 
                                           :maxlength="15" 
                                           :disabled="authorityCzxkData">
                                <span slot="content">{{row.ZBY}}</span>
                      </editable-cell>
               </el-table-column>
        
               <el-table-column class="column" label="联系电话">
                   <editable-cell id="ZBYLXDH" 
                                  :show-input="row.editMode" 
                                  slot-scope="{row}" v-model="row.ZBYLXDH"
                                  @change="checkPhone(row.ZBYLXDH,'ZBYLXDH')" 
                                  :show-word-limit="true" 
                                  :maxlength="15" 
                                  :disabled="authorityCzxkData">
                                <span slot="content">{{row.ZBYLXDH}}</span>
                    </editable-cell>
                 </el-table-column>
        
                 <el-table-column class="column" label="工作负责人">
                     <editable-cell 
                                :show-input="row.editMode" 
                                slot-scope="{row}" 
                                v-model="row.GZFZR"
                                :show-word-limit="true" 
                                :maxlength="15" 
                                 :disabled="authorityCzxkData">
                                <span slot="content">{{row.GZFZR}}</span>
                      </editable-cell>
                  </el-table-column>
        
                  <el-table-column class="column" label="联系电话">
                      <editable-cell id="GZFZRLXDH" 
                                 :show-input="row.editMode"
                                 slot-scope="{row}"
                                 v-model="row.GZFZRLXDH"
                                 @change="checkPhone(row.GZFZRLXDH,'GZFZRLXDH')" 
                                 :show-word-limit="true" 
                                 :maxlength="15" 
                                 :disabled="authorityCzxkData">
                                <span slot="content">{{row.GZFZRLXDH}}</span>
                      </editable-cell>
                  </el-table-column>
        
                  <el-table-column label="+" 
                                   width="50" 
                                   class="column"
                                   v-if="showButtonAddCZXk">
                        <template slot="header" slot-scope="scope">
                                <el-button type="primary" size="mini" circle icon="el-icon-plus"
                                           @click="addCzxkData(scope.column, scope.$index)">
                                </el-button>
                         </template>
                          <template slot-scope="scope">
                                <el-button type="danger"
                                           size="mini" 
                                           circle 
                                           class="el-icon-minus"
                                           @click="deleteCzxkData(scope.$index, scope.row)"
                                           v-if="showButtonAddCZXk">
                                </el-button>
                          </template>
                   </el-table-column>  
     </el-table>
</td>
```

# 2、import

```javascript
import editableCell from "@/common/plugins/EditableCell.vue";
 components:{
        editableCell
    }
```

使用`<editable-cell></editable-cell>`在template中使用

- `:show-input`属性展示的input是可以编辑的模式，
- `v-model`数据绑定，
- `show-word-limit`开启字词限制，
- `maxlength`最大长度为15，
- `:disabled`是否禁用接收一个布尔值，true或者false.
- 可以根据业务逻辑，放这个authorityCzxkData放在`计算属性`中,进行动态控制。
- 通过slot插槽来展示值。

```javascript
export default {
    computed:{
        //权限 => CzxkData
        authorityCzxkData(){
                 let self =this;
                       	 
                if(self.urlParams.atvdID.includes("TCSQ") && self.urlParams.status=="A" && self.primaryData.CZGZXK=='是'){
                       return false;
                }else{
                       return true;
                 }
        }
  },
```

# 3、add Or Delete Row

- 1、在内嵌表格中加多一行，专门用来显示添加图标按钮，删除图标按钮。

```vue
   <el-table-column 
   				label="+" 
   				width="50" 
   				class="column"
                v-if="showButtonAddCZXk">
       
          <template slot="header" slot-scope="scope">
               <el-button type="primary" 
                          size="mini" 
                          circle icon="el-icon-plus"
                          @click="addCzxkData(scope.column, scope.$index)">
              </el-button>
          </template>
       
          <template slot-scope="scope">
                <el-button type="danger" 
                           size="mini" 
                           circle 
                           class="el-icon-minus"
                           @click="deleteCzxkData(scope.$index, scope.row)"
                           v-if="showButtonAddCZXk">
                </el-button>
          </template>
    </el-table-column>
```

- 2、这一行什么时候显示呢？通过`showButtonAddCZXk`计算属性来控制。

```vue
  <el-table-column 
   				label="+" 
   				width="50" 
   				class="column"
                v-if="showButtonAddCZXk">
  </el-table-column>
```

```javascript
export default {
       computed:{
       	 showButtonAddCZXk(){
                let self =this;
                if(self.urlParams.atvdID.includes("TCSQ") && self.urlParams.status=="A" && self.primaryData.CZGZXK=='是'){
                    return true;
                }else{
                    return false;
                }
            },
       }
}
```

- 对数组的元素的添加删除，通过双向绑定，就能得到动态变化。添加使用数组的push()方法，删除使用splice(index,1)

```javascript
export default {   
    data(){
        return{
            formData:{czxkData:[]}
        }
   	 },
    methods:{	
        //1.添加厂站许可子表
        addCzxkData(column, event){
            let self =this;
            let czxkData =self.formData.czxkData;
            let newRow = {
                "ID": "",
                "ZBY": "",
                "ZBYLXDH": "",
                "GZFZR": "",
                "GZFZRLXDH":"",
                "SSSJ": self.primaryData.ID
            };
            czxkData.push(newRow);
        },
        
        //2.删除厂站许可子表
        deleteCzxkData(rowIndex, event) {
            this.formData.czxkData.splice(rowIndex, 1);
        },
      }
}
```

# 	4、EditableCell.vue

```vue
<!-- 在需要行编辑的ElemnetUI 的table中引入当前组件，即可实现行编辑 -->
<template>
    <div @click="onFieldClick" class="edit-cell">
        <el-tooltip v-if="!editMode && !showInput && showTips"
                    :placement="toolTipPlacement"
                    :open-delay="toolTipDelay"
                    :content="toolTipContent">
            <div tabindex="0" @keyup.enter="onFieldClick">
                <slot name="content"></slot>
            </div>
        </el-tooltip>
        <div v-else-if="!editMode && !showInput" tabindex="0" @keyup.enter="onFieldClick">
            <slot name="content"></slot>
        </div>

        <component :is="editableComponent"
                   v-if="editMode || showInput"
                   ref="input"
                   @focus="onFieldClick"
                   @keyup.enter.native="onInputExit"
                   v-on="listeners"
                   v-bind="$attrs"
                   v-model="model">
            <slot name="edit-component-slot"></slot>
        </component>
    </div>
</template>

<script>
    export default {
        name: "editable-cell",
        inheritAttrs: false,
        props: {
            value: {
                type: [String, Number],
                default: ""
            },
            toolTipContent: {
                type: String,
                default: "点击修改"
            },
            toolTipDelay: {
                type: Number,
                default: 500
            },
            toolTipPlacement: {
                type: String,
                default: "top-start"
            },
            showInput: {
                type: Boolean,
                default: false
            },
            editableComponent: {
                type: String,
                default: "el-input"
            },
            closeEvent: {
                type: String,
                default: "blur"
            },
            showTips: {
                type: Boolean,
                default: true
            }
        },
        data() {
            return {
                editMode: false
            };
        },
        computed: {
            model: {
                get() {
                    return this.value;
                },
                set(val) {
                    this.$emit("input", val);
                }
            },
            listeners() {
                return {
                    [this.closeEvent]: this.onInputExit,
                    ...this.$listeners
                };
            }
        },
        methods: {
            onFieldClick() {
                this.editMode = true;
                this.$nextTick(() => {
                    let inputRef = this.$refs.input;
                    if (inputRef) {
                        inputRef.focus();
                    }
                });
            },
            onInputExit() {
                this.editMode = false;
            },
            onInputChange(val) {
                this.$emit("input", val);
            }
        }
    };
</script>
<style>
    .edit-cell {
        min-height: 20px;
    }
</style>

```

