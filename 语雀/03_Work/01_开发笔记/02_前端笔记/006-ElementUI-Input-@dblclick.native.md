# ElementUI-Input-@dblclick.native



1、在el-input中通过` @dblclick.native`指定方法名。即可绑定双击事件。

```html
  <el-input id="SQDW"
                              v-model="primaryData.SQDW"
                              :show-word-limit="true"
                              :maxlength="30"
                              @dblclick.native="selectSQDW()"
                              placeholder="双击点选">
                    </el-input>
```

2、对应的方法

```js
 methods:{
        selectSQDW(){
            console.log("触发了双击事件。。。")
        }
    }
```

