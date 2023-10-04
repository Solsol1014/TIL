# this

    //자기 자신을 가리키는 포인터 this
    #include <iostream>

    class Marine {
        static int total_marine_num;
        const static int i = 0;

        int hp;
        int coord_x, coord_y;
        bool is_dead;

        const int default_damage;

        public:
        Marine();
        Marine(int x, int y);
        Marine(int x, int y, int default_damage);

        int attack();
        Marine& be_attacked(int damage_earn);
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

    Marine& Marine::be_attacked(int damage_earn) {
        hp -= damage_earn;
        if (hp<=0)
            is_dead = true;

        return *this;
    }

    void Marine::show_status() {
        std::cout << "*** Marine ***" << std::endl;
        std::cout << "Location : (" << coord_x << ", " << coord_y << ")" << std::endl;
        std::cout << "HP : " << hp << std::endl;
        std::cout << "Total Marines : " << total_marine_num << std::endl;
    }

    int main() {
        Marine marine1(2, 3, 5);
        marine1.show_status();
        //Marine::show_total_marine();

        Marine marine2(3, 5, 10);
        marine2.show_status();
        //Marine::show_total_marine();

        std::cout << std::endl << "Marine1 attacks Marine2 twice" << std::endl;
        marine2.be_attacked(marine1.attack()).be_attacked(marine1.attack());

        marine1.show_status();
        marine2.show_status();

        return 0;
    }

## 1. this
this라는 것은 C++언어 차원에서 정의되어 있는 키워드이다. 이는 객체 자신을 가리키는 포인터의 역할을 한다. 따라서, be_attacked함수의 내용은

    Marine& Marine::be_attacked(int damage_earn) {
        this->hp -= damage_earn;
        if (this->hp <= 0)
            this->is_dead = true;

        return *this;
    }

와 동일한 의미가 된다.

실제로 모든 멤버 함수 내에서는 this 키워드가 정의되어 있으며 클래스 안에서 정의된 함수 중에서 this 키워드가 없는 함수는 static 함수 뿐이다.

## 2. 레퍼런스를 리턴하는 함수

    //레퍼런스를 리턴하는 함수
    #include <iostream>

    class A {
        int x;

        public:
        A(int c) : x(c) {}

        int& access_x() {
            return x;
        }

        int get_x() {
            return x;
        }

        void show_x() {
            std::cout << x << std::endl;
        }
    };

    int main() {
        A a(5);
        a.show_x();

        int& c = a.access_x();
        c = 4;
        a.show_x();

        int d = a.access_x();
        d = 3;
        a.show_x();

        // 아래는 오류
        // int& e = a.get_x();
        // e = 2;
        // a.show_x();

        int f = a.get_x();
        f = 1;
        a.show_x();

        return 0;
    }

위의 코드에서 access_x는 x의 레퍼런스를 리턴하게 되고, get_x는 값을 리턴하게 된다. c는 x의 레퍼런스를 받았기 때문에, c는 x의 별명이 된다. 즉, 

    int &c = x;

와 같은 말이라는 것이다. 따라서, c의 값을 바꾸는 것의 a의 x값을 바꾸는 것과 동일한 의미이므로 show_x를 실행하면 x의 값이 5에서 4로 바뀌어 있는 것이다.

따라서 Marine클래스의 be_attacked 함수에서 Marine& 타입을 리턴하는데, 위의 경우 *this를 리턴하고, this는 해당 함수를 호출한 객체 자신의 포인터를 의미하기 때문에, be_attacked함수는 자기 자신을 리턴하는 것이다.