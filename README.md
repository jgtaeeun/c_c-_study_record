# c_c-_study_record

## c
1. C에서 배열 개수 구할 때 
```c
int arr[5] = { 1,2,3,4, };
printf("sizeof(arr):%d\n", sizeof(arr));
printf("sizeof(arr[0]):%d\n", sizeof(arr[0]));
printf("sizeof(arr[1]):%d\n", sizeof(arr[1]));
printf("sizeof(arr[4]):%d\n", sizeof(arr[4]));
printf("sizeof(arr)/sizeof(arr[0]):%d\n", sizeof(arr)/sizeof(arr[0]));

//실행결과
sizeof(arr):20
sizeof(arr[0]):4
sizeof(arr[1]):4
sizeof(arr[4]):4
sizeof(arr)/sizeof(arr[0]):5
```

```c
char arr[6] = "happy";
printf("sizeof(arr):%d\n", sizeof(arr));
for (int i = 0; i < sizeof(arr); i++)
{
    printf("%c", arr[i]);
}
printf("\n");

//---------------char*의 sizeof는 8바이트 
char* arr2[5] = { "H" ,"A" ,"P","P" ,"Y"};
printf("sizeof(arr2):%d\n", sizeof(arr2));
printf("sizeof(arr2[0]):%d\n", sizeof(arr2[0]));
printf("sizeof(arr2)/sizeof(arr2[0]):%d\n", sizeof(arr2)/ sizeof(arr2[0]));
for (int i = 0; i < sizeof(arr2) / sizeof(arr2[0]); i++)
{
    printf("%s ", arr2[i]);
}

//실행결과
sizeof(arr):6
happy
sizeof(arr2):40
sizeof(arr2[0]):8
sizeof(arr2)/sizeof(arr2[0]):5
H A P P Y
```
2. 주소 연산자 &
- 변수의 주소를 전달하거나 조회할 때는 &가 필수입니다.
- %p 포맷 지정자는 void* 타입의 포인터를 출력하기 위한 것
- C 표준에서 %p는 void* 포인터만 받도록 정의되어 있어요.
- 명시적으로 (void*)로 형변환하는 게 권장됩니다.
```C
int a = 10;
printf("%p", (void*)&a);
```

3. 배열이름 주소
- 배열 이름은 배열 첫 요소의 주소를 의미해서 & 없이 바로 사용 가능
- 포인터 변수는 이미 주소를 담고 있으므로 & 없이 사용
```c
#include <stdio.h>

int main(void) {
    int ary[5];
    printf("ary:%u\n", ary);       //1068497640
    printf("&ary:%u\n", &ary);   //1068497640
    printf("ary+1:%u\n", ary+1);  //1068497644
    printf("&ary+1:%u\n", &ary+1); // 배열 전체 다음 위치, &ary+1:1068497660   

    return 0;
}

```

4. 배열과 배열 포인터의 관계
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

5. 핵심 비교: 일반 포인터 vs. 이중 포인터
- 일반 포인터 (예: char*)
```C
char* p = "hello";

printf("%c\n", *p);    // h
printf("%s\n", p);     // hello
```
- 이중 포인터 (예: char**)
```c
char* ary[] = {"apple", "banana"};
char** p = ary;

printf("%s\n", *p);     // apple
printf("%s\n", p[1]);   // banana
```
6. 상수 
- 반드시 선언과 동시에 초기화를 해줘야 합니다.
```C
const int d =  10; 
//d가 상수기에 d=12은 안됨
```

7. 변수
```C
int b = 10;
int* pb = &b;
*pb = 12;
```

8. const , 포인트
- 값 변경불가
```C
	const int* pb2 = &b;
	//*pb2 = 14; // 포인트를 통해 변경 또한 안됨, 상수포인트라서 값 변경 불가이다.
	b = 25;
	printf("%d\n", b);

	int* pb = &b;   //포인터를 이용한 간접참조로 값 변경은 가능하다.
	*pb = 54;
	printf("%d\n", b); 
	return 0;
```
- 주소값 변경불가
```C
int c = 200;
int d =100;
int* const pc = &c;
// pc = &d ; (x) 주소값 변경불가
*pc =40;
```

- 값, 주소 변경불가
```C
const int* const a; 
```

9. static 변수

|정적 지역 변수|함수 외부(전역)에서 선언된 경우, 전역 정적 변수|
|:--:|:--:|
|한 번만 초기화되고, 함수 호출이 끝나도 값이 유지됩니다.<br>초기화하지 않으면 0으로 자동 초기화됩니다.|이 변수를 선언한 파일 내부에서만 접근할 수 있습니다.<br>다른 파일에서는 이 변수를 볼 수도, 사용할 수도 없습니다.<br>초기화하지 않으면 0으로 자동 초기화됩니다.|

10. unsigned

|키워드|의미|
|:--:|:--:|:--:|
|unsigned int|부호 없는 정수 (0 이상만 저장)|
|int|부호 있는 정수 (음수 포함)|

- 컴퓨터는 4바이트를 연산을 한다.
- unsigned는 부호를 신경쓰지 않기에 그냥 이진수 계산하면 된다.
- 하지만 signed는 부호를 신경쓰기에 8비트씩 나눴을 때 가장 왼쪽이 1이면 음수로 판단해서 1로 다 채워진다.
- 또한 signed의 오른쪽 시프트는 부호비트로 빈칸이 채워진다.

```c
unsigned char uch = 0x7f;    //0111 1111
char ch = 0x7f;              //0111 1111

printf("%#x ", uch);
printf("%#x\n", ch);

unsigned char uch2 = 0x9f;    //1001 1111
char ch2 = 0x9f;               //1111 1111 1111 1111 1111 1111 1001 1111

printf("%#x ", uch2);         
printf("%#x\n", ch2);

//실행결과
0x7f 0x7f
0x9f 0xffffff9f
```

```c
unsigned char uch = 0x7f;    //0111 1111
char ch = 0x7f;              //0111 1111

printf("%#x ", uch << 1);  //0111 1111 -> 1111 1110   fe
printf("%#x\n", uch >> 1); //0111 1111 -> 0011 1111    3f

printf("%#x ", ch << 1); //0111 1111 -> 1111 1110   fe
printf("%#x\n", ch >> 1); //0111 1111 -> 0011 1111   3f


unsigned char uch2 = 0x9f;    //1001 1111
char ch2 = 0x9f;               //1111 1111 1111 1111 1111 1111 1001 1111

printf("%#x ", uch2 << 1);  //1001 1111 -> 1 0011 1110 13e
printf("%#x\n", uch2 >> 1);    //1001 1111 -> 0100 1111 4f

printf("%#x ", ch2 << 1);   //1111 1111 1111 1111 1111 1111  1001 1111 -> 0011 1110 ffffff3e
printf("%#x\n", ch2 >> 1);      //1111 1111 1111 1111 1111 1111  1001 1111 -> 1100 1111 ffffffcf

//실행결과
0xfe 0x3f
0xfe 0x3f
0x13e 0x4f
0xffffff3e 0xffffffcf
```
11. 문자열 상수

12. 문자열함수

||strcpy()|strcpy_s() 차이점
|항목|strcpy()|strcpy_s()|
|:--:|:--:|:--:|
|버퍼 크기 확인|❌ (하지 않음)|✅ (직접 크기 지정 필요)|
|오버플로우 위험|있음 (보안 취약점 원인)|없음 (초과시 복사 안 하고 실패 처리)|
|실패 처리|없음|실패 시 return 값으로 알 수 있음|
|표준|C 표준 라이브러리|C11/C17에서 Annex K (옵션)|
|Visual Studio|사용 가능 <br>(**#define _CRT_SECURE_NO_WARNINGS**)|기본 지원|

13. 문자열배열
- sizeof(배열이름)은 배열크기이다. 
- 널문자를 고려한 배열크기
- 초기화하지 않는 경우 반드시 널문자도 저장해야 한다.

14. 문자 입력함수

|항목|getchar()|scanf(" %c", &ch)|
|:--:|:--:|:--:|
|문자를 하나 읽음|✅|✅|
|공백/엔터 무시|❌ (무시 안 함)|✅ (" %c" 덕분에 무시됨)|
|버퍼 정리 필요 여부|필요 (직접 '\n' 무시)|안 필요 (자동 처리)|
|쓰기 쉬움|간단하지만 조심 필요|조금 더 안정적|
|유연성|문자 하나만 딱 읽음|다양한 포맷 지원|



```c
char n;
scanf("%c", &n);
printf("n:%c\n", n);
scanf(" %c", &n);
printf("n:%c\n", n);
return 0;

// 실행결과 - 엔터무시
a
n:a
b
n:b


// 실행결과 - 공백,엔터무시
a
n:a
     n
n:n
```

```c
char s;
s = getchar();
printf("s:%c\n", s);
return 0;

// 실행결과 - 공백을 읽음
           s
s:

// 실행결과 - 엔터를 읽음
a
s:a
s:

```

```c
char s;
s = getchar();
printf("s:%c\n", s);

// 개행문자 제거 (엔터 처리)
while (getchar() != '\n');

s = getchar();
printf("s:%c\n", s);
return 0;

// 실행결과 
a
s:a
b
s:b

```

15. 입력 타입에 따른 입력 주의할 점

|입력 타입|공백/엔터 신경 써야 하나?|이유|
|:--:|:--:|:--:|
|int, float 등 숫자형|❌ 거의 신경 안 써도 됨|scanf()가 숫자만 읽고 공백은 자동 건너뜀|
|char 문자형|⛔ 주의 필요|%c는 공백도 문자로 읽기 때문|
|char[] 문자열|⛔ 주의 필요 (특히 공백)|%s는 공백에서 입력을 끊기 때문|

```c
int a, b;
scanf("%d", &a);
scanf("%d", &b);
printf("a:%d ", a);
printf("b:%d\n", b);

//실행결과
1
6
a:1 b:6
```

```c
//---------------%c 앞에 " " (공백) 넣기
char ch1, ch2;

scanf(" %c", &ch1);  
scanf(" %c", &ch2);  
printf("ch1:%c\n", ch1);
printf("ch2:%c\n", ch2);

//실행결과
a
2
ch1:a
ch2:2
```

```C
char str[10];
scanf("%s", str);  // 공백에서 끊김  ,"Hello World" 입력하면 → str = "Hello"까지만 저장됨

//해결방법
//fgets -최대 sizeof(str) - 1개의 문자만 읽고, 마지막에 널문자 '\0'을 자동으로 붙입니다.
fgets(str, sizeof(str), stdin);  // 공백 포함 문자열 입력 가능

//실행결과
abce defiglisd
str:abce defi

```



16. 메모리 주소 공간

← 높은 주소 (ex: 0x7fff...)
+-------------------------+
|   스택(Stack)           | <- 지역변수, 함수 호출 스택
+-------------------------+
|   힙(Heap)              | <- malloc 등 동적 메모리
+-------------------------+
|   BSS 영역              | <- 전역/정적 변수 (초기화되지 않은 것)
+-------------------------+
|   데이터(Data) 영역     | <- 전역/정적 변수 (초기화된 것)
+-------------------------+
|   코드(Code) 영역       | <- text segment (함수, 실행코드 등), 함수 하나당 한 개 할당
+-------------------------+
← 낮은 주소 (ex: 0x400000)



```c
const int c = 2;
static int b = 5;
int a = 10;
int arr[5] = { 0, };
for (int i = 0; i < sizeof(arr) / sizeof(arr[0]); i++)
{
    //printf("arr[%d]:%d ", i, arr[i]);
}
function();
printf("스택(Stack) 영역:%p\n", (void*)&c);
printf("데이터(Data) 영역:%p\n", (void*)&b);
printf("스택(Stack) 영역:%p\n", (void*)&a);
printf("스택(Stack) 영역:%p\n", (void*)arr);
printf("코드 영역:%p\n", (void*)function);

//실행결과
스택(Stack) 영역:000000B0EAF1FB94
데이터(Data) 영역:00007FF75343D048
스택(Stack) 영역:000000B0EAF1FBB4
스택(Stack) 영역:000000B0EAF1FBD8
코드 영역:00007FF7534313D4
```
|변수/함수|예상 영역|주소 특성|
|:--:|:--:|:--:|
|int a|Stack|서로 비슷한 높은 주소|
|int arr[]|Stack|a 근처 주소|
|const int c|Stack|또는 RODATA	대부분 스택 (최적화에 따라 다름)|
|static int b|Data Segment|낮은 고정된 주소|
|function|Code (Text)|더 낮은 실행 코드 주소|

17. 동적할당

18. 함수오버로딩
- 매개변수 개수, 매개변수 타입, 매개변수 순서



### 공부 해야할 것
- 문자열 상수 
- 동적할당


## c++
1. 다형성
- 정적 다형성 - 오버로딩.함수템플릿
- 동적 다형성 - 오버라이딩. virtual

2. 벡터정렬
- 기본 정렬 (오름차순)
- 정렬값 반환받을 변수 필요없다.
```C
#include <iostream>
#include <vector>
#include <algorithm> // sort 함수 포함

using namespace std;

int main() {
    vector<int> v = {5, 3, 8, 1, 2};

    sort(v.begin(), v.end()); // 오름차순 정렬

    for (int i : v) {
        cout << i << " ";
    }

    return 0;
}
//출력: 1 2 3 5 8
```

3. 문자열 std::string
- C++에서 문자열을 표현할 때는 보통 std::string 을 사용합니다.
- C 스타일의 char[]보다 훨씬 편리하고 기능이 많습니다.

```C
#include <iostream>
#include <string>  // string 사용하려면 필요
using namespace std;

int main() {
    string s = "Hello, world!";
    cout << s << endl;
    return 0;
}
```

- 자주 쓰는 string 함수들

|기능|예시 코드|설명|
|:--:|:--:|:--:|
|길이 구하기|s.length() 또는 s.size()|문자열 길이|
|문자 접근|s[0], s.at(1)|인덱스로 문자 접근|
|붙이기|s += "abc";, s.append("xyz");|문자열 추가|
|부분 문자열|s.substr(0, 5)|부분 문자열 추출|
|찾기|s.find("hello")|문자열 찾기 (없으면 string::npos 반환)|
|비교|s1 == s2, s1 < s2|비교 가능 (사전순)|
|삭제|s.erase(3, 2)|3번째부터 2글자 삭제|
|삽입|s.insert(2, "ab")|2번째 위치에 삽입|

4. stringstream
- C++에서 문자열을 공백이나 구분자 기준으로 나누거나, 문자열 ↔ 숫자 간 변환에 매우 자주 쓰입니다. 

```C
#include <iostream>
#include <sstream>
#include <vector>
#include <string>
using namespace std;

int main() {
    string input = "D 2 C U 3 C D 4 C U 2 Z Z";
    stringstream ss(input);
    string token;
    vector<string> result;

    while (ss >> token) {  // 공백 기준 자동 분리
        result.push_back(token);
    }

    for (string s : result)
        cout << s << endl;

    return 0;
}
//출력
// D
// 2
// C
// U
// 3
//...
```

``` C
string s = "123";
int num;
stringstream(s) >> num;
cout << num + 10;  // 출력: 133
```

5. 문자연산은 아스키 연산 결과
```C
char a = 'A'; // ASCII 값 65
char b = 'C'; // ASCII 값 67
int result = b - a; // 67 - 65 = 2
```

6. vector의 인덱스는 음수를 인식 못함
- vector (혹은 배열, 리스트)의 인덱스는 0 이상 정수입니다.
- 음수 인덱스를 사용하면 에러가 발생하거나, 정의되지 않은 동작(Undefined Behavior)을 일으킵니다.
```C
for (int i = 0; i < v.size(); ++i) {
    std::cout << v[i] << " ";
}
```

- 뒤에서부터 접근하고 싶다면: v.size()가 0이면 v.size() - 1이 -1**이 되어, unsigned int 타입으로 암묵 변환되면 아주 큰 값이 되어버릴 수 있습니다. 그러므로 빈 vector 체크가 필요할 수 있습니다:
```C
if (!v.empty()) {
    for (int i = v.size() - 1; i >= 0; --i) {
        // ...
    }
}
```
