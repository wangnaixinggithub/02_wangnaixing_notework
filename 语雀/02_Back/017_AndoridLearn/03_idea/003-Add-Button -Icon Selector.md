# Add Button Icon Selector

> 在IDEA开发工具中，添加按钮图标选择器。并进行配置使用

# 1、Create Drawable Resource File

![image-20220618155810523](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618155810523.png)

# 2、Input FileName And Click OK

![image-20220618155906400](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618155906400.png)

# 3、Need Config More Item

- 将Icon以图标的形式注册进选择器。

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/ic_baseline_aspect_ratio_24" android:state_pressed="true"/>
    <item android:drawable="@drawable/ic_baseline_add_photo_alternate_24"/>
</selector>
```

# 4、Use It

```xml
    <!--按钮的背景使用配置的图标-->
    <Button
        android:background="@drawable/my_btn_select"
        android:layout_width="200dp"
        android:layout_height="200dp">
    </Button>
```

- 未点击

![image-20220618171858619](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618171858619.png)

- 点击后

![image-20220618172142080](image-20220618172142080.png)