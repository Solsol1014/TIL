# Static
## 1. Static 멤버 변수
총 만들어지는 객체의 수를 알아내기 위한 코드를 짠다고 생각했을 때, 가장 단순한 두 방법은 아래와 같을 것이다.
1. 어떠한 배열에 객체들을 보관해놓고, 생성된 객체의 개수를 모두 센다.
2. 어떤 변수를 만들어서 객체 생성시에 1을 추가하고, 소멸시에 1을 뺀다.

첫번째 방법의 경우, 크기가 자유자재로 변할 수 있는 배열을 만들어야 한다는 문제점이 있고, 두번째의 경우 해당 값을 다른 함수에서도 이용하기 위해 계속 인자로 전달해줘야 하는 번거로움이 존재한다.

전역변수로 만들 수 있겠지만, 전역 변수의 경우 프로젝트의 크기가 커질수록 개발자의 실수로 인해 서로 겹쳐서 오류가 날 가능성이 커져 반드시 필요한 경우가 아니면 사용하지 않는다.

C++에서는 전역변수같지만, 클래스 하나에만 종속되는 변수를 지원하는데, 이가 바로 **static 멤버 변수**이다.

C언어에서 함수의 static 변수가 함수가 종료될 때 소멸되는 것이 아니라 프로그램이 종료될 때 소멸되는 것처럼, 어떤 클래스의 static 멤버 변수의 경우 객체가 소멸될 때 소멸되는 것이 아닌, 프로그램이 종료될 때 소멸되게 된다.

또한, static 멤버 변수의 경우, 클래스의 모든 객체들이 공유하는 변수로써, 각 객체 별로 따로 존재하는 것이 아닌 모든 객체들이 '하나의' static 멤버 변수를 사용하게 된다.

    #include <iostream>

    class Marine {
        static int total_marine_num;

        int hp;
        int coord_x, coord_y;
        bool is_dead;

        const int default_damage;

        public:
        Marine();
        Marine(int x, int y);
        Marine(int x, int y, int default_damage);

        int attack();
        void be_attacked(int damage_earn);
        void move(int x, int y);

        void show_status();

        ~Marine() {total_marine_num--;}
    };

    int Marine::total_marine_num = 0;

    Marine::Marine() : hp(50), coord_x(0), coord_y(0), default_damage(5), is_dead(false) {
        total_marine_num++;
    }

    Marine::Marine(int x, int y) : coord_x(x), coord_y(y), hp(50), default_damage(5), is_dead(false) {
        total_marine_num++;
    }

    Marine::Marine(int x, int y, int default_damage) : coord_x(x), coord_y(y), default_damage(default_damage), hp(50), is_dead(false) {
        total_marine_num++;
    }

    void Marine::move(int x, int y) {
        coord_x = x;
        coord_y = y;
    }

    int Marine::attack() {
        return default_damage;
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
        std::cout << "Total Marines : " << total_marine_num << std::endl;
    }

    void create_marine() {
        Marine marine3(10, 10, 4);
        marine3.show_status();
    }

    int main() {
        Marine marine1(2, 3, 5);
        marine1.show_status();

        Marine marine2(3, 5, 10);
        marine2.show_status();

        create_marine();

        std::cout << std::endl << "Marine1 attacks Marine2" << std::endl;
        marine2.be_attacked(marine1.attack());

        marine1.show_status();
        marine2.show_status();

        return 0;
    }

위의 코드에서 static 변수를 정의하였다. 모든 전역 및 static 변수들은 정의와 동시에 값이 자동으로 0으로 초기화 되기 때문에 굳이 따로 초기화할 필요는 없으나, 클래스 static 변수들의 경우 위 코드와 같은 방법으로 초기화한다.

클래스 내부에서 선언과 동시에 초기화해도 되지 않냐고 할 수 있지만, 멤버 변수들을 클래스 내부에서 초기화시키지 못하는 것처럼 static 변수 역시 클래스 내부에서 초기화하는 것은 불가능하다.

클래스 내부에서 초기화시킬 수 있는 유일한 경우는 const static 변수일 때만 가능하다.

위의 코드에서 create_marine()함수를 통해 marine3를 생성함으로써 총 개수가 3이 되는데, 해당 marine3은 create_marine함수를 빠져나가면서 소멸되므로, 이후 show_status()함수에서는 다시 2개로 출력되게 된다.

## 2. Static 함수
클래스 안에는 static 함수도 정의할 수 있다. static 변수가 특정 객체에 종속되는 것이 아니라 클래스 자체에 하나 존재하는 것처럼, static 함수 역시 클래스 전체에 하나 존재하는 함수이다.

static이 아닌 함수들의 경우 객체를 만들어야지만 함수들을 호출할 수 있지만, static함수의 경우 객체가 없어도 그냥 클래스 자체에서 호출할 수 있게 된다.

    #include <iostream>

    class Marine {
        static int total_marine_num;

        int hp;
        int coord_x, coord_y;
        bool is_dead;

        const int default_damage;

        public:
        Marine();
        Marine(int x, int y);
        Marine(int x, int y, int default_damage);

        int attack();
        void be_attacked(int damage_earn);
        void move(int x, int y);

        void show_status();

        static void show_total_marine();

        ~Marine() {total_marine_num--;}
    };

    int Marine::total_marine_num = 0;

    void Marine::show_total_marine() {
        std::cout << "Total Marines : " << total_marine_num << std::endl;
    }

    Marine::Marine() : hp(50), coord_x(0), coord_y(0), default_damage(5), is_dead(false) {
        total_marine_num++;
    }

    Marine::Marine(int x, int y) : coord_x(x), coord_y(y), hp(50), default_damage(5), is_dead(false) {
        total_marine_num++;
    }

    Marine::Marine(int x, int y, int default_damage) : coord_x(x), coord_y(y), default_damage(default_damage), hp(50), is_dead(false) {
        total_marine_num++;
    }

    void Marine::move(int x, int y) {
        coord_x = x;
        coord_y = y;
    }

    int Marine::attack() {
        return default_damage;
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
        std::cout << "Total Marines : " << total_marine_num << std::endl;
    }

    void create_marine() {
        Marine marine3(10, 10, 4);
        //marine3.show_status();
        Marine::show_total_marine();
    }

    int main() {
        Marine marine1(2, 3, 5);
        //marine1.show_status();
        Marine::show_total_marine();

        Marine marine2(3, 5, 10);
        //marine2.show_status();
        Marine::show_total_marine();

        create_marine();

        std::cout << std::endl << "Marine1 attacks Marine2" << std::endl;
        marine2.be_attacked(marine1.attack());

        marine1.show_status();
        marine2.show_status();

        return 0;
    }

static 함수는 말했다시피 어떤 객체에 종속되는 것이 아니라 클래스에 종속되는 것으로, 따라서 이를 호출하는 방법도 (객체).(멤버함수)가 아니라, 위의 코드와 같이 (클래스)::(static 함수)형식으로 호출하게 된다.

어떠한 객체도 이 함수를 소유하고 있지 않기에 static함수 내에서는 클래스의 static 변수만을 이용 가능하다.