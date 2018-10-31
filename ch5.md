## 5.3.2

### 练习

#### 5.9

```c++

unsigned int aCnt = 0;
unsigned int eCnt = 0;
unsigned int iCnt = 0;
unsigned int oCnt = 0;
unsigned int uCnt = 0;
char ch;
while (cin >> ch) {
    if (ch == 'a')
        ++aCnt;
    else if (ch == 'e')
        ++eCnt;
    else if (ch == 'i')
        ++iCnt;
    else if (ch == 'o')
        ++oCnt;
    else if (ch == 'u')
        ++uCnt;
}
cout << "numbers of a: " << aCnt << "\n"
     << "numbers of e: " << eCnt << "\n"
     << "numbers of i: " << iCnt << "\n"
     << "numbers of o: " << oCnt << "\n"
     << "numbers of u: " << uCnt << endl;

```

#### 5.10

```c++

unsigned int aCnt = 0;
unsigned int eCnt = 0;
unsigned int iCnt = 0;
unsigned int oCnt = 0;
unsigned int uCnt = 0;
char ch;
while (cin >> ch) {
    switch (ch)
    {
    case 'a':
    case 'A':
        ++aCnt;
        break;
    case 'e':
    case 'E':
        ++eCnt;
        break;
    case 'i':
    case 'I':
        ++iCnt;
        break;
    case 'o':
    case 'O':
        ++oCnt;
        break;
    case 'u':
    case 'U':
        ++uCnt;
        break;
    default:
        break;
    }
}
cout << "numbers of a: " << aCnt << "\n"
     << "numbers of e: " << eCnt << "\n"
     << "numbers of i: " << iCnt << "\n"
     << "numbers of o: " << oCnt << "\n"
     << "numbers of u: " << uCnt << endl;

```

#### 5.11

```c++

unsigned int aCnt = 0;
unsigned int eCnt = 0;
unsigned int iCnt = 0;
unsigned int oCnt = 0;
unsigned int uCnt = 0;
unsigned int symbolsCnt = 0;
char ch;
while (cin.get(ch)) {
    switch (ch)
    {
    case 'a':
    case 'A':
        ++aCnt;
        break;
    case 'e':
    case 'E':
        ++eCnt;
        break;
    case 'i':
    case 'I':
        ++iCnt;
        break;
    case 'o':
    case 'O':
        ++oCnt;
        break;
    case 'u':
    case 'U':
        ++uCnt;
        break;
    case ' ':
    case '\t':
    case '\n':
        ++symbolsCnt;
        break;
    default:
        break;
    }
}
cout << "numbers of a: " << aCnt << "\n"
     << "numbers of e: " << eCnt << "\n"
     << "numbers of i: " << iCnt << "\n"
     << "numbers of o: " << oCnt << "\n"
     << "numbers of u: " << uCnt << "\n"
     << "numbers of symbolsCnt: " << symbolsCnt << endl;

```

#### 5.12

```c++

unsigned int ffCnt = 0;
unsigned int flCnt = 0;
unsigned int fiCnt = 0;

string s;
while (cin >> s) {
    if (s == "ff")
        ++ffCnt;
    else if (s == "fl")
        ++flCnt;
    else if (s == "fi")
        ++fiCnt;
}
cout << "numbers of a: " << ffCnt << "\n"
    << "numbers of e: " << flCnt << "\n"
    << "numbers of u: " << fiCnt << endl;

```

本题用switch case实现比较麻烦，查资料大致有两种方法：一种是用枚举，将字符串和枚举量绑定；另一种是用hash值，计算每个字符串的hash值，再用hash值去匹配

#### 5.14

```c++

string pre;
string s;
int count = 1;
int max = 1;
string continue_str;

while (cin >> s) {
    if (s == pre) {
        ++count;
    }
    else {
        if (count > max) {
            continue_str = pre;
            max = count;
        }
        count = 1;
        pre = s;
    }
}
if (count > max) { //判断输入尾部的连续数据
    continue_str = pre;
    max = count;
}
cout << continue_str << " appears " << max << " times." << endl;

```

#### 5.17

解法1

```c++

vector<int> v1 = { 1,1,2,6 };
vector<int> v2 = { 1,1,2,3,4,5,6,7 };
bool status = true;

decltype(v1.size()) min = (v1.size() < v2.size()) ? v1.size() : v2.size();

for (decltype(min) i = 0; i < min; ++i) {
    if (v1[i] != v2[i]) {
        status = false;
        break;
    }
}
cout << status << endl;

```

解法2

```c++

vector<int> v1 = { 1,1,2,3 };
vector<int> v2 = { 1,1,2,3,4,5,6,7 };

decltype(v1.size()) min = std::min(v1.size(), v2.size());

v1.resize(min);
v2.resize(min);

cout << (v1 == v2) << endl;

```

#### 5.19

```c++

string s1;
string s2;
do {
    cout << "please enter two strings: ";
    cin >> s1 >> s2;

    cout << (s1.size() < s2.size() ? s1 : s2) << endl;
} while (cin); //留意这个写法，cin对象一直存在

```

#### 5.20

```c++

string pre;
string s;
bool sign = false;
while (cin >> s) {
    if (s == pre) {
        cout << s << endl;
        sign = true;
        break;
    }
    pre = s;
}
if (!sign)
    cout << "no contiune word" << endl;

```

#### 5.21

```c++

string pre;
string buf;
while (cin >> buf && !buf.empty()) {
    if (buf == pre && isupper(buf[0])) {
        cout << buf << endl;
        continue;
    }
    pre = buf;
}

```

#### 5.22

```c++

int sz;
while ((sz = get_size()) <= 0)
{
}

```

#### 5.25

```c++

int x;
int y;

while (cin >> x >> y) {
    try
    {
        if (0 == y)
            throw 0;
        cout << "x/y=" << x / y << endl;
        break;
    }
    catch (const int)
    {
        char tmp;
        cout << "exception:divided by 0. enter again (y/n)" << endl;
        cin >> tmp;
        if (tmp == 'n' || tmp == 'N') {
            break;
        }
    }
}  

```

**注意：c++ try-catch不能捕捉除0异常，下面的代码在vs2015中修改属性选项：项目属性 -> C/C++ -> 代码生成 -> 启用 C++ 异常 中选择“是，但有SEH异常（/EHa）”。GUN C++暂未找到相关编译选项**
另外windows下还可以使用__try/__except(SEH)来捕捉异常，于C++标准无关，细节不再赘述

```c++

int x;
int y;

cin >> x >> y;

try
{
    cout << "x/y=" << x / y << endl;
}
catch (...)
{
    cout << "exception：divided by 0" << endl;
}

```