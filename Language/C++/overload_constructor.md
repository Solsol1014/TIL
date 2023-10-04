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

위 코드를 보게 된다면 이름이 print인 함수가 3개가 정의가 되어있다. 고전적인 C컴파일러에서는 오류가 발생했겠지만, Cpp에서는 **함수의 이름이 같더라도 인자가 다르면 다른 함수**라고 판단하기 때문에 오류가 발생하지 않는다. a는 int, b는 char, c는 double타입인데, 각각 타입에 맞는 함수들이 호출된 것이다. c언어의 경우, 각 타입에 따라 함수의 이름을 제각각 다르게 만들어서 호출해주어야 했으나, C++에서는 컴파일러가 알아서 *적합한 인자를 가지는 함수*를 찾아서 호출해준다.

    /* 함수의 오버로딩2 */
    #include <iostream>

    void print(int x) {
        std::cout << "int : " << x << std::endl;
    }

    void print(double x) {
        std::cout << "double : " << x << std::endl;
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

위의 코드에서 char의 경우, 자기와 정확하게 일치하는 인자를 가지는 함수가 없기 때문에 *자신과 최대로 근접한 함수*를 찾게 된다.

### C++ 컴파일러에서 함수를 오버로딩하는 과정
1. 자신과 타입이 정확히 일치하는 함수를 찾는다.
2. 정확히 일치하는 타입이 없는 경우, 아래와 같은 형변환을 통해서 일치하는 함수를 찾아본다.
    * char, unsigned char, short는 int로 변환된다.
    * unsigned short는 int의 크기에 따라 int 혹은 unsigned int로 변환된다.
    * float은 double로 변환된다.
    * enum은 int로 변환된다.
3. 위와 같이 변환해도 일치하는 것이 없다는 아래의 좀 더 포괄적인 형변환을 통해 일치하는 함수를 찾는다.
    * 임의의 숫자(numeric) 타입은 다른 숫자 타입으로 변환된다. (ex -  float -> int)
    * enum도 임의의 숫자 타입으로 변환된다. (ex - enum -> double)
    * 0은 포인터 타입이나 숫자 타입으로 변환된 0은 포인터 타입이나 숫자 타입으로 변환된다.
    * 포인터는 void 포인터로 변환된다.
4. 유저 정의된 타입 변환으로 일치하는 것을 찾는다.

위의 과정을 통하더라도 일치하는 함수를 찾을 수 없거나, 같은 단계에서 두 개 이상이 일치하는 경우에는 **모호하다 (ambiguous)** 라고 판단해서 오류를 발생하게 된다.

위의 과정에 따라 위의 코드에서 char타입의 인자를 가진 print가 없기에 int로 변환되어 print(int x)가 호출되게 된 것이다.

### 모호한 오버로딩 (Ambiguous Overloading)

    //모호한 오버로딩
    #include <iostream>

    void print(int x) {
        std::cout << "int : " << x << std::endl;
    }

    void print(char x) {
        std::cout << "double : " << x << std::endl;
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

위의 소스를 컴파일하면 오류가 발생한다. 위의 코드에서는 int와 char을 인자로 받는 print 함수만 있기 때문에 double은 1단계에서 일치하는 것이 없다. 2단계에서도 double의 캐스팅에 관한 내용이 없기때문에 일치하는 것이 없고, 3단계에서는 '임의의 숫자 타입이 임의의 숫자 타입'으로 변환되어 생각되기 때문에 char로도, int로도 변환될 수 있게된다.

따라서, 같은 단계에 두 개 이상의 가능한 일치가 존재하므로 오류가 발생하게 된다.

## 2. 생성자

    #include <iostream>

    class Date {
        int year_;
        int month_;
        int day_;

        public:
        void SetDate(int year, int month, int day);
        void AddDay(int inc);
        void AddMonth(int inc);
        void AddYear(int inc);

        int GetCurrentMonthTotalDays(int year, int month);

        void ShowDate();
    };

    void Date::SetDate(int year, int month, int day) {
        year_ = year;
        month_ = month;
        day_ = day;
    }

    int Date::GetCurrentMonthTotalDays(int year, int month) {
        static int month_day[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

        if (month!=2) {
            return month_day[month-1];
        }
        else if (year % 4 ==0 && year % 100 != 0) {
            return 29;
        }
        else {
            return 28;
        }
    }

    void Date::AddDay(int inc) {
        while (true) {
            int current_month_total_days = GetCurrentMonthTotalDays(year_, month_);

            if (day_ + inc <= current_month_total_days) {
                day_ += inc;
                return;
            }
            else {
                inc -= (current_month_total_days - day_ + 1);
                day_ = 1;
                AddMonth(1);
            }
        }
    }

    void Date::AddMonth(int inc) {
        AddYear((inc+month_-1)/12);
        month_ = month_+inc%12;
        month_ = (month_==12?12:month_%12);
    }

    void Date::AddYear(int inc) {
        year_ += inc;
    }

    void Date::ShowDate() {
        std::cout << "오늘은 " << year_ << "년 " << month_ << "월 " << day_ << "일 입니다." << std::endl;
    }

    int main() {
        Date day;
        day.SetDate(2011, 3, 1);
        day.ShowDate();

        day.AddDay(30);
        day.ShowDate();

        day.AddDay(2000);
        day.ShowDate();
        
        day.SetDate(2012, 1, 31); //윤년
        day.AddDay(29);
        day.ShowDate();

        day.SetDate(2012, 8, 4);
        day.AddDay(2500);
        day.ShowDate();

        return 0;
    }

### 클래스 함수 정의
생성자에 대해 설명하기 전, 위의 코드에서 클래스 내부에는 함수의 정의만 나와있고, 함수 전체 몸통은 바깥쪽에 나와있다. 함수 앞에 Date::처럼 클래스의 이름을 붙여주게 되면 해당 함수가 "해당 클래스의 정의된 함수"라는 의미를 부여하게 된다.

클래스 내부에 함수를 쓸 경우 클래스 크기가 너무 길어져서 보기 좋지 않기 때문에, 보통 간단한 함수들을 제외하면 대부분의 함수들은 클래스 바깥에서 위와 같이 정의하게 된다.

### 생성자
main 함수를 살펴보면, day 인스턴스를 생성해서 SetDate로 초기화 한 이후에 다른 함수들을 호출하게 된다. 만약, SetDate로 초기화하지 않고 다른 함수를 호출하게 되면, 쓰레기 값이 출력되게 된다. 이처럼 인스턴스를 초기화해주는 작업을 도와주는 장치가 바로 **생성자 (constructor)** 이다.

    #include <iostream>

    class Date {
        int year_;
        int month_;
        int day_;

        public:
        void SetDate(int year, int month, int day);
        void AddDay(int inc);
        void AddMonth(int inc);
        void AddYear(int inc);

        int GetCurrentMonthTotalDays(int year, int month);

        void ShowDate();

        Date(int year, int month, int day) {
            year_ = year;
            month_ = month;
            day_ = day;
        }
    };

    //생략

    int main() {
        Date day(2011, 3, 1);
        day.ShowDate();

        day.AddYear(10);
        day.ShowDate();

        return 0;
    }

생성자는 기본적으로 "객체 생성 시 자동으로 호출되는 함수"라고 볼 수 있다. 이 때, 자동으로 호출되면서 객체를 초기화 해주는 역할을 담당하게 된다. 생성자는 다음과 같이 정의한다.

    ClassName(parameter) {}

위 코드의 경우, 

    Date(int year, int month, int day)

로 Date의 생성자를 정의한 것이다.

이렇게 정의가 된 생성자는 객체를 생성할 때 생성자에서 정의한 인자에 맞게 선언을 한다면 해당 정의된 생성자를 호출하며 객체를 만들 수 있게된다.

즉, 위의 코드와 같이 객체를 생성하면 day 객체를 만들면서 위에서 정의한 생성자를 호출한다는 의미가 된다.

생성자를 호출하는 방법에는 다른 방법또한 있는데,

    Date day(2011, 3, 1); //암시적 방법 (implicit)
    Date day = Date(2012, 3, 1); //명시적 방법 (explicit)

위의 명시적 방법이 해당 방법이다. 마치 함수를 호출하듯이 사용하는 것이 암시적 방법, 명시적으로 생성자를 호출한다는 것을 보여주는 것이 명시적 방법이다.

### 디폴트 생성자 (Default Constructor)
맨 처음에 단순히 SetDate함수를 이용해서 객체를 초기화하였을때 생성자를 명시하지 않았는데, 생성자 정의를 하지 않은 채 객체를 생성했을 때 과연 생성자가 호출될까. 답은 생성자가 호출된다이다. 여기서 호출이 되는 생성자가 바로 **디폴트 생성자 (Default Constructor)** 이다.

디폴트 생성자는 인자를 하나도 가지지 않는 생성자인데, 클래스에서 사용자가 어떠한 생성자도 명시적으로 정의하지 않았을 경우에 컴파일러가 자동으로 추가해주는 생성자이다.

    Date() {
        year_ = 2012;
        month_ = 8;
        day_ = 12;
    }

위의 코드처럼 직접 디폴트 생성자를 정의하는 것도 가능하다.

    Date day = Date();
    Date day2;

각각 명시적 방법과 암시적 방법을 이용해 디폴트 생성자로 객체를 생성하는 방법이다. 여기서 주의할 점은 위에서 인자가 있는 생성자에서 적용했던 것처럼 밑처럼 암시적 표현으로 객체를 생성하게 되면,

    Date day3();

해당 객체를 디폴트 생성자를 이용해서 초기화하는 것이 아니라, **리턴값이 Date이고 인자가 없는 함수 day3를 정의**한 것으로 인식한다.

이는 암시적 표현으로 객체를 선언할 때에 반드시 주의해두어야 한다.

#### 명시적으로 디폴트 생성자 사용하기
C++11 이전에는 디폴트 생성자를 사용하고 싶을 경우 그냥 생성자를 정의하지 않는 방법밖에 없었는데, 이 때문에 코드를 읽는 사용자 입장에서 개발자가 이를 빼먹은 것인지, 정말 디폴트 생성자를 사용하고 싶어서 그런 것인지 알 길이 없었다.

해서, C++11부터는 명시적으로 디폴트 생성자를 사용하도록 명시할 수 있다.

    class Test {
        public:
        Test() = default; //디폴트 생성자를 정의해라
    }

위의 코드처럼 생성자의 선언 뒤에 =default를 붙여준다면, Test의 디폴트 생성자를 정의하라고 컴파일러에게 명시적으로 알려줄 수 있다.

### 생성자 오버로딩
생성자 역시 함수이기 때문에 오버로딩이 적용될 수 있다.

    Date() {
        std::cout << "default constructor" << std::endl;
    }

    Date(int year, int month, int day) {
        std::cout << "3 parameters' constructor" << std::endl;
        year_ = year;
        month_ = month;
        day_ = day;
    }

위의 코드와 같이 생성자를 정의하면, 객체를 생성할 때에 인자에 따라 적절한 생성자가 호출된다.