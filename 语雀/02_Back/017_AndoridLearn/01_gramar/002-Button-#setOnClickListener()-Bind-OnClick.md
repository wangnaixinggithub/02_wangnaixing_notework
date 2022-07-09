# Button-#setOnClickListener()-Bind-OnClick

# 1、Use it

> Button组件提供了`#setOnClickListener()` 来绑定点击事件，当用户点击按钮时，就会触发一次回调。

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
		
        //2.绑定点击监听，传入匿名接口实现类。重写 #onClick()方法
        btn01.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Log.i("WNX","按钮1触发了点击事件");
            }
        });
    }
}
```

# 2、Validation Test

- 我在模拟机器点击了三次，控制台成功打印了三次语句输出。

![image-20220618175755461](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618175755461.png)