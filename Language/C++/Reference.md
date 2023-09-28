# Reference (참조자)
## 1. 참조자의 도입

    #include <iostream>

    int change_val(int *p) {
        *p = 3;

        return 0;
    }

    int main() {
        int number = 5;

        std::cout << number << std::endl;
        change_val(&number);
        std::cout << number << std::endl;
        
        return 0;
    }

위의 방법은 C에서 포인터와 주소를 이용했던 방법이다. 포인터와 주소값뿐만 아니라, Cpp에서는 다른 변수나 상수를 가리키는 방법으로 참조자 (reference)를 제공한다.

    #include <iostream>

    int main() {
        int a = 3;
        int& another_a = a;

        another_a = 5;
        std::cout << "a: " << a << std::endl;
        std::cout << "another_a: " << another_a << std::endl;

        return 0;
    }

위의 값을 컴파일하면 a와 another_a 둘 다 5로 나온다.

    int& another_a = a;

참조자를 정의하는 방법은 위 코드처럼 가리키고자 하는 타입 뒤에 &를 붙이면 된다.

int형 변수의 참조자를 만들고 싶을 때는 int&를, double의 참조자를 만드려면 double&를, int\*과 같은 포인터 타입의 참조자를 만드려면 int\*&으로 쓰면 된다.

위와 같이 작성한다는 것은 another_a는 a의 참조자다라는 것은 선언하는 건데, 이 말은 another_a는 a의 또다른 이름이라고 정의하는 것이다. 따라서, another_a에 어떠한 작업을 수행하던 이는 a에 해당 작업을 하는 것과 같다.

참조자와 포인터는 유사한 개념인데, 이 둘간에는 몇 가지 중요한 차이점이 존재한다.

### 레퍼런스는 반드시 선언할때 어떤 변수의 참조자인지 지정해야한다

    int& another_a;

위의 코드와 같은 문장은 불가능하다. 하지만 포인터의 경우,

    int* p;

는 문제가 없는 코드이다.

### 레퍼런스가 한번 지정이 되면 절대로 다른 변수의 레퍼런스가 될 수 없다
레퍼런스는 한 번 어떤 변수의 참조자가 된다면, 더이상 다른 변수를 참조할 수 없게 된다.

예를 들어,

    int a = 10;
    int& another_a = a;

    int b = 3;
    another_a = b;

라는 코드를 살펴보면, 마지막 줄의 코드는 another_a에게 다른 변수인 b를 가리키라고 하는 것이 아니라, 그냥 another_a가 참조한 a에 b의 값을 대입하라는 의미이다.

반면, 포인터의 경우에는

    int a = 10;
    int* p = &a;

    int b = 3;
    p = &b;

위의 코드처럼 어떤 변수를 가리키는지 자유롭게 바뀔 수 있다.

### 레퍼런스는 메모리 상에 존재하지 않을 수도 있다