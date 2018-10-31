#### 7.3

```c++
struct Sales_data {
    string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;

    string bookNo;
    unsigned units_sole = 0;
    double revenue = 0.0;
};

Sales_data& Sales_data::combine(const Sales_data& rhs) {
    units_sole += rhs.units_sole;
    revenue += rhs.revenue;
    return *this;
}

double Sales_data::avg_price() const {
    if (units_sole)
        return revenue / units_sole;
    else
        return 0;
}
```

#### 7.4

```c++
struct Persion {
    string get_name()const { return name; }
    string get_address()const { return address; }

    string name;
    string address;
};
```

#### 7.6

```c++
Sales_data add(const Sales_data& lhs, const Sales_data& rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
istream& read(istream& is, Sales_data& item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sole >> price;
    item.revenue = price * item.units_sole;
    return is;
}
ostream& print(ostream& os, const Sales_data& item)
{
    os << item.isbn() << " " << item.avg_price << " "
        << item.units_sole << " " << item.revenue;
    return os;
}
```

#### 7.9

```c++
istream& read(istream& is, Persion& item)
{
    is >> item.name >> item.address;
    return is;
}
ostream& print(ostream& os, const Persion& item)
{
    os << item.name << ":" << item.address;
    return os;
}
```

#### 7.11

```c++
struct Sales_data {
    Sales_data() = default;
    Sales_data(const string& s) :bookNo(s) {}
    Sales_data(const string& s, unsigned n, double price) :bookNo(s), units_sole(n), revenue(n*price) {}
    Sales_data(istream& is) {
        read(is, *this);
    }

    string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;

    string bookNo;
    unsigned units_sole = 0;
    double revenue = 0.0;
};
```

#### 7.13

```c++
void program1() {
    Sales_data total(cin);
    if (cin) {
        do {
            Sales_data trans(cin);
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(cout, total);
                total = trans;
            }
        } while (cin);
        print(cout, total) << endl;
    }
    else {
        cerr << "No data?!" << endl;
    }
}
```

#### 7.14

```c++
Sales_data::Sales_data() {
    bookNo = "";
    units_sole = 0;
    revenue = 0.0;
}
```

#### 7.15

```c++
struct Persion {
    Persion::Persion(const string& s_n, const string& s_a):name(s_n), address(s_a){}
    string get_name()const { return name; }
    string get_address()const { return address; }

    string name;
    string address;
};
```

#### 7.20

```c++
class Sales_data {
public:
    Sales_data() = default;
    Sales_data(const string& s) :bookNo(s) {}
    Sales_data(const string& s, unsigned n, double price) :bookNo(s), units_sole(n), revenue(n*price) {}
    Sales_data(istream& is) {
        read(is, *this);
    }

    string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
private:
    string bookNo;
    unsigned units_sole = 0;
    double revenue = 0.0;
};
```

#### 7.24

```c++
class Screen {
public:
    typedef string::size_type pos;
public:
    Screen() = default;
    Screen(pos ht, pos wd):height(ht), width(wd), contents(ht*wd, ' '){}
    Screen(pos ht, pos wd, char c)
        :cursor(0), width(wd), height(ht), contents(ht*wd, c) {}
    char get() const { return contents[cursor]; }
    inline char get(pos ht, pos wd) const;
    Screen& move(pos r, pos c);
private:
    pos cursor;
    pos height;
    pos width;
    string contents;
};
inline char Screen::get(pos r, pos c) const {
    pos row = r*width;
    return contents[row + c];
}
inline Screen& Screen::move(pos r, pos c) {
    pos row = r*width;
    cursor = row + c;
    return *this;
}
```

#### 7.27

```c++
class Screen {
public:
    typedef string::size_type pos;
public:
    Screen() = default;
    Screen(pos ht, pos wd):height(ht), width(wd), contents(ht*wd, ' '){}
    Screen(pos ht, pos wd, char c)
        :cursor(0), width(wd), height(ht), contents(ht*wd, c) {}
    char get() const { return contents[cursor]; }
    inline char get(pos ht, pos wd) const;
    Screen& move(pos r, pos c);
    const Screen& display(ostream& os)const {
        to_display(os);
        return *this;
    }
    Screen& display(ostream& os) {
        to_display(os);
        return *this;
    }
    Screen& set(char c);
    Screen& set(pos r, pos col, char c);
private:
    void to_display(ostream& os) const;
private:
    pos cursor;
    pos height;
    pos width;
    string contents;
};

inline char Screen::get(pos r, pos c) const {
    pos row = r*width;
    return contents[row + c];
}

inline Screen& Screen::move(pos r, pos c) {
    pos row = r*width;
    cursor = row + c;
    return *this;
}

inline Screen& Screen::set(char c) {
    contents[cursor] = c;
    return *this;
}

inline Screen& Screen::set(pos r, pos col, char c) {
    contents[r*width + col] = c;
    return *this;
}

void Screen::to_display(ostream& os) const {
    for (pos i = 0; i != contents.size(); ++i) {
        if (i && !(i%width))
            os << "\n";
        os << contents[i];
    }
}
```

#### 7.31

```c++
class Y;  //必须有这个类前置声明，不然类X中的成员变量未定义
class X {
private:
    Y* y;
};
class Y {
private:
    X x;
};
```

#### 7.32

本题没有将类声明和定义分开，因而在没有头文件包含的情况下用到类前置声明，保证编译通过

```c++
class Screen; //前置声明
class Window_mgr { //要先定义类
public:
    void clear(Screen& s);
};

class Screen {
    friend void Window_mgr::clear(Screen& s); //再声明友元
public:
    typedef string::size_type pos;
public:
    Screen() = default;
    Screen(pos ht, pos wd) :height(ht), width(wd), contents(ht*wd, ' ') {}
    Screen(pos ht, pos wd, char c)
        :cursor(0), width(wd), height(ht), contents(ht*wd, c) {}
    char get() const { return contents[cursor]; }
    inline char get(pos ht, pos wd) const;
    Screen& move(pos r, pos c);
    const Screen& display(ostream& os)const {
        to_display(os);
        return *this;
    }
    Screen& display(ostream& os) {
        to_display(os);
        return *this;
    }
    Screen& set(char c);
    Screen& set(pos r, pos col, char c);
private:
    void to_display(ostream& os) const;
private:
    pos cursor;
    pos height;
    pos width;
    string contents;
};

inline char Screen::get(pos r, pos c) const {
    pos row = r*width;
    return contents[row + c];
}

inline Screen& Screen::move(pos r, pos c) {
    pos row = r*width;
    cursor = row + c;
    return *this;
}

inline Screen& Screen::set(char c) {
    contents[cursor] = c;
    return *this;
}

inline Screen& Screen::set(pos r, pos col, char c) {
    contents[r*width + col] = c;
    return *this;
}

void Screen::to_display(ostream& os) const {
    for (pos i = 0; i != contents.size(); ++i) {
        if (i && !(i%width))
            os << "\n";
        os << contents[i];
    }
}

void Window_mgr::clear(Screen& s){ //最后定义友元函数
    s.contents = string(s.height*s.width, ' ');
}
```

#### 7.41

```c++
class Screen {
public:
    typedef string::size_type pos;
public:
    Screen() :Screen(0, 0) { cout << "Screen()" << endl; }
    Screen(pos ht, pos wd) :Screen(ht, wd, ' ') { cout << "Screen(pos ht, pos wd)" << endl; }
    Screen(pos ht, pos wd, char c)
        :cursor(0), width(wd), height(ht), contents(ht*wd, c) {
        cout << "Screen(pos ht, pos wd, char c)" << endl;
    }
    char get() const { return contents[cursor]; }
    inline char get(pos ht, pos wd) const;
    Screen& move(pos r, pos c);
    const Screen& display(ostream& os)const {
        to_display(os);
        return *this;
    }
    Screen& display(ostream& os) {
        to_display(os);
        return *this;
    }
    Screen& set(char c);
    Screen& set(pos r, pos col, char c);
private:
    void to_display(ostream& os) const;
private:
    pos cursor;
    pos height;
    pos width;
    string contents;
};

inline char Screen::get(pos r, pos c) const {
    pos row = r*width;
    return contents[row + c];
}

inline Screen& Screen::move(pos r, pos c) {
    pos row = r*width;
    cursor = row + c;
    return *this;
}

inline Screen& Screen::set(char c) {
    contents[cursor] = c;
    return *this;
}

inline Screen& Screen::set(pos r, pos col, char c) {
    contents[r*width + col] = c;
    return *this;
}

void Screen::to_display(ostream& os) const {
    for (pos i = 0; i != contents.size(); ++i) {
        if (i && !(i%width))
            os << "\n";
        os << contents[i];
    }
}

// 测试
ch7::Screen s1;
ch7::Screen s2(2, 3);
ch7::Screen s3(2, 3, 'o');
```

执行结果
> Screen(pos ht, pos wd, char c)
Screen(pos ht, pos wd)
Screen()
Screen(pos ht, pos wd, char c)
Screen(pos ht, pos wd)
Screen(pos ht, pos wd, char c)

#### 7.43

```c++
class NoDefault {
public:
    NoDefault(int){}
};

class C {
public:
    C() :nd(0) {}
private:
    NoDefault nd;
};
```

#### 7.49

```c++
class Sales_data {
public:
    Sales_data() = default;
    Sales_data(const string& s) :bookNo(s) {}
    Sales_data(const string& s, unsigned n, double price) :bookNo(s), units_sole(n), revenue(n*price) {}

    string isbn() const { return bookNo; }
    Sales_data& combine(Sales_data) { cout << "combine(Sales_data)"; return *this; }  //(1)
    Sales_data& combine(Sales_data&){ cout << "combine(Sales_data&)"; return *this; } //(2)
    Sales_data& combine(const Sales_data& s)const {                                   //(3)
        cout << "combine(const Sales_data&)const" << " ";
        cout << s.bookNo << endl;
        Sales_data* sd = new Sales_data();
        return *sd; }
private:
    string bookNo;
    unsigned units_sole = 0;
    double revenue = 0.0;
};

Sales_data sd = "world";
string s1 = "hello";
sd.combine(s1);
```

(1)和(3)可以调用，(2)不行，感觉从string->Sales_data->Sales_data&是不行的？

#### 7.58

```c++
//example.h
class Example {
public:
    static double rate;
    static const int vecSize;
    static std::vector<double> vec;
};

//example.cpp
double Example::rate = 6.5;
const int Example::vecSize = 20;
vector<double> Example::vec(vecSize); //::作用域操作符之后就进入其作用域了，vecSize便可以直接使用了
```