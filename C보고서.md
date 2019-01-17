두 정수가 주어지고, 그 수의 합을 반환하는 함수를 만들자.<br>
```cpp
int sum(int a, int b){
    return a + b;
}
```
값을 반환할 때에는 return 값; 형태로 써주면 된다.<br>
```cpp
void hello(){
    printf(“HelloWorld”);
}

int main(){
    hello();
    return 0;
}
```
이 코드는 다음과 같이 실행된다.<br>
<img src = "https://i.imgur.com/TLiAfnR.png" width = "300px">

### 제 3장 배열과 포인터
<b>가. 배열이란?</b><br>
특정 기능에만 쓰이는 int형 변수를 100개 선언하는 것은 비효율적이다. 그래서 ‘배열’이라는 것을 사용한다. 배열은 같은 자료형인 여러 개의 변수를 쉽게 선언할 수 있고, 그 데이터들은 메모리에 연속적으로 할당이 되어 있어 각각의 요소에 쉽게 접근할 수 있다.<br><br>
<b>나. 배열의 선언과 접근</b><br>
 배열 선언은 다음과 같이 한다.<br>
 `자료형 배열_이름[개수];`<br>
만약 int형의 변수 100개를 배열을 이용해 저장하고, 그 배열의 이름을 arr로 한다면 int arr[100]; 과 같이 선언한다.>br?
배열에 있는 각각의 요소는 인덱스로 접근이 가능하다. 인덱스는 0부터 시작하고 (길이-1)번까지 접근 가능하다. 만약 첫 번째 요소의 값을 가져오고 싶다면, arr[0] 으로 가져올 수 있고, 10번째 요소는 arr[9]로 가져올 수 있다.<br><br>
<b>다. 데이터의 주소 값</b><br>
데이터의 주소는 해당 데이터가 저장된 메모리 주소의 첫 번째 바이트를 나타낸다.<br>
<img src = "https://i.imgur.com/YtBvptV.png"><br><br>
<b>라. 포인터</b><br>
C언어에서 포인터는 메모리의 주소 값을 저장하는 변수이다.<br>
변수의 주소는 `&변수` 로 가져온다.<br>
```cpp
int n = 100;
int *ptr = &n;
```
<img src = "https://i.imgur.com/0dX3f0o.png" width = "300px"><br><br>
<b>마. 포인터와 배열의 관계</b><br>
배열의 이름은 배열의 첫 번째 요소(0번 인덱스)의 주소이다. 그리고, 거기에 n을 더하면 n번 인덱스의 주소를 가져올 수 있다.<br>
예를 들어, int arr[100]; 로 배열을 선언했다고 하자. arr 자체는 arr[0]의 주소를 나타낸다. arr+5는 arr[5]의 주소를 나타낸다. 그래서 arr[i]는 `*(arr+i)`처럼 쓸 수 있고, &arr[i]는 `arr+i`로 쓸 수 있다.<br>
참고로, 포인터에 대한 덧셈 연산은 +1 할 때 마다 `sizeof(타입)`만큼 증가한다.<br><br>
<b>바. 배열 포인터와 포인터 배열</b><br>
이름이 혼동될 수 있다. 배열 포인터는 위에서 설명한 것과 같이 배열 이름 자체가 주소 값을 나타내는 것을 말한다. 포인터 배열은 배열의 요소가 포인터인 것을 만한다. int형 배열은 배열의 요소가 int형인 것처럼 포인터 배열은 요소가 포인터이다.

### 제 4장 메모리
<b>가. 메모리 영역의 구성 요소</b><br>
메모리에는 여러 가지 영역이 있다. 그 중, 프로그램이 OS로부터 할당 받는 대표적인 메모리 영역은 크게 4가지 가 있다.<br>
메모리의 낮은 주소(low memory)부터 높은 주소(high memory)까지 차례로<br>
코드(Code) 영역, 데이터(Data) 영역, 스택(Stack) 영역, 힙(Heap) 영역이 존재한다.<br>
<img src = "https://i.imgur.com/4Z17rRQ.png" width = "300px"><br><br>
<b>나. 코드(Code) 영역</b><br>
코드 영역에는 여러 데이터가 들어간다. 대표적인 것 들을 살펴보자면,<br>
우리가 작성한 소스코드가 코드 영역에 저장이 된다. 물론 컴파일 된 기계어 형태로 저장이 된다. 또한, 상수 도 코드 영역에 저장이 된다.<br><br>
<b>다. 데이터(Data) 영역</b><br>
데이터 영역은 프로그램 시작과 함께 메모리에 적재되며, 프로그램 종료 시 메모리에서 해제가 된다. 이러한 특성을 가지고 있는 전역 변수와 정적 변수가 데이터 영역에 저장이 된다. 또한, 구조체도 데이터 영역에 저장이 된다.<br>
참고로, 초기화 되지 않은 데이터는 BSS(Block Stated Symbol) 영역에 저장이 되고, 초기화 된 데이터만 데이터 영역에 저장된다.<br><br>
<b>라. BSS 영역</b><br>
앞에서 서술한 대로 BSS(Block Stated Symbol) 영역에는 초기화가 되지 않은 데이터 들이 저장이 된다.<br>
BSS영역에는 실제 변수가 차치할 공간이 할당되지 않아 그 만큼 binary size가 작아진다.<br><br>
<b>마. 스택(Stack) 영역</b><br>
스택 영역은 함수 호출과 관계가 있는 각종 변수 가 저장되는 영역이다. 함수가 호출될 때 스택에 함수의 매개변수, 호출이 끝난 뒤 돌아갈 반환 주소 값, 해당 함수에서 선언이 된 지역 변수가 저장이 된다. 이렇게 스택 영역에 저장되는 함수의 호출 정보를 스택 프레임이라 한다.<br><br>
```cpp
void f () {
    //something
}

void g () {
    f ();
    //something
}

int main () {
    g ();
    return 0;
}
```
아래 그림은 위 예제 코드에 의한 스택 프레임의 변화이다.<br>
<img src = "https://i.imgur.com/T3Rus7e.png" width = "500px"><br>
<img src = "https://i.imgur.com/PySPb4m.png" width = "500px"><br><br>
<b>바. 힙(Heap) 영역</b><br>
메모리의 힙 영역은 사용자가 직접 관리하는 영역이다. 힙 영역은 사용자가 필요할 때 할당해서 쓰고 다 쓰면 해제된다. 힙 영역은 스택 영역과 다르게 낮은 주소에서 높은 주소로 메모리가 할당된다.

### 제 5장 메모리 동적 할당
c언어에서 메모리를 동적으로 할당할 때에는 malloc 함수를 사용하고, 원형은 다음과 같이 생겼다.<br>
```cpp
#include <stdlib.h>
void *malloc (size_t size);
```
이 함수는 할당 받을 바이트 수를 인수로 받은 뒤, 힙 영역에서 메모리 크기에 맞고, 아직 할당되지 않은 적당한 블록을 찾아서 할당을 한다. 그렇게 찾은 블록의 첫 번째 바이트 주소를 반환한다. 반환 값이 주소 값이기 때문에 해당 공간에 직접 접근할 때에는 포인터를 사용해야 한다.

### 제 6장 구조체
<b>가. 구조체란?</b><br>
구조체는 여러 가지의 자료형을 묶어 새로운 자료형을 만드는 것이다. 만약 점의 x, y좌표를 저장하고 싶다면 int/float/double같은 형태의 변수 2개 대신에 그 것들을 구조체로 묶어 하나의 자료형으로 만들 수 있다.<br><br>
<b>나. 구조체의 선언과 사용</b><br>
구조체는 struct키워드로 선언한다. 선언문의 형태는 다음과 같다.<br>
```cpp
struct 구조체_이름{
    자료형 멤버이름;
    …
}

```
위에서 들었던 예시처럼 점의 좌표를 저장하는 구조체를 만들어보자.<br>
```cpp
struct DOT{
    double x, y;
}

```
이런 식으로 구조체를 선언한다.<br>
점의 정보를 저장하는 변수를 만들고, 그 점의 x, y좌표 값에 접근해보자.<br>
```cpp
struct DOT p; //구조체 변수 선언
p.x = 5; //x좌표 지정
p.y = 6; //y좌표 지정

```
이런 식으로 값에 접근이 가능하다.

### 제 7장 열거형
<b>가. 열거형이란?</b><br>
열거형은 정수형 상수에 이름을 붙여준다. 만약 다음과 같이 상수 여러 개를 선언하면 귀찮아진다.<br>
```cpp
const int val1 = 1;
const int val2 = 2;
const int val3 = 3;
…
```
이럴 때 열거형을 사용하면 조금 더 편하게 선언할 수 있다.<br><br>
<b>나. 열거형의 선언</b><br>
열거형은 다음과 같이 정의한다.<br>
```cpp
enum 열거형_이름{
    값1 = 초기화,
    값2,
    값3
    …
};
```
열거형은 선언만 하면 되는게 아니라 따로 변수도 선언해주어야 한다.<br>
`enum 열거형_이름 변수이름;` 형태로 선언을 할 수 있다.<br><br>
<b>다. 열거형의 활용</b><br>
열거형으로 요일을 정의해보자.<br>
```cpp
#include <stdio.h>

enum DayOfWeek{
   sun = 0,
   mon,
   tue, wed, thu, fri, sat
};

int main(){
   enum DayOfWeek week;
   week = tue;
   printf(“%d”, week);
   return 0;
}
```
일단 이 코드는 2를 출력한다.<br>
열거형은 첫 값만 할당을 해주면 그 아래에 있는 값들은 각각 1씩 증가하면서 자동으로 할당된다. 따라서 sun에 0을 할당해주면 그 다음에 있는 mon, tue, wed … 는 각각 1, 2, 3 …이 할당된다.

### 제 8장 파일 입출력
<b>가. 파일 열기</b><br>
파일을 열고 닫는 함수는 다음 2가지다.<br>
```cpp
fopen(파일 경로, 열기 방식);
fclose(stream이름);
```
위 함수를 쓰기 위해서는 FILE* 형 변수가 필요하다.<br>
```cpp
FILE *이름 //파일이 열릴 포인터 이름( = stream 이름)
```
 a.txt를 열고자 한다면<br>
 ```cpp
 FILE *f = fopen(“a.txt”, “r”);
 //something
 fclose(f);
 ```
파일 경로는 만약 같은 폴더라면 파일 이름만, 아니라면 상대주소 혹은 절대주소로 표현한다.<br><br>
<b>나. 파일 열기 방식</b><br>
<table>
<tr> <th>모드</th> <th>기능</th> <th>파일 없음</th> <th>파일 있음</th> <th>기존 파일 보호</th> </tr>
<tr> <th>r</th> <td>읽기</td> <td>에러</td> <td>기존 파일 이용</td> <td>에러</td> </tr>
<tr> <th>r+</th> <td>읽기/쓰기</td> <td>에러</td> <td>기존 파일 이용</td> <td>덮어쓰기</td> </tr>
<tr> <th>w</th> <td>쓰기</td> <td>새로 생성</td> <td>새로 생성</td> <td>덮어쓰기</td> </tr>
<tr> <th>w+</th> <td>쓰기/읽기</td> <td>새로 생성</td> <td>새로 생성</td> <td>덮어쓰기</td> </tr>
<tr> <th>a</th> <td>이어쓰기</td> <td>새로 생성</td> <td>기존 파일 이용</td> <td>이어쓰기</td> </tr>
<tr> <th>a+</th> <td>이어쓰기/읽기</td> <td>새로 생성</td> <td>기존 파일 이용</td> <td>이어쓰기</td> </tr>
</table>

<br><br>
<b>다. 파일 읽어오기</b><br>
파일을 읽어오는 함수 fscanf, fgets 2가지를 소개하고자 한다.<br>
두 함수는 다음과 같이 사용한다.<br>
```cpp
fscanf(stream이름, 읽어올 자료 유형, 읽어들일 위치);
fgets(읽어들일 위치, 읽어올 문자열 크기, stream이름);
```
fscanf의 첫 번째 인자는 stream의 이름을 적어주고, 나머지 인자는 표준 입출력의 scanf함수와 똑같다.<br>
fgets는 첫 인자에 내용을 저장할 문자열을 넣고, 두 번째 인자는 읽어올 문자열의 길이, 세 번째 인자에 stream의 이름을 넣어주면 된다.<br><br>
<b>라. 파일 쓰기</b><br>
파일을 읽어오는 함수 fprintf, fputs를 소개하겠다.<br>
두 함수는 다음과 같이 사용한다.<br>
```cpp
fprintf(쓸 위치, 쓸 내용, 서식 문자);
fputs(쓸 내용, 쓸 stream이름);
```
fprintf는 첫 번째 인자에 stream이름이 들어가고, 나머지 인자는 printf함수와 똑같다.<br>
Fputs는 첫 번째 인자에 내용을 적고, 두 번째 인자의 stream이름을 넣는다.

<hr>

참고 문헌<br>
* https://ko.wikipedia.org/wiki/%EB%B6%80%EB%8F%99%EC%86%8C%EC%88%98%EC%A0%90
* https://dojang.io/mod/page/view.php?id=480
* http://desire-with-passion.tistory.com/169
* http://tcpschool.com/c/c_pointer_intro
* http://tcpschool.com/c/c_memory_structure
* http://tcpschool.com/c/c_memory_stackframe
* https://blog.perfectacle.com/2017/01/25/c-ref-003/
* http://sfixer.tistory.com/entry/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%98%81%EC%97%ADcode-data-stack-heap
* https://kldp.org/node/122255
