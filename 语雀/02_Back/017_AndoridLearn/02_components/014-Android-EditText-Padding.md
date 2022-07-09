# Android-EditText-Padding

> 设定内部距，通常有四个方向分别是 上 右 下 左

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent">

    <!--内容和EditText的内边距-->
    <EditText
        android:id="@+id/et_one"
        android:padding="10dp"
        android:gravity="center"
        android:textSize="@dimen/wangnaixing_margin"
        android:textStyle="bold"
        android:shadowColor="@color/red"
        android:shadowRadius="5"
        android:shadowDx="2"
        android:shadowDy="3"
        android:textColor="@color/black"
        android:background="@color/red"
        android:hint="请输入用户名"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
    </EditText>

</LinearLayout>
```

![image-20220619090428659](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220619090428659.png)