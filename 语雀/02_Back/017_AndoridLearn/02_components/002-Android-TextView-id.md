# Android-TextView-id

- 设置组件的ID

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    
    <!--为组件设置一个ID,这个ID可以在Java中获取到-->    
    <TextView
        android:id="@+id/tv_one"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

- 这个ID可以在Java中获取到

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
        setContentView(R.layout.activity_main);
        //根据ID获取到组件
        TextView textView = findViewById(R.id.tv_one);
    }
}
```

