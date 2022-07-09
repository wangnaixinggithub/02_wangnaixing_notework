# Use-RowSpan-Handle-Table-column

- `rowspan`用于合并列，使用此关键字的列会多出一列提供分配。不参与列，也同样多一列分配。

# 1、No RowSpan

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用rowspan合并列</title>
</head>
<body>
 <table style="width: 100%; border-collapse:collapse;" border="1">
        <colgroup>
            <col width="7%"></col>
            <col width="15%"></col>
            <col width="20%"></col>
            <col width="9%"></col>
            <col width="20%"></col>
            <col width="9%"></col>
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
            <tr>
                <td>31</td>
                <td>32</td>
                <td>33</td>
                <td>34</td>
                <td>35</td>
                <td>36</td>
                <td>37</td>
            </tr>

        </tbody>
</table>

</body>
</html>
```

![image-20220527114612222](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220527114612222.png)

# 2、rowspan="3"

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用rowspan合并列</title>
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
                <td rowspan="3">11</td>
                <td>12</td>
                <td>13</td>
                <td>14</td>
                <td>15</td>
                <td>16</td>
                <td>17</td>
                <!--可以再添加一列-->
                <td>Can add Row</td>

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
            <tr>
                <td>31</td>
                <td>32</td>
                <td>33</td>
                <td>34</td>
                <td>35</td>
                <td>36</td>
                <td>37</td>
            </tr>

        </tbody>
</table>

</body>
</html>
```

![image-20220527114852575](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220527114852575.png)

# 3、rowspan="2"

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用rowspan合并列</title>
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
                <td rowspan="2">11</td>
                <td>12</td>
                <td>13</td>
                <td>14</td>
                <td>15</td>
                <td>16</td>
                <td>17</td>
                <!--可以再添加一列-->
                <td>Can add Row</td>

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
            <tr>
                <td>31</td>
                <td>32</td>
                <td>33</td>
                <td>34</td>
                <td>35</td>
                <td>36</td>
                <td>37</td>
                <!--未参与合并的列，    可以再添加一列-->
                <td>Can add Row</td>
            </tr>

        </tbody>
</table>

</body>
</html>
```

![image-20220527120502020](041-Use-RowSpan-Handle-Table-column.assets/image-20220527120502020.png)
