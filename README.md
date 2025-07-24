# c_c-_study_record

## c
1. 배열과 배열 포인터의 관계
```
&arr      == ptr  
arr       == *ptr   
arr       == *ptr    ==     ptr[0]
arr[0] == ptr[0][0]
```
```c
#include  <stdio.h>

int main(void) {
    int arr[3] = { 1, 2, 3 };
    int (*ptr)[3] = &arr;  
    printf("%d\n", (*(*ptr + 0) + 0));  // 1
    printf("%d\n", ptr[0][1]);      // 2
    printf("%d\n", ptr[0][2]);      // 3
    return 0;
}
```

2. 응용포인트배열부터 계속 이어서 하기


## c++