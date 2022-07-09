# Bubble-Sort

```java
#include <iostream>

void exchangeTwoNumber(int a, int b);
void printArray(int arr);

using namespace std;
int main() {
    int arr[10] = {10,9,8,7,6,5,4,3,2,1};
    int length = sizeof(arr)/sizeof(arr[0]);

    for (int i =0; i < length;i++ ){
        for (int j = 0; j < length; ++j) {
            if (arr[j] > arr[j+1]){
                int temp = arr[j] ;
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }

    for (const auto &item: arr){cout<<item<<endl;}

    return 0;
}

```

