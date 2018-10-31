#### 6.4

```c++

long long fact(int n) {
    assert(n >= 0);

    if (0 == n || 1 == n)
        return 1;

    long long res = 1;
    while (n) {
        res *= n--;
    }

    return res;
}

long long calc_fact() {
    int n;
    cin >> n;

    return fact(n);
}

```

#### 6.5

```c++

long long my_abs(int val) { //注意可能溢出
    long long res = val;
    return res > 0 ? res : -res;
}

```

#### 6.7

```c++

int func_exam() {
    static int x = 0;
    return x++;
}

```

#### 6.10

```c++

void my_swap(int* p1, int* p2) {
    int tmp = *p1;
    *p1 = *p2;
    *p2 = tmp;
}

```

#### 6.12

```c++

void my_swap(int& p1, int& p2) {
    int tmp = p1;
    p1 = p2;
    p2 = tmp;
}

```

#### 6.17

```c++

bool is_upper(const string& s) {
    for (auto c : s) {
        if (isupper(c))
            return true;
    }
    return false;
}

void str_to_lower(string& s) {
    for (auto& c : s) {
        c = tolower(c);
    }
}

```

#### 6.18

比较两个matrix类
`bool compare(matrix& m1, matrix& m2);`

改变vector容器中指定位置的值
`vector<int>::iterator change_val(int, vector<int>::iterator);`

#### 6.21

```c++

int func_test_6_21(int lh, int* rh){
    assert(rh != nullptr);

    return lh > *rh ? lh : *rh;
}

```

#### 6.22

```c++

void change_points(int* &lh, int* &rh) {
    int* tmp = lh;
    lh = rh;
    rh = tmp;
}

```

#### 6.23

```c++

void print(int x) {
    cout << x << endl;
}

void print(const int* begin, const int* end) {
    while (begin != end) {
        cout << *begin++ << " ";
    }
    cout << endl;
}

```

#### 6.25

```c++

void print_main_param1(int n, char* argv[]) {
    string s;
    for (int i = 1; i < n; ++i) {
        s += argv[i];
    }
    cout << s << endl;
}

```

#### 6.26

```c++

void print_main_param2(int n, char* argv[]) {
    for (int i = 1; i < n; ++i) {
        cout << argv[i] << endl;
    }
}

```

#### 6.27

```c++

void sum_for_initializer_list(initializer_list<int> il) {
    long long res = 0;
    for (const auto& i : il) {
        res += i;
    }
    cout << res << endl;
}

```

#### 6.27

```c++

void vec_rec(const vector<int>& v) { //因为函数中有局部静态变量，因而不能连续调用多次，或者输出多个vector
    static decltype(v.size()) n = 0;
    if (n == v.size())
        return;

    cout << v[n++] << " ";
    vec_rec(v);
}

void vec_rec(const vector<int>& v, decltype(v.size()) n) { //添加一个参数即可解决上面的问题
    if (n == v.size())
        return;

    cout << v[n] << " ";
    vec_rec(v, ++n);
}

```

#### 6.36

```c++
string(&func1())[10];
```

----

##### 关于本题的思考

`string (&func1())[10];`
注意：声明数组引用格式 
`int(&a)[10]`
声明数组指针
`int(*a)[10]`

接着，声明返回数组引用的函数
`int (&fun(int i))[10]`
类似的，声明返回数组指针的函数
`int (*fun(int i))[10]`

> 有一个特别需要注意的问题，当需要查看变量或声明的类型时，通常使用的typeid函数并不是所有时候都正确显示结果。
> 规格上明确表明，**使用typeid就像给函数模板传递参数**。因此，const、引用、volatile都会被忽略

```c++

cout << typeid(volatile int).name() << endl;  //int
cout << typeid(int&).name() << endl;         //int
cout << typeid(const int).name() << endl;    //int

```

> 如果想要正确显示结果，必须使用boost库提供的type_id_with_cvr函数

```c++
#include<boost\type_index.hpp>
using boost::typeindex::type_id_with_cvr;

cout << type_id_with_cvr<volatile int>().pretty_name() << endl;  //volatile int
cout << type_id_with_cvr<int&>().pretty_name() << endl;          //int &
cout << type_id_with_cvr<const int>().pretty_name() << endl;     //const int

```

最后一点，使用typeid或type_id_with_cvr显示类型时，遇到string类会发现结果非常复杂，不易查看。原因在于string本身不是一个类名，而是一个typedef。输出结果会显示完整类（模板）名

```c++
typedef basic_string<char, char_traits<char>, allocator<char> > string; //vs2015
```

#### 6.37

```c++
// 类型别名
//typedef string str10[10];
using str10 = string[10];
str10& func2();

// 尾置声明
auto func3()->string(&)[10];

// decltype
string s_10[10];
decltype(s_10)& func4();
```

**最简单的是类型别名中using方式，最时髦的是尾置声明，最差的是类型别名中原c语言方式，最不实用的是decltype方式，最值得期待的是C++14提供的auto自动推导返回值方式。**

到了c++11,已经有3种方式声明返回数组的指针：

1. 类型别名。其中原c语言方式，非常晦涩；如果使用c++11提供的using，清楚许多
2. 尾置返回类型，最清晰的方式了
3. decltype推导，这个需要有一个与返回类型相同的已声明的数组变量。使用场景受限，当没有现成可用的变量时无法使用。特别注意：不要认为可以直接将函数体中最后返回值用在decltype中，编译器会因为变量未定义而报错。不过可以结合第2中方式使用
4. c++14可以直接用auto推导函数返回值

#### 6.38

```c++
int odd[] = {};
int even[] = {};
decltype(odd)& arrPtr(int i)
{
    return (i % 2) ? odd: even;
}
```

#### 6.42

```c++
string make_plural(size_t ctr, const string& word, const string &ending = "s")
{
    return (ctr > 1) ? word + ending: word;
}

cout << ch6::make_plural(1, "success") << "\n";
cout << ch6::make_plural(1, "failure") << endl;
cout << ch6::make_plural(2, "success", "es") << "\n";
cout << ch6::make_plural(2, "failure") << endl;
```

#### 6.44

```c++
inline bool isShorter(const string &s1, const string &s2)
{
    return s1.size() < s2.size();
}
```

#### 6.47

```c++
void debug_vec_rec(vector<int> v) {
    if (v.empty())
        return;

    cout << "vector size = " << sizeof(v) << ",";
    cout << __func__ << " ";
    cout << v[0] << endl;
    v.erase(v.begin());
    debug_vec_rec(v);
}
```

#### 6.50

```c++
void f() { cout << "f()" << endl; }
void f(int) { cout << "f(int)" << endl; }
void f(int, int){ cout << "f(int, int)" << endl; }
void f(double, double =3.14){ cout << "f(double, double =3.14)" << endl; }
```

#### 6.54

```c++
int func_54(int, int);
vector<decltype(func_54)*> vf;

typedef int f1(int, int);
vector<f1*> v1;
typedef int(*f1_1)(int, int);
vector<f1_1> v1_1;

using f2 = int(*)(int, int);
using f2_1 = int(int, int);
vector<f2> v2;
vector<f2_1*> v2_1;
```

#### 6.55

```c++
int int_add(int x, int y) { return x + y; } //可能溢出，下面几个类似（与本习题无关）
int int_minus(int x, int y) { return x - y; }
int int_multi(int x, int y) { return x * y; }
int int_div(int x, int y) { return x / y; }

vector<decltype(func_54)*> v_2i = { int_add, int_minus, int_multi, int_div };
```

#### 6.56

```c++
void test_p() {
    int x = 10;
    int y = 5;
    cout << v_2i[0](x, y) << "\n";
    cout << v_2i[1](x, y) << "\n";
    cout << v_2i[2](x, y) << "\n";
    cout << v_2i[3](x, y) << endl;
}
```