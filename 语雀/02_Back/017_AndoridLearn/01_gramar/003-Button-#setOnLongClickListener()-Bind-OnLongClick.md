# Button-#setOnLongClickListener()-Bind-OnLongClick

> Button组件提供了`#setOnClickListener()` 来绑定点击事件，当用户长点击按钮时，就会触发一次回调。

```java
package com.example.androidlearn3;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //1.根据ID获取btn01
        Button btn01 = this.findViewById(R.id.btn_01);
        
        //2.绑定点击监听，传入匿名接口实现类。重写 #onLongClick()方法
        btn01.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View view) {
                Log.i("WNX","用户长按按钮，触发长按事件回调");
                return false;
            }
        });
    }
}
```

# 2、Validation Test

- 我在模拟机器长点击了三次，控制台成功打印了三次语句输出。

![image-20220618180351610](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618180351610.png)