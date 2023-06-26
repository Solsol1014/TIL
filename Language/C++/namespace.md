# Namespace (이름공간)
## 1. 정의
어떤 정의된 객체에 대하여 어디 소속인지 지정해주는 것.   
중복된 이름을 가진 함수를 구분하기 위해 같은 이름의 함수더라도 소속된 이름 공간이 다르면 다른 것으로 취급한다.   
   
자바에서 모듈을 쓰는 것과 같은 이유 아닐까..?

    std::cout

위의 경우, std라는 namespace에 정의되어 있는 cout을 의미한다.   
std::없이 cout으로 하면 오류 발생.   
   
## 2. Namespace 정의하기

    //header1.h 의 내용
    namespace header1 {
        int foo();
        void bar();
    }

---

    //header2.h 의 내용
    namespace header2 {
        int foo();
        void bar();
    }    

위 코드에서 header1 안에 정의된 foo는 header1이라는 namespace에 있는 함수 foo가 되고, header2에 있는 foo또한 header2라는 namespace에 있는 함수 foo가 된다.   

## 3. 함수 호출
자기 자신이 정의되어 있는 이름공간 안에서는 함수 앞에 이름공간을 명시하지 않아도 사용 가능하다.

    #include "header1.h"
    #include "header2.h"

    namespace header1 {
        int func() {
            foo(); //알아서 header1::foo() 가 실행됨.
            header2::foo(); //header2::foo() 실행됨.
        }
    }

header1 이름공간 안에서 foo()를 부르면 알아서 header1::foo()가 실행된다. header2::foo()를 부르기 위해서는 header2::foo()라고 입력하면 불러와진다.   

#include 다음에 using header1::foo; 를 쓰면 foo();를 했을때 자동으로 header1::foo(); 를 호출한다.   
한 함수가 아닌, 특정 이름 공간안에 정의된 모든 함수들을 이름공간 이름 없이 사용하고 싶다면, 

    #include "header1.h"

    using namespace header1;

    int main() {
        foo();
        bar();
    }

위와 같이 using namespace header1; 이라고 명시하면 된다.   

해당 경우에도 header2의 함수를 사용하고 싶으면 평소처럼 header2::foo(); 로 써주면 사용 가능하다.   

## 4. 이름 없는 이름공간
이름공간에 이름을 설정하지 않아도 되는데, 이와 같은 경우에는 해당 이름공간에 정의된 것들은 해당 파일 안에서만 접근할 수 있게 된다.   
마치 static 키워드를 사용한 것과 같은 효과를 내는 것이다.

    #include <iostream>

    namespace {
        int OnlyInThisFile() {} // == static int OnlyInThisFile()

        int only_in_this_file = 0; // == static int x;
    }

    int main() {
        OnlyInThisFile();
        only_in_this_file = 3;
    }

위의 경우, OnlyInThisFile 함수와 only_in_this_file 변수는 해당 파일 안에서만 접근 가능하다.