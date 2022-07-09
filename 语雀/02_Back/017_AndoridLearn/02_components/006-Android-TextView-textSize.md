# Android-TextView-textSize

- 设置文本的大小

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--设置文本的大小 单位dp-->
    <TextView
        android:id="@+id/tv_one"
        android:text="王乃醒是一个大帅哥"
        android:textColor="@color/Red"
        android:textStyle="bold"
        android:textSize="@dimen/wangnaixing_margin"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

- 能引用到`/resource/values/dimen.xml`的字体大小

```xml
<resources>
    <dimen name="fab_margin">16dp</dimen>
    <dimen name="wangnaixing_margin">30dp</dimen>
</resources>
```

![image-20220618144448805](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618144448805.png)