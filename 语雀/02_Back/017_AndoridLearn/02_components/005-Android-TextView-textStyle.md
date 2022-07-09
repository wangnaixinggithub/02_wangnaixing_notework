# Android-TextView-textStyle

- `textStyle`设置文本内容样式。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--设置文本的样式，可以对文本进行加粗|斜体-->
    <TextView
        android:id="@+id/tv_one"
        android:text="王乃醒是一个大帅哥"
        android:textColor="@color/Red"
        android:textStyle="bold"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

![image-20220618143841637](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618143841637.png)

```xml
    <attr name="textStyle">
        <!--默认-->
       <flag name="normal" value="0" />
        <!--加粗-->
        <flag name="bold" value="1" />
        <!--斜体-->
        <flag name="italic" value="2" />
    </attr>
```

