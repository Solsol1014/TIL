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

