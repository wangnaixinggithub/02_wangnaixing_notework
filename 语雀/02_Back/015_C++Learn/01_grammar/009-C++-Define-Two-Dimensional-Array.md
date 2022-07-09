# C++-Define-Two-Dimensional-Array

- 先声明后赋值

```javascript
#include <iostream>
using namespace std;
int main() {
    int arr[2][3];

    arr[0][0] = 1;
    arr[0][1] = 2;
    arr[0][2] = 3;

    arr[1][0] = 4;
    arr[1][1] = 5;
    arr[1][2] = 6;


    int row = sizeof(arr) / sizeof(arr[0]);
    int column = sizeof(arr[0]) / sizeof (arr[0][0]);


    for (int i = 0; i < row; ++i) {
        for (int j = 0; j < column; ++j) {
            cout<<arr[i][j]<<endl;
        }
    }
    
    return 0;
}
//1
//2
//3
//4
//5
//6
```

- 声明时立刻赋值

```c++
#include <iostream>
using namespace std;
int main() {
    int arr[2][3] = {
            {1,2,3},
            {4,5,6}
    };

    
    int row = sizeof(arr) / sizeof(arr[0]);
    int column = sizeof(arr[0]) / sizeof (arr[0][0]);


    for (int i = 0; i < row; ++i) {
        for (int j = 0; j < column; ++j) {
            cout<<arr[i][j]<<endl;
        }
    }

    return 0;
}

```

