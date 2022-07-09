# Button-#setOnTouchListener()-Bind-OnTouchClick

> Button组件提供了`#setOnTouchListener()` 来绑定点击事件，当用户点击按钮触发该`onTouch()`事件，`#getAction()` 为用户按前，按后的触发时机。

```java
package com.example.androidlearn3;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        //1.根据ID获取btn01
        Button btn01 = this.findViewById(R.id.btn_01);
        
        //2.绑定触摸按钮。
        btn01.setOnTouchListener((view, motionEvent)->{
            Log.i("WNX",motionEvent.getAction()+"");
            return true;
       });
    }
}
```

![image-20220619082355512](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220619082355512.png)