# Android-HelloWorld

# 1、XML Java One By One

`MainActivity.java`  `activity_main.xml` 他们之间是一一对应，一个控制布局一个处理逻辑。	

![image-20220618125627906](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618125627906.png)

```java
package com.example.androidlearn2;

import android.os.Bundle;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //1.设置视图
        setContentView(R.layout.activity_main);
      
        //2.根据ID获取TextView
        TextView tvOne = findViewById(R.id.tv_one);
      
        //3.设置TextView innerText
        tvOne.setText("王乃醒是一个帅哥！");
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/tv_one"
        android:layout_width="match_parent"
        android:layout_height="match_parent"></TextView>

</LinearLayout>
```

