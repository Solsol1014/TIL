# Copy Constructor & Destructor (복사 생성자 & 소멸자)
    #include <iostream>

    class Marine {
        int hp;
        int coord_x, coord_y;
        int damage;
        bool is_dead;

        public:
        Marine();
        Marine(int x, int y);

        int attack();
        void be_attacked(int damage_earn);
        void move(int x, int y);

        void show_status();
    };

    Marine::Marine() {
        hp = 50;
        coord_x = coord_y = 0;
        damage = 5;
        is_dead = false;
    }

    Marine::Marine(int x, int y) {
        coord_x = x;
        coord_y = y;
        hp = 50;
        damage = 5;
        is_dead = false;
    }

    void Marine::move(int x, int y) {
        coord_x = x;
        coord_y = y;
    }

    int Marine::attack() {
        return damage;
    }

    void Marine::be_attacked(int damage_earn) {
        hp -= damage_earn;
        if (hp<=0)
            is_dead = true;
    }

    void Marine::show_status() {
        std::cout << "*** Marine ***" << std::endl;
        std::cout << "Location : (" << coord_x << ", " << coord_y << ")" << std::endl;
        std::cout << "HP : " << hp << std::endl;
    }

    int main() {
        Marine marine1(2, 3);
        Marine marine2(3, 5);

        marine1.show_status();
        marine2.show_status();

        std::cout << std::endl << "Marine1 attacks Marine2" << std::endl;
        marine2.be_attacked(marine1.attack());

        marine1.show_status();
        marine2.show_status();

        return 0;
    }

## 1. 객체의 동적 할당
위 코드처럼 객체를 생성하면 사용자가 몇개의 객체를 만들지 정할 수도 없고, 그렇다고 수십개의 객체를 미리 만들 수도 없다. 이럴때는 해당 객체들을 배열로 정해버리면 된다.

    int main() {
        Marine* marines[100];

        marines[0] = new Marine(2, 3);
        marines[1] = new Marine(3, 5);

        marines[0]->show_status();
        marines[1]->show_status();

        std::cout << std::endl << "Marine1 attacks Marine2" << std::endl;
        
        marines[0]->be_attacked(marines[1]->attack());

        marines[0]->show_status();
        marines[1]->show_status();

        delete marines[0];
        delete marines[1];

        return 0;
    }

new와 malloc은 모두 동적으로 할당하지만, 다른 점이 있는데, 위 코드에서 볼 수 있다시피 new의 경우 객체를 동적으로 생성하면서 동시에 자동으로 생성자도 호출해준다는 점이다.

Marine들의 포인터를 가리키는 배열이기 때문에, 메소드를 호출할 때 .이 아니라 ->를 사용해줘야한다.

또한, 동적으로 할당한 메모리는 해제해주어야 하기 때문에 마지막에 delete로 동적으로 할당된 객체들을 해제해주어야 한다.

## 2. 소멸자
위에서 만든 Marine에 이름을 지정해줄려고 한다.

    #include <iostream>
    #include <string.h>

    class Marine {
        int hp;
        int coord_x, coord_y;
        int damage;
        bool is_dead;
        char* name;

        public:
        Marine();
        Marine(int x, int y);
        Marine(int x, int y, const char* marine_name);

        int attack();
        void be_attacked(int damage_earn);
        void move(int x, int y);

        void show_status();
    };

    Marine::Marine() {
        hp = 50;
        coord_x = coord_y = 0;
        damage = 5;
        is_dead = false;
        name = NULL;
    }

    Marine::Marine(int x, int y) {
        coord_x = x;
        coord_y = y;
        hp = 50;
        damage = 5;
        is_dead = false;
        name = NULL;
    }

    Marine::Marine(int x, int y, const char* marine_name) {
        name = new char[strlen(marine_name)+1];
        strcpy(name, marine_name);

        coord_x = x;
        coord_y = y;
        hp = 50;
        damage = 5;
        is_dead = false;
    }

    void Marine::move(int x, int y) {
        coord_x = x;
        coord_y = y;
    }

    int Marine::attack() {
        return damage;
    }

    void Marine::be_attacked(int damage_earn) {
        hp -= damage_earn;
        if (hp<=0)
            is_dead = true;
    }

    void Marine::show_status() {
        std::cout << "*** Marine : " << name << " ***" << std::endl;
        std::cout << "Location : (" << coord_x << ", " << coord_y << ")" << std::endl;
        std::cout << "HP : " << hp << std::endl;
    }

    int main() {
        Marine* marines[100];

        marines[0] = new Marine(2, 3, "Marine 2");
        marines[1] = new Marine(3, 5, "Marine 1");

        marines[0]->show_status();
        marines[1]->show_status();

        std::cout << std::endl << "Marine1 attacks Marine2" << std::endl;
        
        marines[0]->be_attacked(marines[1]->attack());

        marines[0]->show_status();
        marines[1]->show_status();

        delete marines[0];
        delete marines[1];

        return 0;
    }

위에서 만든 Marine클래스에 name이라는 이름을 저장할 수 있는 인스턴스 변수를 추가했다.

위 코드에서 name에 이름을 넣어줄 때, name을 동적으로 생성해서 문자열을 복사하였는데, 이에 대한 delete가 이루어지지 않았다. 이처럼 동적으로 할당한 메모리를 해제하지 않고 이가 계속 쌓인다면 **메모리 누수 (Memory Leak)** 가 발생하게 된다. 메모리 누수가 일어나게 되면, 프로그램이 비정상적으로 많은 메모리를 점유하게 되고, 성능 하락으로 이어지게 된다.

이와 같은 현상을 해결하기 위한 기능이 **소멸자 (Destructor)** 이다. 생성자가 객체가 생성될 때 자동으로 호출되듯, 소멸자는 객체가 소멸될 때 자동으로 호출되는 함수이다.

    Marine::~Marine() {
        std::cout << name << "'s destructor" << std::endl;
        if (name != NULL)
            delete[] name;
    }

위 코드처럼 생성자가 클래스 이름과 똑같이 생겼다면, 소멸자는 그 앞에 ~만 붙여주면 된다. 생성자와 다른 점은, 소멸자는 인자를 아무것도 가지지 않는다는 것이다.

소멸자가 하는 가장 흔한 역할은 객체가 동적으로 할당받은 메모리를 해제하는 일이라고 볼 수 있다. 그 외에도 쓰레드 사이에서 lock된 것을 푸는 기능 등의 역할을 수행하게 된다.

### 디폴트 소멸자
생성자를 따로 정의하지 않더라도 디폴트 생성자가 존재했던 것처럼, 소멸자도 **디폴트 소멸자 (Default Destructor)** 가 존재한다. 물론, 디폴트 소멸자 내부에서는 아무런 작업도 수행하지 않는다.

## 3. 복사 생성자
동일한 객체가 수십, 수백개가 있다면 각각의 객체를 일일히 생성자로 생성할 수도 있겠지만, 1개만 생성해놓고 나머지 객체를 복사 생성할 수도 있다.

    #include <string.h>
    #include <iostream>

    class Photon_Cannon {
        int hp, shield;
        int coord_x, coord_y;
        int damage;

        public:
        Photon_Cannon(int x, int y);
        Photon_Cannon(const Photon_Cannon& pc);

        void show_status();
    };

    Photon_Cannon::Photon_Cannon(const Photon_Cannon& pc) {
        std::cout << "Copy Constructor" << std::endl;
        hp = pc.hp;
        shield = pc.shield;
        coord_x = pc.coord_x;
        coord_y = pc.coord_y;
        damage = pc.damage;
    }

    Photon_Cannon::Photon_Cannon(int x, int y) {
        std::cout << "Constructor" << std::endl;
        hp = shield = 100;
        coord_x = x;
        coord_y = y;
        damage = 20;
    }

    void Photon_Cannon::show_status() {
        std::cout << "Photon Cannon" << std::endl;
        std::cout << "Location : (" << coord_x << ", " << coord_y << ")" << std::endl;
        std::cout << "HP : " << hp << std::endl;
    }

    int main() {
        Photon_Cannon pc1(3, 3);
        Photon_Cannon pc2(pc1);
        Photon_Cannon pc3 = pc2;

        pc1.show_status();
        pc2.show_status();

        return 0;
    }

위의 복사 생성자가 복사 생성자의 표준적인 정의라고 보면 된다.

    T(const T& a);

즉, 위의 코드처럼 정의되는 것이다. 다른 T의 객체 a를 상수 레퍼런스로 받는다는 의미이다. 여기서 a가 const이기 때문에 복사 생성자 내부에서는 a의 데이터를 변경할 수 없고, 오직 초기화되는 인스턴스 변수들에게 복사만 가능하게 된다.

함수 내부에서 받은 인자의 값을 변화시키는 일이 없다면 const를 붙이는 것이 좋다. 이렇게 하면 후에 발생할 수 있는 실수들을 효과적으로 막을 수 있기 때문이다.

    Photon_Cannon pc3 = pc2;

위 코드 역시 복사 생성자가 호출된다. C++ 컴파일러는 위 문장을 아래와 같이 해석한다.

    Photon_Cannon pc3(pc2);

이런식으로 코드를 짜면 사용자가 상당히 직관적이고 깔끔한 프로그래밍을 할 수 있다.

    Photon_Cannon pc3 = pc2;
    -----------------------
    Photon Cannon pc3;
    pc3 = pc2;

또한, 위와 아래는 엄연히 다른 의미의 문장이다. 위의 것은 말그대로 복사 생성자가 1번 호출되는 것이고, 아래 것은 그냥 생성자가 한번 호출되고, pc3 = pc2;라는 명령이 실행되는 것이다.

복사 생성자는 오직 '생성'시에 호출되는 것이다.

### 디폴트 복사 생성자
디폴트 생성자와 디폴트 소멸자처럼, C++ 컴파일러는 이미 **디폴트 복사 생성자 (Default copy constructor)** 를 지원하고 있다. 위 코드에서 복사 생성자를 지우고 실행하더라도 이전과 동일한 결과가 나타남을 알 수 있다. 디폴트 복사 생성자의 경우 기존의 디폴트 생성자와 소멸자가 하는 일이 아무 것도 없었던 것과 달리, 실제로 '복사'를 해준다.

따라서, 위와 같이 간단한 클래스의 경우 복사생성자를 굳이 써주지 않더라도 디폴트 복사 생성자만 이용해서 복사 생성을 쉽게 처리할 수 있다.

### 디폴트 복사 생성자의 한계
위의 포토캐논 클래스에 마린처럼 이름을 추가하고 디폴트 복사 생성자를 사용하게 된다면 런타임에러가 뜨게 된다. 디폴트 복사 생성자는 단순히 같은 주소값을 가리키는 것으로 복사를 수행한다. int와 같은 타입은 수 하나하나가 상수값이라 복사를 한 객체에서 값을 바꾸더라도 다른 상수값을 가리키는 거라 상관이 없지만, char*과 같은 포인터는 해당 포인터가 가리키는 주소를 복사하고, 한쪽에서 해당 주소의 메모리를 해제시키면 다른 객체에서 해제된 메모리에 접근하는 말도 안되는 일이 생긴다.

이러한 문제를 막으려면, 복사 생성자에서 name을 그대로 복사하지 말고 따로 다른 메모리에 동적할당을 해서 내용만 복사하면 된다. 이렇게 메모리를 새로 할당해서 내용을 복사하는 것을 **깊은 복사 (Deep Copy)** 라고 한다. 위처럼 단순히 주소값을 대입하는 것을 **얕은 복사 (Shallow Copy)** 라고 한다.

컴파일러가 생성하는 디폴트 복사 생성자의 경우 얕은 복사밖에 할 수 없으므로 위와 같이 깊은 복사가 필요한 경우에는 사용자가 직접 복사 생성자를 만들어야 한다.