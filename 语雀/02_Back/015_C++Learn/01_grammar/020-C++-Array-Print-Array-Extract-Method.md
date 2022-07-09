# Array-Print-Array-Extract-Method

# 1、Pass Param is Address Value

- 1、数组名代表了当前数组的指针，可以用来遍历数组元素，通过指针的方式。

```c++
#include <iostream>
using namespace std;

/**
 * 打印数组元素
 * @param arr 数组
 * @param length 长度
 */
void printArray(int* arr,int length){
    for (int i = 0; i < length; ++i) {
        cout<<*arr;
        arr++;
    }
}


int main() {
    int arr[5] = {50,40,30,20,10};
    int length = sizeof arr / sizeof arr[0];
    printArray(arr,length);

}
```

- 2、也可以利用数组的特性，数组名[下标]的方式。

```c++
#include <iostream>
using namespace std;


/**
 * 打印数组元素
 * @param arr 数组
 * @param length 长度
 */
void printArray(int* arr,int length){
    for (int i = 0; i < length; ++i) {
        cout<<arr[i];
    }
}


int main() {
    int arr[5] = {50,40,30,20,10};
    int length = sizeof arr / sizeof arr[0];
    printArray(arr,length);

}
```

- 3、冒泡排序

```c++
#include <iostream>
using namespace std;

/**
 * 交换两个数的值
 * @param p 引用p
 * @param q 引用q
 */
void swap(int *p,int *q){
    int  temp = *p;
    *p = *q;
    *q = temp;

}
/**
 * 打印数组元素
 * @param arr 数组
 * @param length 长度
 */
void printArray(int* arr,int length){
    for (int i = 0; i < length; ++i) {
        cout<<arr[i]<<",";
    }
}

/**
 * 冒泡排序
 * @param arr 数组
 * @param length 长度
 */
void bubbleSort(int* arr,int length){
    for (int i = 0; i < length; ++i) {
        for (int j = 0; j < length; ++j) {
            if (arr[j] > arr[j+1]){
                swap(&arr[j],&arr[j+1]);
            }
        }
    }

}


int main() {
    int arr[5] = {50,40,30,20,10};
    int length = sizeof arr / sizeof arr[0];
    bubbleSort(arr,length);
    printArray(arr,length);

}
```

​	