# Use-Table-colgroup-col-width

- 使用Colgroup可以确定表格列的宽度。

# 1、`<col width="10%"></col>`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>table-colgroup-col-width</title>
</head>
<body>
 <table style="width: 100%; border-collapse:collapse;" border="1">
        <colgroup>

            <col width="10%"></col>
            <col width="10%"></col>
            <col width="10%"></col>
            <col width="10%"></col>
            <col width="10%"></col>
            <col width="10%"></col>
            <col width="10%"></col>

        </colgroup>
        <tbody>
            <tr>
                <td>11</td>
                <td>12</td>
                <td>13</td>
                <td>14</td>
                <td>15</td>
                <td>16</td>
                <td>17</td>
            </tr>
            <tr>
                <td>21</td>
                <td>22</td>
                <td>23</td>
                <td>24</td>
                <td>25</td>
                <td>26</td>
                <td>27</td>
            </tr>

        </tbody>
</table>

</body>
</html>
```

![image-20220527120114893](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220527120114893.png)

# 2、 `<col width="20%"></col>`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>table-colgroup-col-width</title>
</head>
<body>
 <table style="width: 100%; border-collapse:collapse;" border="1">
        <colgroup>

            <col width="5%"></col>
            <col width="5%"></col>
            <col width="5%"></col>
            <col width="20%"></col>
            <col width="20%"></col>
            <col width="20%"></col>
            <col width="20%"></col>

        </colgroup>
        <tbody>
            <tr>
                <td>11</td>
                <td>12</td>
                <td>13</td>
                <td>14</td>
                <td>15</td>
                <td>16</td>
                <td>17</td>
            </tr>
            <tr>
                <td>21</td>
                <td>22</td>
                <td>23</td>
                <td>24</td>
                <td>25</td>
                <td>26</td>
                <td>27</td>
            </tr>

        </tbody>
</table>

</body>
</html>
```

![image-20220527120216150](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220527120216150.png)