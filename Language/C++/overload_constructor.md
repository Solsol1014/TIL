# Overload & Constructor (오버로딩 & 생성자)
## 1. 오버로딩
C언어에서는 하나의 이름을 가지는 함수는 단 1개만 존재할 수 있었다. 하지만, Cpp에서는 같은 이름을 가진 함수가 여러개 존재해도 된다. Cpp에서 같은 이름의 함수를 호출했을 때 구분을 하는 방법은 함수를 호출하였을 때 사용하는 인자를 보고 구분을 하게 된다.

    /* 함수의 오버로딩 */
    #include <iostream>

    void print(int x) {
        std::cout << "int: " << x << std::endl;
    }

    void print(char x) {
        std::cout << "char: " << x << std::endl;
    }

    void print(double x) {
        std::cout << "double: " << x << std::endl;
    }

    int main() {
        int a = 1;
        char b = 'c';
        double c = 3.2f;

        print(a);
        print(b);
        print(c);

        return 0;
    }

위 코드를 보게 된다면 이름이 print인 함수가 3개가 정의가 되어있다. 고전적인 C컴파일러에서는 오류가 발생했겠지만, Cpp에서는 **함수의 이름이 같더라도 인자가 다르면 다른 함수**라고 판단하기 때문에 오류가 발생하지 않는다.