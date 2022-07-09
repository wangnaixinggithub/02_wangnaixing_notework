# Android-EditText-#getText()#toString()

> 获取到用户在输入框中输入的内容

```java
package com.example.androidlearn3;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.text.Editable;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btn = this.findViewById(R.id.btn_one);
        EditText editText = this.findViewById(R.id.et_one);

        btn.setOnClickListener(view -> {
            //获取用户的输入
            String text = editText.getText().toString();
            Log.i("WNX",text);
        });
    }
}
```

