#### 9.4

```c++
bool find_val(vector<int>::iterator first, vector<int>::iterator last, int val) {
    while (first != last) {
        if (*first == val)
            return true;
        ++first;
    }
    return false;
}
```

#### 9.5

```c++
vector<int>::iterator find_val2(vector<int>::iterator first, vector<int>::iterator last, int val) {
    while (first != last) {
        if (*first == val)
            return first;
        ++first;
    }
    return last;
}
```

#### 9.18

```c++
void func_918(){
    string s;
    deque<string> dq;
    while (cin >> s) {
        dq.push_back(s);
    }
    for (auto item = dq.begin(); item != dq.end(); ++item)
        cout << *item << " ";
    cout << endl;
}
```

#### 9.19

```c++
void func_919(){
    string s;
    list<string> dq;
    while (cin >> s) {
        dq.push_back(s);
    }
    for (auto item = dq.begin(); item != dq.end(); ++item)
        cout << *item << " ";
    cout << endl;
}
```

#### 9.20

```c++
void func_920() {
    list<int> l = { 1,2,3,4,5,6 };
    deque<int> d_odd;
    deque<int> d_even;
    for (auto val : l) {
        if (val % 2)
            d_odd.push_back(val);
        else
            d_even.push_back(val);
    }
}
```

#### 9.26

```c++
void del_vec() {
    int ia[] = { 0,1,1,2,3,5,8,13,21,55,89 };
    vector<int> v(begin(ia), end(ia));
    for (auto item = v.begin(); item != v.end();) {
        if (*item % 2)
            item = v.erase(item);
        else
            ++item;
    }
    for (auto i : v)
        cout << i << " ";
}
```

#### 9.27

```c++
void del_slist() {
    forward_list<int> flis = { 0,1,1,2,3,5,8,13,21,55,89 };
    for (auto pre = flis.before_begin(), item = flis.begin(); item != flis.end();) {
        if (*item % 2) {
            item = flis.erase_after(pre);
        }
        else {
            pre = item;
            ++item;
        }
    }
}
```

#### 9.28

```c++
void func_928(forward_list<string>& flis, const string& s1, const string& s2) {
    int count = 0;
    auto pre = flis.before_begin(), item = flis.begin();
    while (item != flis.end()) {
        if (*item == s1) {
            item = flis.insert_after(item, s2);
            ++count;
        }
        pre = item;
        ++item;
    }
    if (!count)
        flis.insert_after(pre, s2);
}
```

#### 9.31

```c++
void change_elem() {
    //list
    list<int> li = { 0,1,2,3,4,5,6,7,8,9 };
    auto iter = li.begin();
    while (iter != li.end()) {
        if (*iter % 2) {
            //与vector不同的是，同样是需要iter向后移动两个单位。
            //list迭代器不能直接+2，只能使用++两次。
            //可以使用库函数实现
            //advance(iter, 2);
            li.insert(iter++, *iter); //这里用后++实现
        }
        else {
            iter = li.erase(iter);
        }
    }

    //forward_list
    forward_list<int> flis = { 0,1,2,3,4,5,6,7,8,9 };
    auto pre = flis.before_begin();
    auto iter_f = flis.begin();
    while (iter_f != flis.end()) {
        if (*iter_f % 2) {
            //返回插入的元素位置，即pre之后，iter之前。因而iter需要向后移动2个位置
            iter_f = flis.insert_after(pre, *iter_f); 
            pre = ++iter_f;
            ++iter_f;
        }
        else {
            iter_f = flis.erase_after(pre);
        }
    }
}
```

#### 9.41

```C++
vector<char> vc;
string s(vc.begin(), vc.end());
```

#### 9.42

```C++
void func_942() {
    string s;
    s.reserve(100);
}
```

#### 9.43

```C++
void xchange(string& s, string oldVal, string newVal) { //使用下标更方便
    if (s.empty() || oldVal.empty())
        return;

    auto iter = s.begin();
    while (iter != s.end()) {
        if (*iter == oldVal[0]) {
            string substr = s.substr(iter - s.begin(), oldVal.size());
            if (substr == oldVal) {
                iter = s.erase(iter, iter + oldVal.size());
                iter = s.insert(iter, newVal.begin(), newVal.end());
                advance(iter, newVal.size());
                continue;
            }
        }
        ++iter;
    }
}
```

#### 9.44

```C++
void xchange1(string& s, string oldVal, string newVal) { 
    if (s.empty() || oldVal.empty())
        return;

    for (decltype(s.size()) i = 0; i != s.size();) {
        if (s[i] == oldVal[0]) {
            string substr = s.substr(i, oldVal.size());
            if (substr == oldVal) {
                s.replace(i, oldVal.size(), newVal);
                i += newVal.size();
                continue;
            } 
        }
        ++i;
    }
}
```

#### 9.45

```c++
string add_prefix_and_postfix(const string& s, const string& prefix, const string& postfix) {
    if (prefix.empty() || postfix.empty())
        return s;

    string result = s;
    result.insert(result.begin(), prefix.begin(), prefix.end());
    result.append(postfix);

    return result;
}
```

#### 9.46

```c++
string add_prefix_and_postfix1(const string& s, const string& prefix, const string& postfix) {
    if (prefix.empty() || postfix.empty())
        return s;

    string result = s;
    result = result.insert(0, prefix);
    result = result.insert(result.size(), postfix);

    return result;
}
```

#### 9.47

```c++
void func_947_1() {
    string nums = "0123456789";
    string charc = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    string s = "ab2c3d7R4E6";
    decltype(s.size()) pos = 0;
    while ((pos = s.find_first_of(nums, pos)) != string::npos) {
        cout << "pos[" << pos << "]=" << s[pos] << " ";
        ++pos;
    }
    cout << "\n";
    pos = 0;
    while ((pos = s.find_first_of(charc, pos)) != string::npos) {
        cout << "pos[" << pos << "]=" << s[pos] << " ";
        ++pos;
    }
    cout << endl;
}

void func_947_2() {
    string nums = "0123456789";
    string charc = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    string s = "ab2c3d7R4E6";
    decltype(s.size()) pos = 0;
    while ((pos = s.find_first_not_of(charc, pos)) != string::npos) {
        cout << "pos[" << pos << "]=" << s[pos] << " ";
        ++pos;
    }
    cout << "\n";
    pos = 0;
    while ((pos = s.find_first_not_of(nums, pos)) != string::npos) {
        cout << "pos[" << pos << "]=" << s[pos] << " ";
        ++pos;
    }
    cout << endl;
}
```

#### 9.49

```c++
void func_949(const string& fname) {
    string dwords = "gjpqy";
    string uwords = "bdfhlt";

    string s;
    string longest_word;
    ifstream input(fname);
    while (input >> s) {
        if (s.find_first_of(dwords) == string::npos &&
            s.find_first_of(uwords) == string::npos &&
            s.size() > longest_word.size())
        {
            longest_word = s;
        }
    }
    cout << "the longest word is " << longest_word << endl;
}
```

#### 9.50

```c++
void func_950_int(const vector<string>& v) {
    if (v.empty())
        return;

    string vaild = "+-0123456789";
    long long sum = 0;
    for (const auto& s : v) {
        sum += stoi(s.substr(s.find_first_of(vaild)));
    }
    cout << "sum=" << sum << endl;
}

void func_950_double(const vector<string>& v) {
    if (v.empty())
        return;

    string vaild = "+-.0123456789"; //浮点数可以是.开头
    double sum = 0;
    for (const auto& s : v) {
        sum += stod(s.substr(s.find_first_of(vaild)));
    }
    cout << "sum=" << sum << endl;
}
```