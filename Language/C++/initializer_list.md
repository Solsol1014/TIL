# Initializer List (생성자 초기화 리스트)

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

    Marine::Marine() : hp(50), coord_x(0), coord_y(0), damage(5), is_dead(false) {}

    Marine::Marine(int x, int y) : coord_x(x), coord_y(y), hp(50), damage(5), is_dead(false) {}

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

위의 코드와 앞쪽에서 만든 Marine 클래스와 다른 점은 생성자가 다르다는 점 뿐이다. 위의 코드의 생성자를 보면 함수의 본체에는 아무것도 없고, 추가된 내용들이 기존의 생성자와 동일한 작업을 수행한다.

위와 같이 생성자 이름뒤에 멤버 변수들이 오는 것을 **초기화 리스트 (Initializer List)** 라고 부르며, 생성자 호출과 동시에 멤버 변수들을 초기화해주게 된다.

멤버 초기화 리스트의 일반적인 꼴을 아래와 같다.

    (생성자 이름) : var1(arg1), var2(arg2) {}

여기서 var은 클래스의 멤버 변수를 지칭하고, arg는 그 멤버 변수들을 초기화시킬 인자를 의미한다. 여기서, var과 arg의 이름이 같아도 되는데, 예를 들어,

    Marine::Marine(int coord_x, int coord_y) : coord_x(coord_x), coord_y(coord_y), hp(50), damage(5), is_dead(false) {}

위의 코드는 정상적으로 작동한다. 괄호 바깥쪽의 이름은 무조건 멤버 변수를 지칭하고, 괄호 안의 인자는 원칙상 생성자에서 인자로 받은 변수를 우선적으로 지칭하는 것이기 때문이다.

이전에 했던 방식대로 인자와 멤버 변수를 같은 이름으로 만든다면, 당연히 오류가 날 것이다.

## 초기화 리스트를 사용하는 이유
이전에 했던 방식이나 초기화 리스트나 하는 일이 똑같아 보이지만, 실제로는 약간의 차이가 존재한다. 초기화 리스트를 사용하게 되면 **생성과 초기화를 동시에** 하게 된다.

반면, 초기화 리스트를 사용하지 않는다면 **생성을 먼저하고 이후에 대입**을 수행하게 된다.

    int a = 10;
    -------------
    int a;
    a = 10;

즉, 위 두 개 코드의 차이와 비슷한 것인데, 만약 int 대신 클래스가 위치해있다면, 전자는 복사 생성자가 호출되는데, 후자의 경우 디폴트 생성자가 호출된 뒤에 대입이 수행한다는 의미와 비슷한 것이다.

위의 이유로 초기화 리스트를 사용하는 것이 조금 더 효율적인 작업이라는 사실을 알 수 있다. 이 뿐만 아니라, 반드시 *생성과 동시에 초기화되어야 하는 것들*이 존재하는데, 대표적으로 레퍼런스와 상수가 있다.

상수와 레퍼런스들은 모두 생성과 동시에 초기화가 되어야 하기 때문에, 만약 클래스 내부에 레퍼런스 변수나 상수를 넣고 싶다면 생성자에서 무조건 초기화 리스트를 사용해서 초기화시켜주어야만 한다.

위의 예시 코드에서 default damage를 아래와 같이 상수로 선언한다고 하면, 무조건 생성자에서 초기화 리스트를 사용해야 하는 것이다.

    const int default_damage;

위와 같이 중요한 값들을 상수로 처리하는 것은 협업과정에서 혹시라도 수정이 되지 않게 하기 때문에 매우 유용한 일이다.