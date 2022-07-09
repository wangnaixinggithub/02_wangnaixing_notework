# Android-Text-gravity

- 设置组件内容的对其方式，在此例中，我选择水平居中对齐。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--设置控件内容的对齐方式-->
    <TextView
        android:id="@+id/tv_one"
        android:text="王乃醒是一个大帅哥"
        android:textColor="@color/Red"
        android:textStyle="bold"
        android:gravity="center_horizontal"
        android:textSize="@dimen/wangnaixing_margin"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

- 系统提供的对齐方案有多种，可选。

```xml
    <attr name="gravity">
        <flag name="top" value="0x30" />>
        <flag name="bottom" value="0x50" />
        <flag name="left" value="0x03" />
        <flag name="right" value="0x05" />
        <flag name="center_vertical" value="0x10" />
        <flag name="fill_vertical" value="0x70" />
        <flag name="center_horizontal" value="0x01" />
        <flag name="fill_horizontal" value="0x07" />
        <flag name="center" value="0x11" />
        <flag name="fill" value="0x77" />
        <flag name="clip_vertical" value="0x80" />
        <flag name="clip_horizontal" value="0x08" />
        <flag name="start" value="0x00800003" />
        <flag name="end" value="0x00800005" />
    </attr>
```

