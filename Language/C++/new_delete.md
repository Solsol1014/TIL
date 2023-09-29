# New & Delete
C언어에서는 malloc과 free 함수를 지원하여 힙 상에서의 메모리 할당을 지원하였다. Cpp에서도 이를 사용할 수 있으나, 언어 차원에서 지원하는 함수로 new와 delete가 있다. new는 malloc과 대응되고, delete는 free와 대응이 된다.

    /* new와 delete의 사용 */
    #include <iostream>

    int main() {
        int* p = new int;
        *p = 10;

        std::cout << *p << std::endl;

        delete p;
        
        return 0;
    }

위의 코드 내용을 컴파일하면 int영역이 할당되어 10이 출력됨을 알 수 있다.

위의 코드처럼 new를 사용하는 방법은

    T* pointer = new T;

와 같다.

할당된 공간을 해제하기 위해서는 위의 코드처럼

    delete p;

를 하면 된다.

## new로 배열 할당하기

    /* new로 배열 할당하기 */

    #include <iostream>

    int main() {
        int arr_size;
        std::cout << "array size: ";
        std::cin >> arr_size;
        int *list = new int[arr_size];

        for (int i = 0;i<arr_size;i++) {
            std::cin >> list[i];
        }

        for (int i = 0;i<arr_size;i++) {
            std::cout << i << "th element of list : " << list[i] << std::endl;
        }

        delete[] list;

        return 0;
    }

### new로 배열 할당
위의 코드는 new로 배열을 할당하는 코드인데, 먼저 배열의 크기를 잡을 arr_size라는 변수를 정의하고 그 값을 입력받는다. 그리고 list에 new를 이용하여 크기가 arr_size인 int배열을 생성한다. 배열을 new로 할당할 때에는 []을 이용해 배열의 크기를 넣어주면 된다.

    T* pointer = new T[size];

T를 임의의 타입이라고 할 때, 위와 같이 코드를 작성하면 된다.

### 변수의 범위
C++에서는 변수를 아무데서나 정의할 수 있다고 했는데, 이 때, 변수는 그 변수를 선언한 중괄호를 빠져나갈 때 소멸하게 된다.

또한, 중괄호 밖에서 특정 변수를 선언한 이후, 중괄호 안에서 똑같은 이름의 변수를 선언하게 된다면, 컴파일러는 해당 변수를 가장 가까운 범위인 안쪽 중괄호 안에서 선언한 변수 값을 가져오게 된다.

위의 코드에서 for문 안쪽에서 int i를 선언하였는데, 이는 for문 안쪽에서 선언된 것이라고 정의되어 for문을 빠져나갈때 해당 i가 소멸하게 된다. 이렇게, for문 초기식에서 i를 정의하게 된다면 밖에서 i를 다른 용도로 사용했더라도 for문 안쪽에서는 i가 카운터로 사용되기 때문에 오류가 발생할 가능성이 낮아지게 된다.

### delete로 배열 해제
앞에서 new []를 이용하여 할당을 하였으므로, 해제할 때에는 delete[]를 통하여 해제를 하게 된다. new - delete가 짝을 이루고, new[] - delete[]가 짝을 이루게 되는 것이다.