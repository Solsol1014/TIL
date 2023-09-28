# Cpp's Structure (Cpp의 문법구조)
## 1. 변수의 정의

    //변수의 정의
    #include <iostream>

    int main() {
        int i;
        char c;
        double d;
        float f;

        return 0;
    }

변수의 정의에 있어서는 C언어와 달라진 바가 없다. 

변수명 작성 규칙도 바뀐 것이 없다. C와 마찬가지로 알파벳과 _기호, 숫자등을 사용할 수 있다.

## 2. 배열과 포인터
배열과 포인터를 정의하는 방법도 C와 Cpp가 동일하다.

    int arr[10];
    int *parr = arr;

    int i;
    int *pi = &i;

위의 코드는 C와 Cpp 양쪽에서 모두 유효하다.

## 3. 반복문과 조건문

    #include <iostream>
    
    int main() {
        int i;

        for (i=0;i<10;i++) {
            std::cout << i << std::endl;
        }

        return 0;
    }

위처럼 Cpp에서도 for문의 사용은 똑같다.

다만, 기존의 C에서는 변수를 정의할 때 항상 소스 맨 윗부분에 선언을 하라고 했는데, Cpp에서는 변수를 사용하기 전까지 어느 위치에서건 변수를 선언할 수 있다.

예를들어, 

    /* 변수는 변수 사용 직전에 선언해도 된다 */
    #include <iostream>

    int main() {
        int sum = 0;

        for (int i=1;i<=10;i++) {
            sum += i;
        }

        std::cout << "합은 : " << sum << std::endl;
        return 0;
    }

이처럼 변수 사용 직전에 선언해도 상관없다.

while문 또한 C에서의 사용 방법과 똑같다.

    /* while문 */
    #include <iostream>

    int main() {
        int i = 1;
        int sum = 0;

        while (i <= 10) {
            sum += i;
            i++;
        }

        std::cout << "합은: " << sum << std::endl;

        return 0;
    }

if - else문 또한 C와 동일한 문법 구조를 가지고 있다.

    /* if-else 문 예제 */
    #include <iostream>

    int main() {
        int lucky_number = 3;
        
        std::cout << "내 비밀 수를 맞춰보세요 - " << std::endl;

        int user_input; //사용자 입력

        while(1) {
            std::cout << "입력: ";
            std::cin >> user_input;
            
            if (lucky_number == user_input) {
                std::cout << "맞추셨습니다" << std::endl;
                break;
            }
            else {
                std::cout << "다시 생각해보세요" << std::endl;
            }
        }

        return 0;
    }

switch문 또한 C와 동일한 문법 구조를 가지고 있다.
