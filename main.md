### 저자
* 선린인터넷고등학교 소프트웨어과, Unifox 10기 나정휘

<hr>

### 제1장. C++과 표준 라이브러리
##### 가. C ++에 추가된 각종 구문에 대한 간략한 개요
<b>A. I/Ostream</b><br>
I/Ostream에 있는 출력의 기본적인 내용은 매우 간단하다. 출력 스트림(output stream / ostream)은 데이터들에 대한 통로와 같다. 어떤 것이든 통로에 넣어주면 적절히 출력이 된다. std::cout은 사용자 콘솔 혹은 표준 출력에 해당하는 통로다. std::cout말고도 여러가지 다른 통로가 있다. 예를 들어 std::cerr은 오류 콘솔에 출력을 한다. &lt;&lt; 연산자가 테이터를 통로에 넣어주는 역할을 한다. 출력 스트림을 사용하면 다양한 유형의 여러 데이터를 한 줄의 코드에서 연속적으로 통로로 보낼 수 있다. 다음 코드는 문자열을 출력한 뒤, 숫자 데이터와 다른 문자열을 출력하는 코드이다.<br>
```cpp
std::cout << "HelloWorld" << 1234 << "Other String" << std::endl;
```
std::endl은 줄의 끝(end-of-line)을 나타낸다. 출력 스트림이 std::endl을 만나게 되면, 통로에 들어있는 모든 데이터를 비운 뒤 다음 줄로 넘어간다. 다음 줄로 넘어가는 것을 나타낼 수 있는 또 다른 방법은 \n 문자를 쓰는 것이다. \n 문자는 이스케이프 문자(escape character)이며, 줄 바꿈을 나타내는 문자이다. 다음 표는 가장 대중적인 몇 가지의 이스케이프 문자를 정리해 놓은 표이다.<br>
<table>
  <tr> <th>\n</th> <td>줄 바꿈 (new line)</td> </tr>
  <tr> <th>\r</th> <td>캐리지 리턴 (carriage return)</td> </tr>
  <tr> <th>\t</th> <td>탭 (tab)</td> </tr>
  <tr> <th>\\</th> <td>역 슬래시 (backslash character)</td> </tr>
  <tr> <th>\\"</th> <td>큰 따옴표 (quotation mark)</td> </tr>
</table>

스트림은 사용자로부터 입력을 받는 데에도 쓰인다. 가장 간단한 방법은 &gt;&gt; 연산자를 사용해 입력 스트림(input stream/istream)을 사용하는 것이다. std::cin은 키보드를 이용해 사용자로부터 입력을 받는다.<br><br>
<b>B. namespace</b><br>
namespace는 서로 다른 코드 조각 간의 이름 충돌 문제를 해결해준다. 예를 들어, 프로그래머가 foo()라는 함수를 포함한 코드를 작성했다고 가정해보자. 어느 날, 프로그래머가 foo()라는 이름의 함수를 갖고 있는 라이브러리를 사용하게 된다면, 이름 충돌 문제가 발생한다. 라이브러리의 함수 이름을 변경할 수 없다면 큰 어려움을 겪을 것이다. 이름이 정의된 컨텍스트를 정의할 수 있기 때문에 namespace가 이러한 문제를 해결해 줄 수 있다. namespace에 코드를 넣으려면 namespace 블록 안에 코드를 넣으면 된다. 다음 코드가 aa.h에 있다고 가정하자.<br>
```cpp
namespace asdf {
    void foo ();
}
```
asdf라는 namespace에 foo() 함수를 넣으면 다른 라이브러리에서 제공하는 foo()함수와 분리된다. namespace에 있는 foo()함수를 호출하기 위해서는 스코프 설정 연산자라고 불리는 :: 연산자를 사용하여 namespace를 함수 이름 앞에 추가해서 쓴다. <br>
```cpp
asdf::foo(); //asdf에 있는 foo함수 호출
```
asdf 라는 namespace에 속하는 코드를 namespace를 명시적으로 앞에 추가하지 않아도 동일한 namespace 내의 다른 코드를 호출할 수 있다. 이러한 암시적 namespace는 코드를 더 읽기 쉽게 만드는데 유용하다. using 지시문을 사용하여 namespace를 미리 지정할 수 있다. 이 지시문은 뒤에 나올 코드가 지정된 namespace의 이름을 사용하고 있다는 것을 컴파일러에 알린다.<br>
```cpp
#include <iostream>
#include "aa.h"
using namespace asdf;

int main () {
    foo ();
}
```
단일 소스 파일에 여러 개의 using 지시문을 포함할 수는 있지만, 이 방법을 남용하지 않도록 조심해야 한다. 동일한 이름을 포함하는 두 개의 namespace를 사용하는 경우에는 이름 충돌이 다시 발생하게 된다. 실수로 잘못된 namespace에 있는 함수를 호출하지 않도록 작동 중인 namespace를 기억해야 한다.<br>
또한, 출력 스트림 예제로 썼던 코드에 있는 cout과 endl은 사실 std라는 namespace에 정의되어 있다. HelloWorld를 출력하는 코드는 다음과 같이 쓸 수 있기도 하다.<br>
```cpp
#include <iostream>
using namespace std;

int main () {
    cout << "HelloWorld" << endl;
}
```
using 지시문은 namespace내의 특정 항목을 참조하는 데에도 사용 가능하다. 예를 들어 사용하려는 std namespace의 유일한 부분이 cout인 경우에는 다음과 같이 참조할 수 있다.<br>
```cpp
using namespace std::cout;
```
다음 코드는 namespace를 추가하지 않고도 cout을 사용할 수 있지만, std namespace에 들어있는 다른 항목을 사용할 때에는 명시를 해줘야 한다.<br>
```cpp
using namespace std::cout;
cout << "HelloWorld" << std::endl;
```
<b>D. alternative function syntax (새로운 함수 정의 문법)</b><br>
C++은 여전히 C에서 설계된 함수 구문을 이용한다. 그동안 C++에는 많은 새로운 기능이 추가되어 이전 함수 구문에 많은 문제점이 노출되었다. C++ 11부터는 새로운 함수 정의 문법이 지원된다. 이 새로운 문법은 일반 함수에서는 그리 유용하지 않지만, 템플릿 함수의 반환 타입을 지정하는 데에는 매우 유용하게 쓰인다.<br>
다음 예제는 새로운 함수 정의 문법을 보여준다. 이 예제에서의 auto 키워드는 새로운 함수 정의 문법을 사용하여 함수 프로토타입을 시작한다는 의미이다.<br>
```cpp
auto func (int i) -> int
{
    return i + 2;
}
```
함수의 반환 타입은 더 이상 시작 부분이 아닌 화살표 뒤에 배치된다. 다음 코드는 새로운 함수 정의 문법으로 선언된 함수의 호출이 이전과 동일하며, main함수도 해당 문법을 사용할 수 있음을 보여준다.<br>
```cpp
using namespace std;

auto main () -> int
{
 cout << func (3) << endl;
 return 0;
}
```

##### 나. 스마트 포인터의 기초
메모리 문제를 방지하기 위해서는 일반적인 C-스타일 포인터보다는 스마트 포인터를 사용해야 한다. 스마트 포인터는 함수 종료와 같은 스코프 탈출 시 자동으로 메모리 할당을 해제한다.<br>
C++에는 std::unique_ptr, std::shared_ptr, std::weak_ptr 총 세 가지의 스마트 포인터가 있으며 모두 <memory>헤더에 정의되어 있다. unique_ptr는 일반 포인터와 유사하지만, unique_ptr가 스코프를 벗어나거나 삭제될 때 자동으로 메모리 혹은 리소스를 해제한다. unique_ptr는 가리키고 있는 객체의 단독 소유권을 갖는다. unique_ptr에 C-스타일 배열을 저장할 수도 있다. unique_ptr를 만들려면 std::make_unique<> 를 사용하면 된다.<br>
 예를 들어 `Employee* anEmployee = new Employee;` 대신 `auto anEmployee = std::make_unique<Employee> ();` 와 같이 쓸 수 있다.<br>
make_unique는 C++14부터 사용할 수 있다. 컴파일러가 C++14를 지원하지 않는다면 unique_ptr를 다음과 같이 만들 수 있다.<br>
```cpp
std::unique_ptr<Employee> anEmployee (new Employee);
```
shared_ptr는 데이터의 분산된 소유를 허용한다. shared_ptr가 할당될 때마다 참조 카운트가 증가되어 데이터의 소유자가 하나 더 있음을 나타낸다. shared_ptr가 범위를 벗어나면 참조 카운트가 감소한다. 참조 카운트가 0이 되면 데이터의 소유자가 더 이상 없다는 것을 의미하여 포인터에 의해 참조되는 데이터는 메모리에서 해제된다. shared_ptr에는 배열을 저장할 수 없다. shared_ptr를 만들려면 std::make_shared<> 를 사용하면 된다. 다만, weak_ptr를 사용하여 shared_ptr의 참조 카운트를 변화시키지 않으면서 shared_ptr를 관찰할 수 있다.

### 제2장. C++ 문자열
<b>가. C++ std::string 클래스에 대한 세부 정보</b><br>
C++은 표준 라이브러리의 일부로 문자열을 훨씬 향상된 방식으로 구현한다. C++에서 std::string은 cstring헤더(C언어의 string.h)에 있는 함수와 동일한 많은 기능을 지원하고, 사용자를 위해 메모리 할당을 관리해주는 클래스이다. string클래스는 std namespace의 string헤더에 정의되어 있다.<br>
C++ 문자열 클래스의 필요성을 이해하려면 C-스타일 문자열의 장단점을 고려해야 한다.<br>
* 장점
  * 기본적인 char타입 및 배열 구조를 이용해서 간단하다.
  * 가볍고, 제대로 사용하면 필요한 만큼의 메모리만 차지한다.
  * low-level이기 때문에 원시 메모리처럼 쉽게 조작하고 복사할 수 있다.
  * C 프로그래머가 쉽게 이해할 수 있다.
* 단점
  * 일급 객체를 조작하기 위해서 많은 노력이 필요하다.
  * 메모리 버그에 취약하다.
  * C++의 객체 지향 특성을 이용하지 않는다.

C++ string은 클래스임에도 불구하고 거의 기본적인 타입인 것처럼 취급할 수 있다. 사실, 단순한 타입이라고 생각할수록 좋다. 연산자 오버로딩을 통해 C++ 문자열은 C-스타일 문자열보다 훨씬 쉽게 사용할 수 있다. 예를 들어, + 연산자는 두 문자열을 연결해주는 연산으로 다시 정의된다. <br>
```cpp
string A = "12";
string B = "34";
string C;
C = A + B;
```
이 코드에서 C의 값은 "1234"가 된다.<br>
또한, += 연산자도 오버로딩이 되어있어 문자열을 더 쉽게 더할 수 있게 해준다.<br>
```cpp
string A = "12";
string B = "34";
A += B;
```
이 코드에서는 A의 값이 "1234"가 된다.<br>
C-스타일 문자열의 또 다른 문제는 == 연산자를 이용하여 비교를 할 수 없다는 것이다. 다음 코드를 보자.<br>
```cpp
char* a = "12";
char b [] = "12";
return (a == b);
```
위와 같이 비교를 하면 항상 false가 나온다. 위 코드에서 비교하는 대상은 문자열의 내용이 아닌 두 포인터(주소 값)을 비교한다. C-스타일 문자열을 비교하기 위해서는 다음과 같이 코드를 써야 한다.<br>
```cpp
char* a = "12";
char b [] = "12";
return !strcmp(a, b);
```
또한, C-스타일 문자열은 <, >, <=, >= 연산자를 이용해 비교를 못하기 때문에 strcmp()함수에서 -1, 0, 1을 반환하는 것을 이용해 두 문자열의 사전 순 관계를 비교한다. C++ 문자열에서는 ==, !=, < 등과 같은 연산자가 오버로딩 되어 있어 문자열을 비교할 수 있다. 각각의 문자들은 [] 연산자를 사용해 접근할 수도 있다.<br>
다음 코드에서 볼 수 있듯이 문자열 확장(연결)으로 인해 메모리를 추가로 요구할 때 string 클래스에서 자동으로 처리해주기 때문에 메모리 오버런(memory overrun)은 더 이상 신경 쓸 필요가 없다.<br>
```cpp
string myString = "hello";
myString += ", there";
string myOtherString = myString;
if (myString == myOtherString) {
 myOtherString [0] = 'H';
}
cout << myString << endl; //hello, there
cout << myOtherString << endl; //Hello, there
```
위 코드에서 몇 가지 유의해야 할 사항이 있다. 첫 번째로, 문자열이 할당되고 크기가 조정이 되더라도 메모리 누수가 없다는 점이다. 또 다른 점은, 연산자들이 프로그래머가 원하는 대로 작동을 한다는 점이다. 예를 들어, = 연산자는 프로그래머가 원하는 대로 문자열을 복사한다. 만약 프로그래머가 배열 기반 문자열을 사용해왔다면, 이 기능이 그 프로그래머를 더 편하게 해 줄 것이다.<br><br>
<b>나. 원시 문자열 리터럴이란</b><br>
원시 문자열 리터럴(raw string literals)은 개 행 문자, 큰 따옴표 등이 포함되어 있는 문자열을 \n이나 \” 와 같은 이스케이프 문자를 사용하지 않고 일반 텍스트로 처리할 수 있는 문자열 리터럴이다. 이스케이프 문자는 1장에 나와있다. 예를 들어, 일반적인 문자열 리터럴을 이용해 다음과 같이 작성하면 이스케이프 되지 않은 큰 따옴표가 포함 되어있기 때문에 컴파일 에러가 발생한다.<br>
```cpp
string str = "Hello "World"!"; //Error
```
일반 문자열 리터럴에서 큰 따옴표를 포함하기 위해서는 다음과 같이 해야 한다.<br>
```cpp
string str = "Hello \"World\"!";
```
원시 문자열 리터럴을 이용하면 따옴표를 이스케이프 처리하지 않고 표현이 가능하다. 원시 문자열 리터럴은 R”( 로 시작해 )” 로 끝난다.<br>
```cpp
string str = R"(Hello "World"!)";
```
원시 문자열은 여러 줄을 차지할 수도 있다. 예를 들어, 일반 문자열 리터럴을 이용해 다음과 같이 작성하면 일반 문자열 리터럴은 여러 줄에 걸쳐 존재할 수 없기 때문에 컴파일 에러가 발생한다.<br>
```cpp
string str = "Line 1
Line 2 with \t"; //Error
```
하지만, 원시 문자열을 이용하면 다음과 같이 작성이 가능하다.<br>
```cpp
string str = R"(Line 1
Line 2 with \t)";
```
또한, 원시 문자열 리터럴을 사용하면 \t문자가 실제 탭(tab)문자로 바뀌지 않고 그대로 저장이 된다. 그러므로 str을 콘솔에 출력하면 다음과 같이 나온다.<br>
```cpp
Line 1
Line 2 with \t
```
원시 문자열 리터럴은 )” 로 끝나기 때문에 문자열 중간에 )” 를 포함할 수 없다. 예를 들어, 다음 코드는 문자열 중간에 )” 가 포함되어 있기 때문에 유효하지 않다.<br>
```cpp
string str = R"(The characters )" are embedded in this string)"; //Error
```
)” 가 포함된 문자열이 필요하다면, 다음과 같은 확장된 원시 문자열 리터럴이 필요하다.<br>
```cpp
R"d-char-sequence(r-char-sequence)d-char-sequence"
```
r-char-sequence는 실제 원시 문자열이다. d-char-sequence는 원시 문자열 리터럴의 시작과 끝 부분에서 동일해야 하는 선택적 구분 기호 시퀀스이다. 이 구분 기호 시퀀스는 최대 16자까지 사용 가능하다. 이 구분 기호 시퀀스는 원시 문자열 리터럴에서 나타나지 않는 시퀀스로 선택해야 한다. 위에 있는 예제는 구분 기호 시퀀스를 이용하여 다시 작성할 수 있다.<br>
```cpp
string str = R"-(The characters )" are embedded in this string)-";
```

### 제3장. 클래스와 객체의 사용
<b>가. 메소드와 멤버 데이터로 클래스 작성</b><br>
클래스에는 여러 멤버가 있을 수 있다. 멤버에는 멤버 함수(즉, 메소드, 생성자 또는 소멸자)와 데이터 멤버 라고도 하는 멤버 변수가 있다. 다음 두 줄의 코드는 각각 멤버 함수를 선언하는 방법을 보여준다.<br>
```cpp
void setValue (double inValue);
double getValue () const;
```
그리고 다음 코드는 멤버 변수를 선언하는 방법이다.<br>
```cpp
double mValue;
```
클래스는 멤버 함수와 멤버 변수들을 정의한다. 클래스는 특정 인스턴스에만 적용된다. 이 규칙의 유일한 예외는 정적(static) 멤버이다. 클래스는 개념을 정의한다. 따라서 위 코드처럼 mValue라는 멤버 변수가 선언된 클래스의 각 개체들은 mValue에 대한 고유한 값을 갖고 있다. 멤버 함수의 구현은 모든 개체에서 공유된다. 클래스에는 원하는 수의 멤버 함수와 멤버 변수를 포함할 수 있다. 멤버 변수는 멤버 함수와 동일한 이름을 갖지 못한다.<br><br>
<b>나. 메소드와 멤버 데이터 접근 제어</b><br>
클래스의 모든 멤버들은 public, private, protected 세 가지 접근 지정자(액세스 지정자) 중 하나의 대상이 된다. 접근 지정자는 다음 접근 지정자가 나오기 전까지 모든 멤버 선언에 적용된다. 디폴트(기본) 접근 지정자는 private이며, 첫 번째 접근 지정자가 나오기 전까지 지속된다. 다음 코드의 SpreadsheetCell 클래스에서 setValue와 mValue의 접근 지정자는 private이며, getValue의 접근 지정자는 public이다.<br>
```cpp
class SpreadsheetCell {
    void setValue (double inValue); //private access
    public:
        double getValue () const;
    private:
        double mValue;
};
```
C++에서는 구조체도 클래스처럼 멤버 함수(메소드)를 가질 수 있다. 유일한 차이점은 구조체의 기본 접근 지정자는 public이고, 클래스의 기본 접근 지정자는 private이라는 점이다. 그러므로 SpreadsheetCell 클래스는 구조체를 이용해 다음과 같이 작성할 수도 있다.<br>
```cpp
struct SpreadsheetCell {
    double getValue () const; // public access
    private:
        void setValue (double inValue);
        double mValue;
};
```
다음 표에는 세 가지 접근 지정자의 의미가 나와있다.<br>
<table>
  <tr> <th>접근 지정자</th> <th>의미</th> <th>용도</th> </tr>
  <tr> <th>public</th> <td>어디서든 public 멤버에 접근 가능</td> <td>클라이언트에서 사용할 동작,
private혹은 protected 멤버에 대한 접근
</td> </tr>
  <tr> <th>private</th> <td>클래스의 멤버 함수만 private 멤버에 접근 가능, 파생 클래스의 멤버 함수에서는 접근 불가</td> <td>모든 것, 특히 멤버 변수는 기본적으로 private여야 한다.<br>파생 클래스에서의 접근을 허용할 때에는 protected인 setter와 getter를 제공하고, public인 setter와 getter를 제공해 클라이언트에서 접근하게 할 수 있다.
</td> </tr>
  <tr> <th>protected</th> <td>해당 클래스와 파생 클래스의 멤버 함수에서만 protected 멤버에 접근 가능</td> <td>클라이언트에서 사용하지 않기를 원하는 helper메소드</td> </tr>
</table>

<b>다. 메소드 정의</b><br>
앞에서 정의한 SpreadsheetCell클래스는 클래스의 객체를 만드는 데에는 충분하다. 하지만 setValue나 getValue메소드를 호출하려고 하면 Linker가 해당 메소드가 정의되지 않았다고 한다. 그 이유는 클래스 정의가 메소드의 프로토타입(prototype)을 지정하지만 구현을 정의하지는 않기 때문이다. 일반적인 함수의 프로토타입과 구현을 모두 정의하는 것과 같이 메소드도 프로토타입과 구현을 정의해주어야 한다. 클래스의 정의는 메소드의 정의보다 앞에 와야 한다. 일반적으로 클래스의 정의 부분은 헤더 파일(.h 파일)에 들어가고, 메소드의 정의는 해당 헤더를 포함하는 소스 파일(.cpp 파일)에 들어간다. 다음은 SpreadsheetCell 클래스에 있는 두 가지 메소드의 정의 방법이다.<br>
```cpp
#include "SpreadsheetCell.h"

void SpreadsheetCell::setValue (double inValue) {
    mValue = inValue;
}
double SpreadsheetCell::getValue () const {
    return mValue;
}
```
클래스 이름 뒤에 두 개의 콜론(:)이 각 메소드 앞에 온다.<br>
```cpp
void SpreadsheetCell::setValue (double inValue)
```
이 구문은 컴파일러에 setValue메소드의 정의는 SpreadsheetCell클래스의 일부라고 알려준다.<br>
참고로, 메소드를 정의할 때에는 접근 지정자를 반복하지 않는다.<br><br>
<b>라. 다른 메소드 호출</b><br>
클래스의 메소드를 다른 메소드 내부에서 호출할 수 있다. SpreadsheetCell클래스를 예시로 들어보자. 실제 스프레드 시트 응용 프로그램은 셀에 텍스트와 숫자를 허용한다. 사용자가 텍스트 셀을 숫자로 해석하려고 하면 스프레드 시트에서 텍스트 데이터를 숫자로 변환하려고 한다. 만약 텍스트가 유효한 숫자를 나타내지 않는다면 셀 값이 무시된다. 이 프로그램에서는 숫자가 아닌 문자열을 0을 반환한다. 다음 코드는 텍스트 데이터를 지원하는 SpreadsheetCell 클래스 정의를 위한 첫 번째 단계이다.<br>
```cpp
#include <string>
class SpreadsheetCell {
    public:
        void setValue (double inValue);
        double getValue () const;
        void setString (const std::string& inString);
        const std::string& getString () const;
    private:
        std::string doubleToString (double inValue) const;
        double stringToDouble (const std::string& inString) const;
        double mValue;
        std::string mString;
};
```
이 클래스는 텍스트와 숫자 데이터를 모두 저장한다. 클라이언트가 데이터를 문자열로 생성하면 데이터는 double형(실수형)으로 변환되고 double형은 문자열로 변환이 된다. 텍스트가 유효한 숫자가 아니라면 double값은 0이 된다. 이 클래스의 정의는 셀의 텍스트 표현을 설정하고 double형과 문자열을 서로 변환해주는 메소드를 보여준다. 이 메소드는 문자열 스트림(string stream)이라는 것을 사용한다. 다음 코드는 모든 메소드의 구현이다.<br>
```cpp
#include "SpreadsheetCell.h"
#include <iostream>
#include <sstream>
using namespace std;

void SpreadsheetCell::setValue (double inValue) {
    mValue = inValue;
	mString = doubleToString(mValue);
}

double SpreadsheetCell::getValue () const {
	return mValue;
}

void SpreadsheetCell::setString (const string& inString) {
	mString = inString;
	mValue = stringToDouble(mString);
}

const string& SpreadsheetCell::getString () const {
	return mString;
}

string SpreadsheetCell::doubleToString (double inValue) const {
	ostringstream ostr;
	ostr << inValue;
	return ostr.str ();
}

double SpreadsheetCell::stringToDouble (const string& inString) const {
	double temp;
	istringstream istr(inString);
	istr >> temp;
	if (istr.fail() || !istr.eof()) {
	return 0;
	}
	return temp;
}
```
<b>마. 힙과 스택에 있는 객체 사용</b><br>
다음 코드는 스택에 SpreadsheetCell 개체를 생성하고 사용하는 코드이다.<br>
```cpp
SpreadsheetCell myCell, anotherCell;
myCell.setValue(6);
anotherCell.setString("3.2");
cout << "cell 1: " << myCell.getValue() << endl;
cout << "cell 2: " << anotherCell.getValue() << endl;
```
변수 자료형이 클래스 이름이라는 점을 제외하고는 단순 변수를 선언한 것처럼 개체를 생성한다. myCell.setValue(6)에서 나오는 . 을 도트(Dot, 점)연산자라고 부른다. 이 연산자를 통해 개체에 대한 메소드를 호출할 수 있다. 개체에 public 멤버 변수가 있다면 도트 연산자를 이용해 해당 멤버 변수에 접근할 수도 있다. 하지만 public 멤버 변수는 권장하지 않는다.<br>
이 프로그램의 출력은 다음과 같다.<br>
```cpp
cell 1: 6
cell 2: 3.2
```
또한, new 키워드를 사용하여 개체를 동적으로 할당할 수도 있다.<br>
```cpp
SpreadsheetCell* myCellp = new SpreadsheetCell ();
myCellp->setValue (3.7);
cout << "cell 1: " << myCellp->getValue () << " " << myCellp->getString () << endl;
delete myCellp;
myCellp = nullptr;
```
힙에 개체를 생성하면, 화살표 연산자(->)를 통해 해당 개체의 멤버에 접근할 수 있다. 화살표 연산자는 참조( * )연산자와 멤버 접근( . )연산자를 결합한 것이다. 이 두 연산자를 대신 사용할 수도 있겠지만, 귀찮을 것이다.<br>
```cpp
cout << "cell 1: " << (* myCellp).getValue() <<  " " << (* myCellp).getString() << endl;
```
메모리 문제의 발생을 방지하기 위해서 다음과 같이 스마트 포인터를 사용하는 것이 좋다.<br>
```cpp
auto myCellp = make_unique<SpreadsheetCell>();
myCellp->setValue (3.7);
cout << "cell 1: " << myCellp->getValue () << " " << myCellp->getString () << endl
```

<b>바. 객체의 생명 주기</b><br>
객체의 생명주기는 생성, 할당, 소멸 세 가지의 활동을 포함한다. 객체를 생성, 할당, 제거하는 방법과 시기, 그리고 이러한 동작을 사용자가 지정하는 방법을 이해하는 것이 중요하다. 객체는<br>
1. 객체가 스택에 있다고 선언된 시점이나
2. new, new[ ], 혹은 스마트 포인터를 사용하여 공간을 명시적으로 할당할 때

생성된다. 객체가 생성이 되면 해당 객체가 포함하고 있는 다른 모든 객체도 함께 생성된다. 예시로 다음 코드를 보자.<br>
```cpp
#include <string>

class MyClass {
    private:
        std::string mName;
};
int main () {
    MyClass obj;
    return 0;
}
```
MyClass에 포함된 string객체는 main함수 안에서 obj가 생성된 시점에 생성이 되고, 자신을 포함한 객체, 즉 obj가 소멸될 때 같이 없어진다.<br><br>
<b>사. 객체의 생성자</b><br>
생성자 메소드의 이름은 클래스 이름과 동일하고 반환 값이 없으며 매개 변수는 있거나 없을 수 있다. 매개 변수가 없는 생성자를 기본 생성자(default constructor)라고 한다. 특정 상황에서는 기본 생성자를 제공해야 하는 경우가 생기며, 제공하지 않는 경우 컴파일 에러가 발생한다. 다음은 SpreadsheetCell클래스에 생성자를 추가하는 첫 번째 시도이다.<br>
```cpp
class SpreadsheetCell {
    public:
        SpreadsheetCell (double initialValue);
};
```
일반적인 메소드처럼 구현도 제공해야 한다. 생성자의 경우, 다음과 같이 구현을 작성한다.<br>
```cpp
SpreadsheetCell::SpreadsheetCell (double initialValue) {
    setValue (initialValue);
}
```
SpreadsheetCell생성자는 SpreadsheetCell클래스의 멤버이므로 생성자 이름 전에 SpreadsheetCell::을 붙여야 한다. 위 생성자는 setValue를 호출하여 숫자 및 텍스트 표현을 설정한다.<br>
생성자를 사용하면 개체가 생성되고 해당 값이 초기화된다. 스택 기반 할당과 힙 기반 할당 모두 생성자를 사용할 수 있다.<br>
<b>스택</b>에 SpreadsheetCell객체를 할당할 때에는 다음과 같은 생성자를 사용한다.<br>
```cpp
SpreadsheetCell myCell (5), anotherCell (4);
cout << "cell 1: " << myCell.getValue() << endl;
cout << "cell 2: " << anotherCell.getValue() << endl;
```
다만, SpreadsheetCell생성자를 명시적으로 호출하면 안된다. 예를 들어, 다음과 같은 코드를 사용하면 안된다.<br>
```cpp
SpreadsheetCell myCell.SpreadsheetCell(5);
```
마찬가지로, 생성자를 나중에 호출할 수도 없다. 다음 코드도 잘못된 코드이다.<br>
```cpp
SpreadsheetCell myCell;
myCell.SpreadsheetCell(5);
```
<b>힙</b>에 SpreadsheetCell객체를 동적으로 할당할 때에는 다음과 같은 생성자를 사용한다.<br>
```cpp
auto smartCellp = make_unique<SpreadsheetCell> (4);
//다른 작업
//스마트 포인터를 이용하면 소멸시킬 필요가 없다.

//혹은 다음과 같이 사용할 수도 있지만 권장하지 않는다.
SpreadsheetCell* myCellp = new SpreadsheetCell (5);
SpreadsheetCell* anotherCellp = nullptr;
anotherCellp = new SpreadsheetCell (4);
//다른 작업
delete myCellp; myCellp = nullptr;
delete anotherCellp; anotherCellp = nullptr;
```
스마트 포인터를 사용하지 않고 객체를 동적으로 할당한 경우에는 삭제를 해야 하는 것을 기억하자.<br>
클래스에 둘 이상의 생성자를 제공할 수 있다. 모든 생성자는 동일한 이름을 갖지만 서로 다른 개수의 매개 변수 혹은 매개 변수 타입을 사용해야 한다. C++에서 이름이 같은 함수가 두 개 이상이 경우 컴파일러는 호출 구문의 유형과 일치하는 매개 변수 유형의 함수를 선택한다. 이를 메소드 오버로딩이라 부르고, 뒤에 자세한 설명이 나온다. SpreadsheetCell클래스에는 두 개의 생성자를 사용하는 것이 좋다. 하나는 초기 값을 double값으로 취하고, 다른 하나는 초기 값을 문자열로 취하는 것이 좋다. 새로운 클래스의 생성자 정의 부분은 다음과 같다.<br>
```cpp
class SpreadsheetCell {
    public:
        SpreadsheetCell (double initialValue);
        SpreadsheetCell (const std::string& initialValue);
};
```
다음은 두 번째 생성자의 구현 부분이다.<br>
```cpp
SpreadsheetCell::SpreadsheetCell (const string& initialValue) {
    setString(initialValue);
}
```
다음은 서로 다른 두 개의 생성자를 사용하는 코드이다.<br>
```cpp
SpreadsheetCell aThirdCell("test"); //문자열을 이용한 생성자
SpreadsheetCell aFourthCell(4.4); //double을 이용한 생성자
auto aThirdCellp = make_unique<SpreadsheetCell>("4.4"); //문자열을 이용한 생성자
cout << "aThirdCell: " << aThirdCell.getValue() << endl;
cout << "aFourthCell: " << aFourthCell.getValue() << endl;
cout << "aThirdCellp: " << aThirdCellp->getValue () << endl;
```
생성자가 여러 개인 경우, 하나의 생성자를 다른 생성자를 이용해 구현하려고 할 수도 있다. 예를 들어, 몇몇은 다음과 같이 문자열 생성자에서 double생성자를 호출하려 할 것이다.<br>
```cpp
SpreadsheetCell::SpreadsheetCell (const string& initialValue) {
    SpreadsheetCell(stringToDouble(initialValue));
}
```
말이 되는 것처럼 보인다. 이 코드는 컴파일, 링크, 실행 모두 되지만, 사용자가 예상하는 대로 동작하지는 않는다. SpreadsheetCell 생성자에 대한 명시적 호출은 실제로 SpreadsheetCell 타입의 이름이 없는 새로운 임시 개체가 생성이 되고, 초기화 할 개체의 생성자를 호출하지 않는다.<br>
<b>기본 생성자</b>는 매개 변수가 없는 생성자이다. 0-매개 변수 생성자(0-argument constructor) 라고 부르기도 한다. 기본 생성자를 사용하면 클라이언트에서 초기 값을 지정하지 않아도 멤버 변수에 초기 값을 제공할 수 있다.<br>
기본 생성자가 필요한 경우를 보자. 개체의 배열의 경우, 개체의 배열을 생성하는 작업은 두 가지 과정을 거친다. 모든 개체를 연속된 메모리 공간에 할당하고, 각 개체에 대해 기본 생성자를 호출한다. C++에서는 배열 선언 코드에 다른 생성자를 직접 호출하도록 하는 구문을 제공하지 않는다. 예를 들어, SpreadsheetCell 클래스의 기본 생성자를 정의하지 않으면 다음 코드는 컴파일 되지 않는다.<br>
```cpp
SpreadsheetCell cells[3]; //기본 생성자가 없으면 컴파일이 되지 않는다.
SpreadsheetCell* myCellp = new SpreadsheetCell[10]; //마찬가지로 컴파일 되지 않는다.
```
다음과 같은 구문을 사용하면 스택 기반 배열에서는 이러한 제약을 피할 수 있다.<br>
```cpp
SpreadsheetCell cells [3] = {SpreadsheetCell (0), SpreadsheetCell (23), SpreadsheetCell (41)};
```
그러나 클래스의 객체 배열을 만들 때에는 기본 생성자가 있는지 확인하는 것이 좋다. std::vector와 같은 STL 컨테이너에 클래스 개체를 저장할 때에도 기본 생성자가 필요하다. 그리고, 기본 생성자는 클래스의 개체를 다른 클래스 내에서 생성하는 경우에도 유용하게 쓰인다. 다음은 기본 생성자를 사용하는 SpreadsheetCell 클래스 정의의 일부이다.<br>
```cpp
class SpreadsheetCell {
    public:
        SpreadsheetCell();
};
```
다음은 기본 생성자의 구현 부분이다.<br>
```cpp
SpreadsheetCell::SpreadsheetCell () {
    mValue = 0;
}
```
mString의 경우, std::string은 기본 생성자가 자동으로 호출되기 때문에 빈 문자열로 초기화된다. 스택에서 기본 생성자는 다음과 같이 사용한다.<br>
```cpp
SpreadsheetCell myCell;
myCell.setValue(6);
cout << "cell 1: " << myCell.getValue() << endl;
```
힙에서 기본 생성자는 다음과 같이 사용하다.<br>
```cpp
auto smartCellp = make_unique<SpreadsheetCell> ();
/*SpreadsheetCell* myCellp = new SpreadsheetCell ();
SpreadsheetCell* myCellp = new SpreadsheetCell;
delete myCellp; myCellp = nullptr;*/
```
프로그래머가 생성자를 정의하지 않은 경우, 컴파일러가 자동으로 기본 생성자를 만든다. 이것을 Compiler-Generated Default Constructor라고 한다. 처음에 정의했던 SpreadsheetCell 클래스는 다음과 같았다.<br>
```cpp
class SpreadsheetCell {
    public:
        void setValue (double inValue);
        double getValue () const;
    private:
        double mValue;
};
```
이렇게 정의를 한다면, 기본 생성자를 선언하지는 않았지만 다음 코드는 완벽하게 작동한다.<br>
```cpp
SpreadsheetCell myCell;
myCell.setValue(6);
```
다음 정의는 double생성자를 선언했다는 점을 제외하고는 이전 정의와 똑같다. 여전히 기본 생성자를 명시적으로 선언하지 않았다.<br>
```cpp
class SpreadsheetCell {
    public:
        SpreadsheetCell (double initialValue);
};
```
위 정의를 사용하면 다음 코드는 더 이상 컴파일 되지 않는다.<br>
```cpp
SpreadsheetCell myCell;
myCell.setValue(6);
```
이유는, 만약 생성자를 선언하지 않았다면 컴파일러는 매개 변수가 필요 없는 생성자를 하나 생성하지만, 기본 생성자 혹은 다른 생성자를 명시적으로 선언한 경우에는 더 이상 컴파일러에서 기본 생성자를 생성하지 않기 때문이다.<br><br>
<b>아. 복사 생성자</b><br>
C++에는 복사 생성자라는 특수한 생성자가 있어 다른 개체의 정확한 복사본인 개체를 만들 수 있다. 복사 생성자를 직접 작성하지 않으면 C++에서 원본 개체의 동등한 멤버 변수에서 새 개체의 각 멤버 변수를 초기화 하는 작업을 수행한다. 이 초기화 과정은 복사 생성자의 호출을 의미한다. 다음은 SpreadsheetCell 클래스의 복사 생성자에 대한 선언이다.<br>
```cpp
class SpreadsheetCell {
    public:
        SpreadsheetCell (const SpreadsheetCell& src);
};
```
복사 생성자가 원본 개체에 대해 참조를 한다. 다른 생성자들과 마찬가지로 값을 반환하지 않는다. 생성자 내부에서 원본 개체의 모든 멤버 변수를 복사해야 한다. 엄밀히 말하면, 복사 생성자에서 원하는 것은 무엇이든 할 수 있지만, 일반적으로 예상되는 행동을 따르고 원본 개체의 데이터를 새로운 개체에 복사하는 것이 좋다. 다음은 SpreadsheetCell 클래스의 복사 생성자 구현 부분이다.<br>
```cpp
SpreadsheetCell::SpreadsheetCell (const SpreadsheetCell& src) :
 mValue(src.mValue), mString(src.mString) { }
```
C++에서 함수에 인자를 전달하는 기본적인 방법은 pass-by-value(call-by-value)이다. 이는 함수 또는 메소드가 인자로 값 또는 개체의 복사본을 받는다는 것을 의미한다. 따라서 사용자가 개체를 함수나 메소드로 전달할 때마다 컴파일러에서 해당 개체를 초기화 하기 위해 복사 생성자를 호출한다. 예를 들어 SpreadsheetCell 클래스의 setString()메소드가 다음과 같다고 가정해보자.<br>
```cpp
void SpreadsheetCell::setString (string inString) {
    mString = inString;
    mValue = stringToDouble(mString);
}
void SpreadsheetCell::setString (string inString) {
    mString = inString;
    mValue = stringToDouble(mString);
}
```
다시 말하지만, C++ string은 기본적으로 제공되는 자료형이 아닌 클래스이다. 코드가 setString()을 호출할 때 inString 매개 변수는 복사 생성자의 호출로 인해 초기화된다. 복사 생성자의 인자로는 setString()에 전달한 문자열이 전달된다. 다음 코드에서는 setString()의 inString개체에 대해 string 복사 생성자가 호출된다.<br>
```cpp
SpreadsheetCell myCell;
string name = "heading one";
myCell.setString(name);
```
setString() 메소드가 종료되면 inString은 소멸된다. inString은 단지 name의 복사본이기 때문에 name은 온전히 남아있다.<br>
함수나 메소드에서 개체를 반환할 때도 복사 생성자가 호출된다. 이러한 경우에는, 컴파일러가 복사 생성자를 통해 이름이 없는 임시 객체를 생성한다.<br>
복사 생성자를 명시적으로 호출할 수도 있다. 한 개체를 다른 개체의 정확한 복사본으로 만드는 것은 종종 유용하다. 예를 들어, 다음과 같은 코드를 통해 SpreadsheetCell 개체의 복사본을 생성할 수 있다.<br>
```cpp
SpreadsheetCell myCell2(4);
SpreadsheetCell myCell3(myCell2);
```
### 제4장. 클래스와 객체의 심화
##### 가. copy and swap idiom
스마트 포인터와 같은 리소스를 관리하는 모든 클래스는 The Big Three라 불리는 것들을 구현해야 한다. 복사 생성자와 소멸자의 목표와 구현은 간단하지만 복사 할당 연산자는 어렵다. 이를 해결하기 위한 해결책이 복사 및 교환 관용구(copy and swap idiom)이며, 할당 연산자가 코드 중복(Code Duplication)을 피하고 강력한 예외 보장(Strong Exception Guarantee)을 제공하는 두 가지 작업을 지원한다.<br>
작동 방식을 알아보자. 개념적으로 복사 생성자의 기능을 이용하여 데이터의 복사본을 만든 뒤, 복사된 데이터를 swap함수로 가져와 이전 데이터를 새 데이터로 바꾼다. 그 후, 임시 복사본이 삭제되고 이전 데이터가 삭제된다. copy and swap idiom을 사용하기 위해서는 복사 생성자, 소멸자, swap함수 3가지가 필요하다. swap함수는 클래스의 두 개체를 서로 바꿔주는 함수이다. swap함수를 직접 구현하지 않고 std::swap함수를 사용하고 싶을 수도 있겠지만, 이것은 불가능하다. std::swap은 구현 내에서 복사 생성자와 복사 할당 연산자를 사용하여 궁극적으로 할당 연산자를 자체적으로 정의하려 한다.

##### 나. 0의 규칙
현대 C++에서는 0의 규칙을 채택해야 한다. 0의 규칙은 클래스에 소멸자, 복사 및 이동 생성자, 복사 및 이동 할당 연산자 총 5개의 특별한 메소드가 필요하지 않은 방식으로 클래스를 설계해야 한다고 명시한다. 그러기 위해서는 기본적으로 이전 스타일을 통해 동적으로 할당된 메모리는 사용하는 것 대신 STL 컨테이너 같은 현대적인 구조를 사용해야 한다. 예를 들어, 클래스 멤버 변수에 SpreadsheetCell&lowast;&lowast; 대신 vector<vector< SpreadsheetCell> > 쓰는 것은 아주 좋은 선택이다. 벡터 컨테이너는 메모리를 자동으로 처리하므로 위에서 언급한 다섯 가지 특별한 메소드가 필요하지 않다.현대 C++에서는 0의 규칙을 채택해야 한다. 0의 규칙은 클래스에 소멸자, 복사 및 이동 생성자, 복사 및 이동 할당 연산자 총 5개의 특별한 메소드가 필요하지 않은 방식으로 클래스를 설계해야 한다고 명시한다. 그러기 위해서는 기본적으로 이전 스타일을 통해 동적으로 할당된 메모리는 사용하는 것 대신 STL 컨테이너 같은 현대적인 구조를 사용해야 한다. 예를 들어, 클래스 멤버 변수에 SpreadsheetCell&lowast;&lowast; 대신 vector<vector< SpreadsheetCell> > 쓰는 것은 아주 좋은 선택이다. 벡터 컨테이너는 메모리를 자동으로 처리하므로 위에서 언급한 다섯 가지 특별한 메소드가 필요하지 않다.

##### 다. 멤버 변수(데이터 멤버)의 종류
C++은 멤버 변수에 대해 다양한 선택권을 제공한다. 클래스에 단순 멤버 변수를 선언하는 것 외에도 어느 클래스에서도 접근 가능한 static과 const멤버, reference멤버, const reference멤버 등과 같은 멤버 변수를 생성할 수 있다. 이 부분에서는 이러한 다양한 데이터 유형에 대해 설명한다.<br><br>
<b>A. static</b><br>
때때로 클래스의 각 개체에 변수의 복사본을 주는 것은 지나치거나 효과가 없다. 멤버 변수는 클래스에 특정할 수 있지만, 각 개체가 자체 복사본을 가지기에는 적합하지 않다. 예를 들어, 스프레드 시트에서 각각의 셀의 고유 번호를 0부터 지정할 때, 각 셀이 얻을 ID를 카운트하는 카운터가 필요하다. 이러한 경우, 모든 카운터를 동기화해야 하기 때문에 각 셀마다 모두 복사본을 갖고 있는 것은 비효율적이다.<br>
C++은 정적(static) 멤버 변수라는 해결책을 제공한다. 정적 멤버 변수는 개체 대신 클래스와 연결된 멤버 변수이다. 정적 멤버 변수는 클래스에만 적용되는 전역 변수로 간주할 수 있다. 다음은 SpreadsheetCell클래스에 추가로 정적 멤버 변수로 카운터를 선언한 코드이다.<br>
```cpp
class SpreadsheetCell {
    public:
        //...
    private:
        static int sCounter;
        //...
};
```
클래스 정의 부분에 작성하는 것 외에도 이러한 멤버에 대한 공간을 원본 클래스에 할당해야 한다. 이들은 동시에 초기화 할 수 있지만, 일반 변수와 멤버 변수와 달리 기본적으로 0으로 초기화된다. 정적 포인터는 nullptr로 초기화된다. 다음은 sCounter멤버에 대한 공간을 할당하고 초기화하는 코드이다.<br>
```cpp
int SpreadsheetCell::sCounter;
```
이 코드는 함수 또는 메소드 본문 외부에 나타난다. ::연산자를 통해 SpreadsheetCell클래스의 일부라고 지정하는 것을 제외하면 전역 변수를 선언하는 것과 비슷하다.<br><br>
<b>B. const</b><br>
클래스의 멤버 변수는 const로 선언할 수도 있다. 즉, 생성되고 초기화된 후에는 변경될 수 없다. 상수가 클래스에만 적용되는 경우 전역 상수 대신 정적 멤버를 사용해야 한다. Circle이라는 클래스에 원의 정보를 저장하고, 반지름의 최대 값을 maxR이라는 멤버 변수에 저장을 한다고 가정하자. maxR의 값은 변하지 않기 때문에 const로 선언해도 된다.<br>
```cpp
class Circle {
    public:
        const int maxR;
}
```

<b>C. reference</b><br>
클래스의 멤버 변수로 참조 변수를 쓸 수도 있다. 다음 예제는 클래스의 멤버 변수로 참조 변수를 쓰는 방법이다.<br>
```cpp
using T = int;

class A {
    public:
        A (const T &thing) : m_thing(thing) {}
    private:
        const T &m_thing;
};
```
물론 T 대신 다른 클래스 혹은 구조체가 와도 된다. A라는 클래스가 항상 T라는 타입의 데이터 혹은 개체를 참조해야 할 때 참조 변수를 멤버로 갖는다. 혹은, 크기가 크고 복잡한 개체를 복사할 때 발생하는 오버헤드를 방지하기 위해 쓰기도 한다.

##### 라. 메소드와 함수의 종류
또한, C++에서는 다양한 종류의 메소드도 제공한다. 이 부분에서는 메소드와 함수의 다양한 종류에 대해 다룬다.<br><br>
<b>A. static</b><br>
메소드도 멤버 변수처럼 클래스 전체에 적용되는 경우가 있다. 정적 멤버 변수처럼 정적 메소드도 쓸 수 있다. 이전에 구현했던 SpreadsheetCell 클래스에서 stringToDouble()과 doubleToString()메소드를 보자. 두 메소드는 특정 개체에 대한 정보에 접근하지 않으므로 정적일 수 있다. 다음은 정적 메소드에 대한 클래스 정의 방법이다.<br>
```cpp
class SpreadsheetCell {
    private:
        static std::string doubleToString (double val);
        static double stringToDouble (const std::string& str);
};
```
이 두 가지 구현 방법은 이전의 구현과 동일하다. 메소드 정의 앞에 정적 키워드를 반복하지 않는다. 그러나 정적 메소드는 특정 개체에 의해 호출되지 않으므로 this 포인터가 없으며 비 정적 멤버에 접근할 수 있는 특정 개체에 대해서는 실행되지 않는다. 사실 정적 메소드는 일반적인 함수와 같다. 유일한 차이점은 private이나 protected인 정적 멤버 변수에 접근 가능하다는 것이다.<br>
클래스 내의 모든 메소드에서 일반 함수처럼 정적 메소드를 호출한다. 따라서 SpreadsheetCell의 모든 메소드들의 구현은 그대로 유지될 수 있다. 클래스 외부에서 호출할 때에는 ::연산자를 이용해 메소드 이름 앞에 클래스 이름을 지정해야 한다. 접근 제어는 평소와 같이 적용된다. stringToDouble()이나 doubleToString()을 public으로 지정해 클래스 외부의 다른 코드에서 사용하게 할 수도 있다. 그럴 때에는 다음과 같이 호출한다.<br>
```cpp
string str = SpreadsheetCell::doubleToString (5.0);
```

<b>B. const</b><br>
const 개체는 값을 변경할 수 없는 개체이다. 어떤 메소드가 const개체를 참조하거나 포인터로 가리키려고 하면, 컴파일러는 해당 메소드가 멤버 변수를 변경하지 않는다고 보장하지 않는 한 해당 개체에 대한 메소드를 호출하지 못하게 한다. 메소드가 멤버 변수를 변경하지 않는다고 보장하는 방법은 const키워드로 메소드를 표기하는 것이다. 다음은 const로 선언된 멤버 변수를 변경하지 않는 메소드에 대한 SpreadsheetCell 클래스이다.<br>
```cpp
class SpreadsheetCell {
    public:
        double getValue () const;
        const std::string& getString () const;
};
```
const 키워드는 메소드 프로토타입의 일부이므로 다음과 같이 메소드를 정의한다.<br>
```cpp
double SpreadsheetCell::getValue () const {
    return mValue;
}

const std::string& SpreadsheetCell::getString () const {
    return mString;
}
```

<b>C. inline</b><br>
C++에서는 메소드 또는 함수에 대한 호출을 별도의 코드 블록을 호출하는 것으로 생성된 코드에 실제로 구현하지 말 것을 권장한다. 대신, 컴파일러는 메소드 또는 함수 호출이 이루어지는 코드에 메소드 또는 함수 본문을 직접 삽입해야 한다. 이러한 과정을 인라인 방식(inlining)이라 하고, 이러한 행위를 원하는 메소드 또는 함수를 인라인 메소드 또는 인라인 함수라 부른다. 인라인 방식은 #define 매크로보다 안전하다. 함수 또는 메소드 정의에서 inline 키워드를 이름 앞에 배치하여 인라인 함수 또는 인라인 메소드를 정의할 수 있다. 예를 들어, 다음 코드를 이용하면 SpreadsheetCell 클래스의 접근자 메소드를 인라인 메소드로 만들 수 있다.<br>
```cpp
inline double SpreadsheetCell::getValue () const {
    return mValue;
}

inline const std::string& SpreadsheetCell::getString () const {
    return mString;
}
```
이 코드를 통해 컴파일러가 함수 호출을 위한 코드를 생성하는 대신getValue() 및 getString() 호출을 실제 메소드 본문으로 대체하도록 암시한다. inline 키워드는 컴파일러에 대한 힌트일 뿐이고, 컴파일러는 inline으로 바꾸는 것이 성능을 저하시킬 것이라 예상되면 이를 무시할 수도 있다.<br>
또한, C++은 inline 키워드를 사용하지 않고 인라인 메소드를 선언하기 위한 대체 구문을 제공한다. 대신 클래스를 정의할 때 메소드의 구현까지 함께 한다. 다음은 SpreadsheetCell 클래스의 정의 부분이다.<br>
```cpp
class SpreadsheetCell {
    public:
        double getValue () const { return mValue; }
        const std::string& getString () const { return mString; }
};
```

##### 마. 메소드 오버로딩
전에 언급했듯이 한 클래스에 여러 개의 생성자를 작성할 수 있고, 그들의 이름을 모두 같다. 단지 생성자의 매개 변수의 개수나 유형만 다를 뿐이다. C++의 모든 메소드 및 함수에 대해 동일한 작업을 수행할 수 있다. 정확히는, 매개 변수의 개수 또는 유형이 다른 경우 함수 또는 메소드의 이름을 여러 기능에 사용하여 오버로딩 할 수 있다. 예를 들어, SpreadsheetCell 클래스에서 setString()과 setValue() 메소드를 set() 메소드 하나로 바꿀 수 있다. 그러면 클래스의 정의는 다음과 같아진다.<br>
```cpp
class SpreadsheetCell {
    public:
        void set (double inValue);
        void set (const std::string& inString);
};
```
set()의 구현 방식은 동일하게 유지된다. set()을 호출하면 컴파일러는 사용자가 전달한 인자들을 토대로 어떤 메소드를 호출할 지 결정한다.<br>
getValue()와 getString()도 get()으로 바꿀 수 있을 것 같지만, 그렇지 않다. C++에서는 메소드 반환 유형에 기반한 메소드 오버로딩을 할 수 없다. const로 선언한 메소드도 오버로딩 할 수 있다. 즉, 하나는 const로 선언하고, 하나는 const로 선언하지 않는 경우 이름과 매개 변수 구성이 같은 메소드를 두 가지 쓸 수 있다.

##### 바. 디폴트 매개변수
C++의 메소드 오버로딩과 유사한 기능으로 디폴트(기본) 매개 변수가 있다. 프로토 타입 및 메소드 매개 변수에 대한 기본 값을 지정할 수 있다. 사용자가 이러한 인자를 정하면 기본 값이 무시된다. 사용자가 이러한 인자를 생략하면 기본 값이 사용된다. 다만, 가장 오른 쪽에 있는 매개 변수부터 시작하는 연속된 매개 변수 목록에만 기본 값을 줄 수 있다. 그렇지 않으면, 컴파일러가 누락된 기본 인자를 기본 매개 변수와 일치시킬 수 없다. 기본 매개 변수 가능은 함수, 메소드 및 생성자에 사용할 수 있다. 다음과 같이 사용할 수 있다.<br>
```cpp
class SpreadsheetCell {
    public:
        void set (double inValue = 0);
        void set (const std::string& inString = "");
};
```
생성자의 구현은 그대로 유지된다. 기본 매개 변수는 메소드 선언 안에만 지정하고 구현에는 명시하지 않는다.

##### 사. 중첩 클래스(Nested Class)
클래스 정의에는 단지 메소드와 멤버 변수 말고도 여러 가지를 사용할 수 있다. 중첩된 클래스 혹은 구조체를 작성하거나 열거형 타입을 선언할 수도 있다. 만약 그것들이 public이라면, :: 연산자를 이용해 클래스 외부에서 접근할 수 있다. 클래스 내부에서도 다른 클래스를 선언할 수 있다. 예를 들어, Oxygen이라는 클래스는 Air이라는 클래스의 일부라고 할 수도 있다. 이 때, 두 클래스를 다음과 같이 정의할 수 있다.<br>
```cpp
class Air {
    public:
        class Oxygen {
            Oxygen () {
                //생성자
            }
        }

    private:
        Air () {
            //생성자
        }
}
```
이렇게 하면, Oxygen클래스는 Air클래스 내에서 정의되므로 외부에서는 Air 클래스를 거쳐 Oxygen클래스에 접근해야 한다. 예를 들어, 기본 생성자는 이제 다음과 같이 구현한다.<br>
```cpp
Air::Oxygen::Oxygen () {
    //생성자
}
```
이렇게 하면 너무 구문이 복잡해져 using문을 쓰기도 한다. using문을 쓰면 다음과 같이 생성자를 구현할 수 있다.<br>
```cpp
using O = Air::Oxygen;

O::Oxygen () {
    //생성자
}
```

##### 아. friends 클래스
C++ 클래스를 사용하면 클래스가 다른 클래스 또는 다른 클래스의 멤버들을 friends로 선언할 수 있으며, protected 멤버에 접근할 수 있다. 예를 들어 Matt클래스에서 다음과 같이 friends 클래스를 선언할 수 있다.<br>
```cpp
class Matt {
    public:
        friend class JusticeHui;
}
```
이렇게 하면 JusticeHui클래스의 모든 메소드가 private 혹은 protected로 선언된 멤버 변수와 메소드에 접근할 수 있다.<br>
JusticeHui클래스의 특정 메소드만 friends로 만들 때에는 다음과 같이 한다.<br>
```cpp
class Matt {
    public:
        friend void JusticeHui::say (string &str);
}
```

### 제5장. 상속
<b>가. 상속을 통한 클래스의 확장</b><br>
C++로 클래스를 작성할 때, 기존 클래스가 상속되거나 확장됨을 컴파일러에 알릴 수 있다. 이렇게 하면 클래스가 자동으로 부모 클래스라고 불리는(혹은 기본/수퍼 클래스라고 불리는) 원래 클래스의 멤버 변수 및 메소드가 자동으로 포함된다. 기존 클래스를 확장하면 클래스(이제는 파생/자식/하위 클래스라고 불리는 클래스)가 부모 클래스와의 다른 점만 정의하면 된다.<br>
C++에서 클래스를 확장하려면, 클래스를 정의할 때 확장할 클래스를 지정해야 한다. 상속에 대한 구문을 설명하기 위해 Super와 Sub라는 클래스를 사용할 것이다. 먼저, Super 클래스를 작성할 것이다.<br>
```cpp
class Super {
    public:
        Super ();
        void someMethod ();
    protected:
        int mProtectedInt;
    private:
        int mPrivateInt;
};
```
Super클래스를 상속받는 Sub클래스를 만들기 위해 다음 구문을 사용하여 컴파일러에 Sub가 Super에서 파생되었음을 알린다.<br>
```cpp
class Sub : public Super {
    public:
        Sub ();
        void someOtherMethod ();
};
```
Sub클래스 자체는 Super클래스의 특성을 공유하는 하나의 완전한 클래스이다. 그림1은 Sub와 Super의 단순한 관계를 보여준다. 다른 클래스들과 마찬가지로 Sub클래스와 같은 하위 클래스들의 개체를 선언할 수 있다. 그림2처럼 Sub클래스와 같은 하위 클래스를 상속하는 세 번째 클래스를 정의하여 클래스의 체인을 형성할 수 있다. 또한, 하위 클래스가 부모 클래스의 유일한 파생 클래스일 필요는 없다. 그림3에서와 같이 Super클래스를 상속하는 다른 클래스를 추가해 Sub의 형제 클래스를 만들 수도 있다.<br>
<img src = "https://i.imgur.com/6rfDxqm.png" width = "500px"><br>
<b>클라이언트나 코드의 다른 부분의 관점</b>에서 보면, Sub 타입의 개체도 Super클래스에서 파생되었기 때문에 Super 타입의 개체이다. 이는 Super와 Sub의 public 멤버를 모두 사용할 수 있음을 의미한다. 파생 클래스를 사용하는 코드는 상속 체인의 어떤 클래스가 호출할 메소드를 정의했는지 알 필요가 없다. 예를 들어, 다음 코드는 어떤 메소드가 Super클래스에서 선언이 되었더라도 Sub클래스에서 호출이 가능함을 보여준다.<br>
```cpp
Sub mySub;
mySub.someMethod ();
mySub.someOtherMethod ();
```
상속은 한 방향으로 작동한다는 것을 이해하는 것이 중요하다. Sub클래스는 Super클래스와 명확하게 정의된 관계를 갖고 있지만, Super클래스는 Sub클래스에 대해 아무것도 모른다. Super클래스는 하위 클래스가 아니기 때문에 Super타입의 개체는 Sub클래스의 public 멤버에 접근을 못한다. Super클래스에 someOtherMethod라는 public 메소드가 없기 때문에 다음 코드는 컴파일 되지 않는다.<br>
```cpp
Super mySuper;
mySuper.someOtherMethod (); //Error
```
개체에 대한 포인터 또는 참조는 선언된 클래스의 객체 또는 파생 클래스를 참조할 수 있다. 이 시점에서 이해해야 할 개념은, Super에 대한 포인터가 Sub타입의 개체를 가리킬 수 있다는 것이다. 참조의 경우도 마찬가지이다. 이 방법을 통해 Super에서 작동하는 코드는 Sub에서도 작동할 수 있다. 예를 들어, 다음 코드는 형식이 일치하지 않는 것처럼 보이지만 컴파일을 하면 정상적으로 작동한다.<br>
```cpp
Super* superPointer = new Sub(); //Sub클래스를 생성하여 Super 포인터에 저장
```
그러나 Super에 대한 포인터를 통해 하위 클래스의 메소드를 호출할 수 없다. 예를 들어, 다음 코드는 작동하지 않는다.<br>
```cpp
superPointer->someOtherMethod ();
```
이유는 다음과 같다. 객체가 Sub타입이기 때문에 someOtherMethod가 정의되어 있지만 컴파일러는 someOtherMethod가 정의되지 않은 Super타입으로만 생각하기 때문에 컴파일러에서 오류로 표시된다.<br>
<b>파생된 클래스의 관점</b>에서 보면, 그것이 어떻게 쓰이고 어떻게 행동하는지에 관해서는 별로 변한 것이 없다. 일반적인 클래스처럼 파생 클래스에 대한 메소드 및 멤버 변수를 정의할 수 있다. 이전에 선언한 Sub클래스에서는 someOtherMethod라는 메소드를 선언했었다. 이러한 방법을 통해 Super클래스를 확장할 수 있다. 파생 클래스는 상위 클래스의 public이나 protected 멤버에 접근할 수 있으며, 그렇기 때문에 상위 클래스에서 선언된 멤버들이 자신의 것인 것처럼 접근할 수 있다. 예를 들어, Sub클래스의 someOtherMethod에서 Super클래스의 일부로 선언된 mProtectedInt을 사용할 수 있다. 다음 코드는 이러한 구현을 보여준다. 상위 클래스의 멤버들에 접근하는 것은 파생 클래스의 멤버들에 접근하는 것과 다른 것이 없다.<br>
```cpp
void Sub::someOtherMethod () {
    cout << "I can access base class data member mProtectedInt." << endl;
    cout << "Its value is " << mProtectedInt << endl;
}
```
클래스에서 멤버를 protected로 선언할 경우, 파생 클래스가 해당 멤버에 대한 접근 권한을 갖는다. 만약 private로 선언한 경우에는 파생 클래스에서 접근할 수 없다. 다음 코드에서는 파생 클래스가 상위 클래스의 private멤버에 접근을 시도하기 때문에 someOtherMethod의 구현은 컴파일 되지 않는다.<br>
```cpp
void Sub::someOtherMethod () {
    cout << "I can access base class data member mProtectedInt." << endl;
    cout << "Its value is " << mProtectedInt << endl;
    cout << "The value of mPrivateInt is " << mPrivateInt << endl; // Error!
}
```
C++에서는 클래스를 final로 선언할 수도 있다. final을 이용해 선언하는 경우, 다른 클래스가 상속을 시도할 때 컴파일 에러가 발생한다. 클래스 이름 바로 뒤에 final 키워드를 사용하여 클래스를 final로 선언할 수 있다. 예를 들어, 다음 Super클래스는 final로 선언된다.<br>
```cpp
class Super final {
    //...
};
```
다음 클래스는 Super클래스를 상속받으려 시도하지만 Super은 final로 선언되었으므로 컴파일 에러가 발생한다.<br>
```cpp
class Sub : public Super {
    //...
};
```

<b>나. 상속을 통해 다형성 얻기</b><br>
이제 상위 클래스와 파생 클래스 간의 관계를 이해했으므로 상속을 가장 강력한 시나리오인 다형성으로 사용할 수 있다.<br>
전에 사용한 SpreadsheetCell에 대한 단순화된 클래스의 정의는 다음과 같다. 셀의 데이터는 double 혹은 string으로 설정할 수 있다. 그러나 이 예제에서는 항상 string으로 반환된다.<br>
```cpp
class SpreadsheetCell {
   public:
        SpreadsheetCell ();
        virtual void set (double inDouble);
        virtual void set (const std::string& inString);
        virtual std::string getString () const;
    private:
        static std::string doubleToString (double inValue);
        static double stringToDouble (const std::string& inString);
        double mValue;
        std::string mString;
};
```
때로는 double과 string간의 변환을 해야 한다. 이러한 이중성을 이루기 위해 클래스는 주어진 셀이 값을 하나만 포함할 수 있었어야 하더라도 클래스는 두 값을 모두 저장해야 한다. 더 나쁜 것은, 만약 수학 공식이나 날짜와 같은 데이터를 저장해야 하는 셀이 필요할 수도 있다. 그러므로 SpreadsheetCell클래스는 이러한 모든 데이터 유형과 이러한 유형 간의 변환을 지원하기 위해 확장될 것이다.<br>
다형성 SpreadsheetCell클래스를 디자인해보자. 합리적인 접근 방식은 문자열만 포함하도록 SpreadsheetCell의 범위를 좁히고 프로세스에서 StringSpreadsheetCell로 이름을 바꾸는 것이다. double형을 처리하기 위해, 두 번째 클래스인 DoubleSpreadsheetCell를 만든 뒤, StringSpreadsheetCell을 이어받아 자체 형식에 따른 기능을 제공한다. 그림4는 그러한 설계를 보여준다. 이 접근 방식은 DoubleSpreadsheetCell이 StringSpreadsheetCell에서만 파생되어 기본 제공 기능을 일부 활용하기 때문에 재사용을 위한 상속을 모델링한다.<br>
<img src = "https://i.imgur.com/FcAjVJp.png" width = "300px"><br>
그림4에 나와있는 설계를 구현할 경우 파생 클래스가 상위 클래스의 기능 대부분을 재정의하는 것은 아닐지도 모른다. double형은 거의 모든 경우에 문자열과 전혀 다르게 취급되기 때문에 관계가 원래의 생각과 다를 수 있다. 하지만, string을 저장하는 셀과 double을 저장하는 셀은 분명히 관계가 있다. 그림4를 사용하는 대신, DoubleSpreadsheetCell이 StringSpreadsheetCell이라는 것을 이용해 더 좋게 디자인할 수 있다.<br>
<img src = "https://i.imgur.com/zJTzjWJ.png" width = "300px"><br>
그림5의 설계는 SpreadsheetCell계층에 대한 다형성 접근을 보여준다. Double/StringSpreadsheetCell은 공통 부모 클래스인 SpreadsheetCell로부터 상속받으므로 다른 코드의 관점에서는 교환할 수 있다. 실용적인 측면에서는 다음을 의미한다.<br>
1. 파생된 두 클래스 모두 부모 클래스에 의해 정의된 동일한 인터페이스를 지원한다.
2. SpreadsheetCell객체를 사용하는 코드는 셀이 Double/StringSpreadsheetCell인지 알지 못하면서 인터페이스의 모든 메소드를 호출한다.
3. 가상 메소드를 통해 개체의 클래스에 따라 인터페이스의 모든 메소드의 적절한 인스턴스가 호출될 것이다.

모든 셀은 SpreadsheetCell클래스에서 파생되고 있으므로 먼저 해당 클래스를 작성하는 것이 좋다. 상위 클래스를 작성할 때, 파생 클래스가 서로 어떻게 관련되고 있는지를 고려해야 한다. 이 정보를 통해 상위 클래스에 포함되는 공통점을 파생할 수 있다. 예를 들어, 문자열 셀과 double셀은 둘 다 데이터의 한 부분을 포함한다는 점에서 유사하다. 데이터가 사용자로부터 전송되고 사용자에게 다시 표시되므로 이 값은 문자열로 설정되고 문자열로 검색된다. 이러한 동작은 기본 클래스를 구성하는 공유 기능이다.<br>
SpreadsheetCell클래스는 SpreadsheetCell클래스를 상속하는 모든 파생 클래스가 지원하는 동작을 정의한다. 위의 예에서, 모든 셀은 문자열로 설정할 수 있어야 한다. 또한, 모든 셀은 현재 값을 문자열로 반환할 수 있어야 한다. 상위 클래스의 정의는 이러한 메소드를 정의하지만, 멤버 변수는 없다. 그 이유는 뒤에서 설명한다.<br>
```cpp
class SpreadsheetCell {
   public:
       SpreadsheetCell ();
       virtual ~SpreadsheetCell ();
       virtual void set (const std::string& inString);
       virtual std::string getString () const;
};
```
이 클래스의 구현 부분을 작성하기 시작하면 매우 빠르게 문제에 직면하게 된다. SpreadsheetCell클래스는 double이나 string을 포함하지 않기 때문에 구현할 수 없다. 더 일반적으로, 이러한 행동들을 실제로 정의하지 않고는 파생 클래스가 지원하는 행동들을 선언하는 부모 클래스를 작성할 수가 없다. 한 가지 접근 가능한 방식은 이러한 행동에 대해 “아무것도 하지 않음” 기능을 구현하는 것이다. 예를 들어, 상위 클래스에서 설정할 것이 없으므로 SpreadsheetCell클래스에서 set()을 호출하는 것은 효과가 없다. 하지만, 이 접근 방식을 옳지 않다. 이상적으로는, 상위 클래스의 인스턴스인 개체가 없어야 한다. set()을 호출하는 것은 Double/StringSpreadsheetCell에서 호출해야만 효과가 있다. 좋은 해결책은 이러한 제한 조건을 포함해야 한다. 위의 코드는 SpreadsheetCell클래스의 가상 소멸자를 선언한다. 이렇게 하지 않으면 컴파일러에서 기본 소멸자를 생성하지만 가상이 아니다. 즉, 가상 소멸자를 직접 선언하지 않으면 앞에서 설명한 포인터 혹은 참조를 통해 파생 클래스가 소멸될 수 있다.<br>
가상 메소드는 클래스 정의에 명시적으로 정의되지 않은 메소드이다. 메소드를 가상으로 만들면 현재 클래스에 메소드에 대한 정의가 없음을 컴파일러에 알리는 것이다. 어떤 코드도 그것을 인스턴스화할 수 없기 때문에 그 클래스를 추상적이라 한다. 컴파일러는 클래스에 하나 이상의 가상 메소드가 포함된 경우 해당 유형의 개체를 구성하는 데 사용할 수 없다는 사실을 적용한다.<br>
가상 메소드를 선언하기 위한 특수 구문이 있다. 메소드 선언 뒤에는 = 0이 나온다. 구현 부분은 적을 필요가 없다.<br>
```cpp
class SpreadsheetCell {
    public:
        SpreadsheetCell ();
        virtual ~SpreadsheetCell ();
        virtual void set (const std::string& inString) = 0;
        virtual std::string getString () const = 0;
};
```
클래스가 추상 클래스이므로 SpreadsheetCell개체를 생성할 수 없다. 다음 코드는 컴파일 되지 않으며 하나 이상의 가상 함수가 추상화되기 때문에 SpreadsheetCell유형의 개체를 선언할 수 없다.<br>
```cpp
SpreadsheetCell cell; //Error!
```
그러나, 다음 코드는 컴파일 된다.<br>
```cpp
SpreadsheetCell* ptr = nullptr;
```
이는 다음과 같은 추상적인 클래스의 파생 클래스를 인스턴스화 해야 하기 때문에 가능하다.<br>
```cpp
ptr = new StringSpreadsheetCell ();
```

<b>다. 다중 상속 작업</b><br>
여러 클래스를 부모 클래스로 하는 하위 클래스를 정의하는 것은 구문적인 관점에서 매우 간단하다. 클래스 이름을 선언할 때 부모 클래스를 개별적으로 나열하기만 하면 된다.<br>
```cpp
class Baz : public Foo, public Bar {
    //...
};
```
여러 개의 부모를 나열하면 Baz개체의 특성은 다음과 같다.<br>
1. Baz개체는 Foo와 Bar의 public 메소드를 지원한다.
2. Baz의 메소드는 Foo와 Bar의 protected 멤버에 접근할 수 있다.
3. Baz개체는 Foo또는 Bar로 upcast 할 수 있다.
4. 새로운 Baz개체를 작성하면 Foo와 Bar의 기본 생성자가 자동으로 호출된다.
5. Baz개체를 삭제하면 Foo와 Bar의 소멸자가 클래스 정의에 나열된 순서의 역순으로 자동 호출된다.

다음 코드는 Dog와 Bird라는 클래스를 부모로 같는 DogBird라는 클래스를 보여준다.<br>
```cpp
class Dog {
    public:
        virtual void bark () { cout << "Woof!" << endl; }
};
class Bird {
    public:
        virtual void chirp () { cout << "Chirp!" << endl; }
};
class DogBird : public Dog, public Bird {
    //...
};
```
DogBird의 체계는 그림6과 같다.<br>
<img src = "https://i.imgur.com/3j6h5mI.png" width = "300px"><br>
여러 부모가 있는 클래스의 객체를 사용하는 것은 다중 부모가 없는 객체를 사용하는 것과 다를 바 없다. 사실 클라이언트는 그 클래스가 부모가 두 개라는 것을 알 필요조차 없다. 중요한 것은 클래스가 지원하는 속성과 행동들이다. 이 경우, DogBird 개체는 Dog와 Bird의 모든 public 메소드를 지원한다.<br>
```cpp
DogBird myConfusedAnimal;
myConfusedAnimal.bark ();
myConfusedAnimal.chirp ();
```
프로그램의 출력은 다음과 같다.<br>
```cpp
Woof!
Chirp!
```

### 제6장. 형 변환 연산
C++은 static_cast, dynamic_cast, const_cast, reinterpret_cast의 네 가지 특정 형 변환을 지원한다. ()을 사용하는 기존 C-스타일 캐스팅은 여전히 C++에서 여전히 작동한다. C-스타일 캐스팅은 4개의 C++ 캐스팅을 모두 포함하기 때문에 오류가 발생하기 쉽다. C++ 캐스팅은 보다 안전하고 구문적으로 잘 보이기 때문에 새로운 코드로만 사용할 것을 추천한다.<br><br>
<b>A. const_cast</b><br>
const_cast는 가장 간단하다. 이를 사용하여 변수의 const-ness를 제거할 수 있다. 이것은 네 가지 방법 중 유일하게 const-ness를 제거할 수 있는 캐스팅이다. 변수가 const이면 const로 유지해줘야 한다. 그러나 실제로는 함수가 const변수를 취하도록 지정되어 있고, const가 아닌 함수로 전달해야 하는 경우가 있다. 올바른 해결책은 프로그램에서 const를 일관되게 만드는 것이지만, 타사 라이브러리를 사용하는 경우 항상 선택할 수 있는 것은 아니다. 따라서, 변수의 const-ness를 없애야 하는 경우도 있지만, 호출하는 함수가 객체를 수정하지 않을 경우에만 이 작업을 수행해야 한다. 그렇지 않으면 프로그램을 재구성해야 한다.<br>
```cpp
extern void ThirdPartyLibraryMethod (char* str);
void f (const char* str) {
    ThirdPartyLibraryMethod (const_cast<char*>(str));
}
```
<b>B. static_cast</b><br>
static_cast를 사용하여 언어에서 지원되는 명시적 변환을 수행할 수 있다. 예를 들어, 정수 나눗셈을 피하기 위해 int를 double로 변환해야 하는 산술 식을 쓰는 경우, static_cast를 사용한다. 이 경우에는 두 개의 피연산자 중 하나를 double로 만들고, C++이 부동 소수점 나눗셈을 수행하도록 하기 때문에 static_cast를 한 번 하면 된다.<br>
```cpp
int i = 3;
int j = 4;
double result = static_cast<double>(i) / j;
```
또한, static_cast를 사용하여 사용자 정의 생성자 또는 변환 루틴으로 인해 허용되는 명시적 변환을 수행할 수 있다. 예를 들어, A클래스에 B개체를 가져오는 생성자가 있는 경우, B개체를 static_cast를 사용해 A개체로 변환할 수 있다. 그러나 이 동작을 원하는 대부분의 상황에서는 컴파일러가 자동으로 변환을 수행한다.<br>
static_cast의 또 다른 용도는 상속 계층에서 다운그레이드를 수행하는 것이다. 예를 들어<br>
```cpp
class Base {
    public:
        Base () {}
        virtual ~Base () {}
};
class Derived : public Base {
    public:
        Derived () {}
        virtual ~Derived () {}
};
int main () {
    Base* b;
    Derived* d = new Derived ();
    b = d; // Don't need a cast to go up the inheritance hierarchy
    d = static_cast<Derived*>(b); // Need a cast to go down the hierarchy
    Base base;
    Derived derived;
    Base& br = derived;
    Derived& dr = static_cast<Derived&>(br);
    return 0;
}
```
이러한 캐스팅은 포인터와 참조에서 모두 작동한다. 그것들은 그 자체로 개체를 다루지 않는다.<br><br>
<b>C. reinterpret_cast</b><br>
reinterpret_cast는 static_cast보다 좀 더 강력하고, 안전하지 않다. C++ 타입 규칙에 의해 기술적으로 허용되지 않는 일부 캐스팅을 실행하는 데 사용할 수 있지만 경우에 따라 프로그래머에게 적합할 수 있다. 예를 들어, 한 타입에 대한 참조를 다른 유형에 대한 참조로 지정할 수 있다. 마찬가지로, 상속 계층과 관련이 없더라도 포인터 타입을 다른 포인터 타입에 지정할 수 있다. 이는 일반적으로 포인터를 void*로 선언한 뒤, 다시 배치하는 데 사용된다. void* 포인터는 메모리의 특정 위치에 대한 포인터일 뿐이다. void* 포인터는 관련된 유형 정보가 없다. 다음은 몇 가지 예이다.<br>
```cpp
class X {};
class Y {};
int main () {
    X x;
    Y y;
    X* xp = &x;
    Y* yp = &y;
    //관계없는 포인터로 캐스팅하기 위해 reinterpret_cast가 필요함
    //static_cast는 작동하지 않음
    xp = reinterpret_cast<X*>(yp);
    //void* 포인터에는 캐스팅이 필요하지 않음
    void* p = xp;
    //void* 포인터로의 변환을 위해 reinterpret_cast 필요
    xp = reinterpret_cast<X*>(p);
    //관계없는 포인터로 캐스팅하기 위해 reinterpret_cast가 필요함
    //static_cast는 작동하지 않음
    X& xr = x;
    Y& yr = reinterpret_cast<Y&>(x);
    return 0;
}
```
reinterpret_cast는 타입 검사를 수행하지 않고 변환을 수행하므로 매우 주의해야 한다.<br><br>
<b>D. dynamic_cast</b><br>
dynamic_cast는 상속 계층 내의 캐스팅에 대한 런타임 검사를 제공한다. 포인터나 참조를 캐스팅하는데 사용할 수 있다. dynamic_cast는 런타임 중에 개체의 런타임 타입 정보를 확인한다. 캐스팅이 타당하지 않은 경우, dynamic_cast는 nullptr(포인터의 경우)를 반환하거나 std::bad_cast예외(참조의 경우)를 표시한다. 런타임 타입 정보는 개체의 실행 파일에 저장된다. 따라서 dynamic_cast를 사용하려면 클래스에 가상 메소드가 하나 이상 있어야 한다. 그렇지 않을 경우 오류가 발생한다. 예를 들어, Microsoft VC++에서는 다음과 같은 오류가 발생한다.<br>
`error C2683: 'dynamic_cast' : 'MyClass' is not a polymorphic type.`<br>
다음과 같은 클래스 계층이 있다고 가정하자.<br>
```cpp
class Base {
    public:
        Base () {}
        virtual ~Base () {}
};

class Derived : public Base {
    public:
        Derived () {}
        virtual ~Derived () {}
};
```
다음 예는 dynamic_cast의 올바른 사용을 보여준다.<br>
```cpp
Base* b;
Derived* d = new Derived ();
b = d;
d = dynamic_cast<Derived*>(b);
```
다음 예는 오류를 발생시킨다.<br>
```cpp
Base base;
Derived derived;
Base& br = base;
try {
    Derived& dr = dynamic_cast<Derived&>(br);
} catch (const bad_cast&) {
    cout << "Bad cast!\n";
}
```
static_cast 또는 reinterpret_cast를 사용해 상속 계층 구조에서 동일한 캐스팅을 수행할 수 있다. dynamic_cast와의 차이는 dynamic_cast는 런타임 타입 검사를 수행하는 반면, static_cast와 reinterpret_cast는 오류가 있어도 캐스팅을 수행한다는 것이다.<br>
 다음 표에는 다양한 상황에 사용해야 하는 캐스팅이 요약되어 있다.<br>
 <img src = "https://i.imgur.com/3ugU0xh.png" width = "300px">
