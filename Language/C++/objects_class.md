# Objects & Class (객체&클래스)
절차지향언어 이후, 프로그램의 크기가 과거에 비해 크게 거대해지자 새로운 패러다임이 필요해졌고, 이때 나온 언어가 **객체지향언어 (Object oriented language)** 이다.

## 1. 객체란
**객체**란, 변수들과 참고자료(함수)들로 이루어진 소프트웨어 덩어리이다.

객체는 자기만의 정보를 나타내는 변수들과, 이를 가지고 어떠한 작업을 하는 함수들이 둘러싸고 있다고 보면 되는데, 이러한 객체의 변수나 함수들을 보통 **인스턴스 변수 (instance variable)**, **인스턴스 메소드 (instance method)** 라고 부른다.

함수가 둘러싸고 있다고 표현한 이유는 실제로 변수들이 외부로부터 보호되고 있기 때문이다. 다시 말해, 외부에서 인스턴스 변수를 직접 바꾸지 못하고 오로지 인스턴스 메소드를 통해서만 변경이 가능하다는 것이다. 이와 같이 외부에서 직접 인스턴스 변수의 값을 변경할 수 없고, 항상 인스턴스 메소드를 통해서 간접적으로 조절하는 것을 **캡슐화 (Encapsulation)** 라고 한다. 캡슐화를 하는 이유는, 객체가 내부적으로 어떻게 작동하는지 몰라도 사용할 수 있다라는 이유때문이다.

## 2. 클래스
클래스는 쉽게 말해 객체의 설계도라고 볼 수 있다. 객체의 설계도인 클래스를 통해서 실제 객체를 만들게 되는데, 이와 같이 클래스를 이용해서 만들어진 객체를 인스턴스(instance)라고 한다.

    #include <iostream>

    class Animal {
        private:
            int food;
            int weight;

        public:
            void set_animal(int _food, int _weight) {
                food = _food;
                weight = _weight;
            }

            void increase_food(int inc) {
                food += inc;
                weight += (inc/3);
            }

            void view_stat() {
                std::cout << "Food : " << food << std::endl;
                std::cout << "Weight : " << weight << std::endl;
            }
    };

    int main() {
        Animal animal;
        animal.set_animal(100, 50);
        animal.increase_food(30);

        animal.view_stat();

        return 0;
    }

위의 코드에서 main함수에서 Animal 클래스의 인스턴스를 생성한 방법을 살펴보면, 기존의 구조체에서 구조체 변수를 생성할 때와 동일하다는 것을 알 수 있다. 구조체의 경우 앞에 struct를 명시하거나 typedef를 사용해야 했지만, 클래스에서는 그러지 않아도 된다. Animal animal;을 통해 Animal클래스의 인스턴스인 animal을 만들게 된다.

위의 코드에서 Animal클래스에는 food, weight이라는 변수가 있고, set_animal, increase_food, view_stat이라는 함수들이 있는데, Animal클래스 상에서 이들을 지칭할 때 각각 **멤버 변수 (member variable)** 와 **멤버 함수 (member function)** 이라고 부른다. 즉, 인스턴스로 생성된 객체에서는 인스턴스 변수, 인스턴스 함수라고 부르고, 클래스 상에서는 멤버 변수와 멤버 함수라고 불리는 것이다.

멤버 변수들을 정의한 부분 맨 앞, private같은 키워드를 '접근 지시자'라고 하는데, 이는 외부에서 멤버들에게 접근을 할 수 있냐 없냐를 지시해주는 역할을 하게된다. private되고 있는 멤버들은 모두 자기 객체 안에서만 접근할 수 있고, 객체 외부에서는 접근할 수 없게 된다. 이와 반대로, public 키워드의 경우 외부에서 이용할 수 있게 된다.

참고로, 접근지시자 키워드를 명시하지 않았다면 기본적으로 private로 설정된다. 즉, 맨 위의 private 키워드를 지워도 상관이 없다.