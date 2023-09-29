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

    int a = 10;
    int* p  = &a;

위의 코드에서, p는 메모리 상에서 8바이트를 차지하게 된다. 하지만 레퍼런스의 경우는,

    int a = 10;
    int& another_a = a;

위의 코드에서 another_a가 쓰이는 자리는 모두 a로 바뀌어진다. 따라서, 이 경우 레퍼런스는 메모리 상에 존재하지 않게된다.

## 2. 함수 인자로 레퍼러스 받기

    #include <iostream>

    int change_val(int& p) {
        p = 3;

        return 0;
    }

    int main() {
        int number = 5;
        
        std::cout << number << std::endl;
        change_val(number);
        std::cout << number << std::endl;

        return 0;
    }

위의 코드 중,

    int change_val(int& p)

와 같이 함수의 인자로 레퍼런스를 받게 했는데, 여기서 p가 정의되는 순간은 change_val(number)로 호출할 때이므로, 사실상 int& p = number가 실행된다고 생각하면 된다.

여기서, 포인터가 인자일 때와는 다르게 number 앞에 &를 붙일 필요가 없다. 이는 레퍼런스를 정의할 때 대입하는 변수는 아무것도 붙이지 않았던 것과 같은 맥락이다.

## 3. 여러가지 참조자 예시들
### 참조자의 참조자

    #include <iostream>

    int main() {
        int x;
        int& y = x;
        int& z = y;

        x = 1;
        std::cout << "x: " << x << "y: " << y << "z: " << z << std::endl;

        y = 2;
        std::cout << "x: " << x << "y: " << y << "z: " << z << std::endl;

        z = 3;
        std::cout << "x: " << x << "y: " << y << "z: " << z << std::endl;

        return 0;
    }

위의 코드 중 int& z = y; 부분에서 y는 x의 참조자이니 z의 타입은 int&&라고 생각할 수 있지만, 참조자의 참조자를 만드는 것은 필요 없는 일이기 때문에, C++ 문법 상 참조자의 참조자를 만드는 것은 금지되어 있다.

따라서, int& z = y;는 x의 참조자를 선언해라와 같은 의미가 된다.

포인터로 할 수 있는 것을 레퍼런스로 하는 이유는 불필요한 &와 *이 필요가 없기 때문에 코드를 간결하게 나타낼 수 있기 때문이다.

예를 들어, cin 함수의 경우, scanf에서는 &를 써야했지만, cin함수의 인자로 레퍼런스를 받기 때문에 &를 쓰지 않아도 잘 작동한다.

### 상수에 대한 참조자

    #include <iostream>

    int main() {
        int& ref = 4;

        std::cout << ref << std::endl;

        return 0;
    }

위의 코드를 컴파일하면 오류가 난다. 상수값은 리터럴이기 때문에, 상수를 레퍼런스로 참조한다면 리터럴의 값을 바꾸는 행위가 가능하다는 말이 된다. 따라서, C++ 문법상, 상수 리터럴을 일반적인 레퍼런스가 참조하는 것은 불가능하게 되어있다.

하지만,

    const int& ref = 4;

처럼 상수 참조자로 선언한다면 리터럴또한 참조할 수 있다. 따라서,

    int a = ref;

는 a = 4;와 동일하게 처리된다.

### 레퍼런스의 배열과 배열의 레퍼런스

    int a, b;
    int& arr[2] = {a, b};

위의 코드가 가능하다고 생각할 수 있지만, 해당 코드를 컴파일해보면 레퍼런스의 배열을 불법(illegal) 이라고 한다.

C++의 규정을 찾아보면,
>There shall be no references to references, no arrays of references, and no pointers to references

>레퍼런스의 레퍼런스, 레퍼런스의 배열, 레퍼런스의 포인터는 존재할 수 없다.

라고 언어 차원에서 나와있다. 

C++상에서 배열은 문법 상 배열의 이름이 첫번째 원소의 주소값으로 변환이 될 수 있어야 하는데, 주소값이 존재한다는 것은 해당 원소가 메모리 상에서 존재한다는 것을 의미한다. 하지만, 레퍼런스는 특별한 경우가 아닌 이상 메모리 상에서 공간을 차지하지 않는다.

따라서, 이러한 모순때문에 레퍼런스들의 배열을 정의하는 것은 언어 차원에서 금지되어 있는 것이다.

하지만, 그와 반대인 배열들의 레퍼런스가 불가능한 것은 아니다.

    #include <iostream>

    int main() {
        int arr[3] = {1, 2, 3};
        int(&ref)[3] = arr;

        ref[0] = 2;
        ref[1] = 3;
        ref[2] = 1;

        std::cout << arr[0] << arr[1] << arr[2] << std::endl;
        return 0;
    }

위의 코드에 따르면 ref가 arr을 참조하도록 하였는데, ref[0]부터 ref[2]가 각각 arr[0]부터 arr[2]의 레퍼런스가 된다. 포인터와는 다르게 배열의 레퍼런스의 경우 참조하기 위해서는 반드시 배열의 크기를 명시해야한다.

int (&ref)[3]이라면 반드시 크기가 3인 int배열의 레퍼런스가 되어야하고, int (&ref)[5]라면 반드시 크기가 5인 int배열의 레퍼런스가 되어야한다.

    int arr[3][2] = {1, 2, 3, 4, 5, 6};
    int (&ref)[3][2] = arr;

2차원 배열은 위의 코드처럼 작성하면 된다.

## 4. 레퍼런스를 리턴하는 함수

    int function() {
        int a = 2;
        return a;
    }

    int main() {
        int b = function();
        return 0;
    }

위의 코드에서, function 안에 정의된 a라는 변수의 값이 b에 **복사**된다.

function이 종료되고 나면 a는 메모리에서 사라지게 된다.

### 지역변수의 레퍼런스를 리턴

    int& function() {
        int a = 2;
        return a;
    }

    int main() {
        int b = function();
        b = 3;
        
        return 0;
    }

위의 코드를 컴파일한다면 경고가 나오고, 실제로 실행해보면 런타임 오류가 발생한다. function의 리턴 타입은 int&로 레퍼런스인데, 문제는 function안에 정의되어있는 a는 함수의 리턴과 함께 사라진다는 것이다.

    int b = function();

위의 문장은 사실상 밑의 코드와 같은 의미이다.

    int& ref = a;

    //a가 사라짐
    int b = ref; //말이 안됨

function이 레퍼런스를 리턴하면서 원래 참조하고 있던 변수가 사라져버리게 되어 오류가 발생하게 됩니다.

위와 같이 레퍼런스는 있는데 참조하던 값이 사라진 레퍼런스를 댕글링 레퍼런스 (Dangling Reference)라고 한다.

> 따라서 위처럼 **레퍼런스를 리턴하는 함수에서 지역 변수의 레퍼런스를 리턴하지 않도록 조심**해야 한다.

### 외부 변수의 레퍼런스를 리턴

    int& function(int& a) {
        a = 5;
        return a;
    }

    int main() {
        int b = 2;
        int c = function(b);
        
        return 0;
    }

댕글링 레퍼런스와의 차이점은 인자로 받은 레퍼런스를 그대로 리턴하고 있다는 점이다. function(b)를 실행한 시점에서 a는 main의 b를 참조하게 된다.따라서, function이 리턴한 참조자는 살아있는 변수인 b를 계속 참조하게된다.

위처럼 참조자를 리턴하는 경우의 장점은, 예를 들어 c언어에서 엄청나게 큰 구조체가 있을때, 해당 변수를 그냥 리턴하면 전체 복사가 발생해야해서 시간이 오래 걸리지만, 해당 구조체를 가리키는 포인터를 리턴한다면 주소값 복사로 빠르게 끝난다. 이와 마찬가지로, 레퍼런스를 리턴하게 된다면 **레퍼런스가 참조하는 타입의 크기와 상관없이 딱 한 번의 주소값 복사로 전달이 끝나게 된다**.

### 참조자가 아닌 값을 리턴하는 함수를 참조자로 받기

    int function() {
        int a = 5;
        return a;
    }

    int main() {
        int& c = function();

        return 0;
    }

위의 코드를 컴파일하면 오류가 발생한다. 오류가 발생하는 이유는, 함수의 리턴값은 해당 문장이 끝난 후 바로 사라지는 값인데, 이의 참조자를 만들게 된다면 바로 다음에 댕글링 레퍼런스가 되어버리기 때문이다.

    #include <iostream>

    int function() {
        int a = 5;
        return a;
    }

    int main() {
        const int& c = function();
        std::cout << "c: " << c << std::endl;

        return 0;
    }

위의 코드처럼 함수의 리턴값을 참조자로 받더라도 const 참조자로 받는다면 문제없이 컴파일된다.

원칙상 함수의 리턴값은 해당 문장이 끝나면 소멸되는 것이 정상이나, 예외적으로 **상수 레퍼런스로 리턴값을 받게되면 해당 리턴값의 생명이 연장된다**.