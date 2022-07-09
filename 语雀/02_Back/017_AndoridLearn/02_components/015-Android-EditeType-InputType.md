# Android-EditeType-InputType

- 类似input输入框中的type属性。主要对输入框输入类型做了限定。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent">

    <!--输入类型为密码形式-->
    <EditText
        android:id="@+id/et_one"
        android:padding="10dp"
        android:gravity="left"
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
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
    </EditText>

</LinearLayout>
```

![image-20220619091002175](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220619091002175.png)