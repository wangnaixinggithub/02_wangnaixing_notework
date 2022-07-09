# Add Button Color Selector

> 在IDEA开发工具中，添加按钮颜色选择器。并进行配置使用

# 1、New Directory And Create Color Resource File



![image-20220618173319614](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618173319614.png)![image-20220618173343859](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618173343859.png)2、Input FileName And Click OK

![image-20220618173544795](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618173544795.png)

# 3、Need Config More Item

- 将颜色以item的形式注册进选择器。

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="@color/purple_200" android:state_pressed="true"/>
    <item android:color="@color/teal_200"/>
</selector>
```

# 4、Use It

```xml
    <!--按钮的背景着色使用配置的图标-->
    <Button
        android:text="我是按钮"
        android:background="@drawable/btn_select"
        android:backgroundTint="@color/btn_color_selector"
        android:layout_width="200dp"
        android:layout_height="200dp">
    </Button>
```

- 未点击

![image-20220618174107884](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618174107884.png)

- 点击后

![image-20220618174056666](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618174056666.png)