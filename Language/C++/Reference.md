# Reference (참조자)
## 1. Reference의 도입
C언어에서는 어떠한 변수를 가리키고 싶을 댄 반드시 포인터를 사용해야했지만, C++에서는 또 다른 방식을 제공한다. 이가 바로 참조자(레퍼런스 - Reference)이다.

    #include <iostream>
    
    int main() {
        int a = 3;
        int& another_a = a;

        another_a = 5;
        std::cout << "a : " << a << std::endl;
        std::cout << "another_a : " << another_a << std::endl;

        return 0;
    }

위의 코드의 결과는 a:5, another_a:5로 나온다.   

int& another_a = a; 에서처럼 a의 참조자 another_a를 정의하기 위해서는 가리키고자 하는 타입 뒤에 &를 붙이면 된다.   
int형 변수의 참조자를 만들고 싶다면 int&를, double의 참조자를 만들려면 double&로 하면 된다. int* 과 같은 포인터 타입의 참조자를 만드려면 int*&로 쓰면 된다.   

참조자를 선언한다는 것은, another_a는 a의 또 다른 이름이라는 것이다. 따라서, 참조자에 어떠한 작업을 수행하던 원래의 변수에 작업을 하는 것과 마찬가지이다.   

## 2. Reference의 특징
참조자와 포인터는 상당히 유사한 개념이다. 다만, 레퍼런스와 포인터는 몇 가지 중요한 차이점이 있다.
### 1. 레퍼런스는 반드시 처음에 누구의 별명이 될 것인지 지정해야 한다.

    int& another_a;

와 같은 문장은 불가능하다. 반면에 포인터의 경우, 

    int* p;

는 전혀 문제가 없는 코드이다.
### 2. 레퍼런스가 한번 참조자가 되어버리면, 더이상 다른 변수를 참조할 수 없게 된다.

    int a = 10;
    int &another_a = a; //another_a 는 이제 a의 참조자이다

    int b = 3;
    another_a = b;

위의 코드에서 another_a = b; 는 another_a에게 b를 가리키라고 하는 것이 아닌, 그냥 another_a, 즉 a에게 b의 값을 대입하라는 의미이다.  
선언부 이후, another_a에 무언가를 하는 것은 a에 무언가를 하는 것과 동일하기 때문이다.
### 3. 레퍼런스는 메모리 상에 존재하지 않을 수도 있다.

    int a = 10;
    int* p = &a;

위의 코드에서 p는 메모리 상에서 8바이트를 차지하는 변수가 된다.

    int a = 10;
    int& another_a = a;

위의 코드에서 another_a를 위해 메모리 상에 공간을 할당할 필요가 없다. 
## 3. Reference의 활용
### 1. 함수 인자로 레퍼런스 받기

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
    }

성공적으로 컴파일 한다면, 5, 3이라는 결과가 나온다. 

    int change_val(int& p) {}

인자로 참조자를 받게했는데, int& p가 되는 이유는, p가 정의되는 순간은 change_val(number)로 호출할때이므로, 사실상 int& p = number가 실행되는 것이다.   

참조자 p에게 number로 선언을 하는데, 중요한 점은 포인터가 인자일 때와는 다르게 number 앞에 &를 붙일 필요가 없다.   

이후 change_val 안에서 p = 3; 이라 하는 것은 main함수의 number에 number = 3; 을 하는 것과 같은 작업이다.
### 2. 여러가지 참조자 예시들

    #include <iostream>

    int main() {
        int x;
        int& y = x;
        int& z = y;

        x = 1;
        std::cout << "x : " << x << " y : " << y << " z : " << z << std::endl;

        y = 2;
        std::cout << "x : " << x << " y : " << y << " z : " << z << std::endl;

        z = 3;
        std::cout << "x : " << x << " y : " << y << " z : " << z << std::endl;
    }

int x;, int&