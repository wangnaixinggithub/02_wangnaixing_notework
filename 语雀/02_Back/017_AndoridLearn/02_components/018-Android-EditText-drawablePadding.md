# Android-EditText-drawablePadding

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent">

    <!--drawablePadding 输入的文字距离图标的距离-->
    <EditText
        android:id="@+id/et_one"
        android:padding="10dp"
        android:gravity="left"
        android:drawableLeft="@drawable/ic_baseline_person_24"
        android:drawablePadding="30dp"
        android:textSize="@dimen/wangnaixing_margin"
        android:textStyle="bold"
        android:inputType="textPassword"
        android:shadowColor="@color/red"
        android:shadowRadius="5"
        android:shadowDx="2"
        android:shadowDy="3"
        android:textColor="@color/black"
        android:background="@color/red"
        android:hint="请输入用户名"
        android:textColorHint="@color/purple_200"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
    </EditText>

</LinearLayout>
```

![image-20220619092616711](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220619092616711.png)