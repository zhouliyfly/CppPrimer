#### 8.1

```c++
istream& rw_func(istream& is) {
    auto old_state = cin.rdstate();
    string s;
    while (is >> s) {
        cout << s << " ";
    }
    is.clear();
    cin.setstate(old_state);
    return is;
}
```

其中保存旧状态，恢复新状态也可以不写。因为cin开始就是0，clear之后也是0

#### 8.4

```c++
void rwf_func(const string& s) {
    vector<string> v;
    ifstream input(s, ifstream::in);
    string tmp;

    while(getline(input, tmp))
        v.push_back(tmp);

    /*for (auto str : v)
        cout << str << endl;*/
}
```

#### 8.5

```c++
void rwf_func1(const string& s) {
    vector<string> v;
    ifstream input(s, ifstream::in);
    string tmp;

    while (input >> tmp)
        v.push_back(tmp);

    /* for (auto str : v)
        cout << str << endl;*/
}
```

#### 8.6

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
istream& read(istream& is, Sales_data& item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sole >> price;
    item.revenue = price * item.units_sole;
    return is;
}
ostream& print(ostream& os, const Sales_data& item)
{
    os << item.isbn() << " "
        << item.units_sole << " " << item.revenue;
    return os;
}

void rwf_func2(istream& is, ostream& os=cout) {
    Sales_data total;
    if (read(is, total)) {
        Sales_data trans;
        while (read(is, trans)) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(os, total) << endl;
                total = trans;
            }
        }
        print(os, total) << endl;
    }
    else {
        cerr << "No data?!" << endl;
    }
}

//main.cpp----
ifstream in(argv[1]);
rwf_func2(in);
```

#### 8.7

```c++
//main.cpp----
ifstream in(argv[1]);
ofstream ou(argv[2]);
rwf_func2(in, ou);
```

#### 8.8

```c++
//main.cpp----
ifstream in(argv[1]);
ofstream ou(argv[2], ostream::app);
rwf_func2(in, ou);
```

#### 8.9

```c++
void rw_func3(istream& is) {
    string line;
    string s;
    while (getline(is, line)) {
        istringstream in(line);
        while (in >> s) {
            cout << s << " ";
        }
        cout << endl;
    }
}

rw_func3(cin);
```

#### 8.10

```c++
void rw_func4(istream& is) {
    string line;
    string s;
    vector<string> v;
    while (getline(is, line)) {
        v.push_back(line);
    }
    for (auto w : v) {
        istringstream in(w);
        while (in >> s) {
            cout << s << " ";
        }
    }
}
```

#### 8.11

```c++
struct PersonInfo {
    string name;
    vector<string> phones;
};

void rw_func5(istream& is) {
    string line, word;
    vector<PersonInfo> people;
    istringstream record;
    while (getline(cin, line)) {
        PersonInfo info;
        record.str(line);
        record >> info.name;
        while (record >> word)
            info.phones.push_back(word);
        people.push_back(info);
        record.clear();  //关键这一步，清除流之后才能继续读
    }
    for (auto item : people) {
        cout << item.name << ":";
        for (auto num : item.phones)
            cout << num << " ";
        cout << endl;
    }
}
```

#### 8.13

```c++
struct PersonInfo {
    string name;
    vector<string> phones;
};

bool valid(const string& num) {
    if (num.empty() || num.size() != 8)
        return false;

    for (auto c : num) {
        if (!isdigit(c))
            return false;
    }
    return true;
}

string format(string s) {
    s.insert(4, "-");
    return s;
}

void rw_func6_5(istream& is, vector<PersonInfo>& people) {
    string line, word;
    istringstream record;
    while (getline(is, line)) {
        PersonInfo info;
        record.str(line);
        record >> info.name;
        while (record >> word)
            info.phones.push_back(word);
        people.push_back(info);
        record.clear();
    }
}
void rw_func6(vector<PersonInfo>& people, ostream& os) {
    for (const auto& entry : people) {
        ostringstream formatted, badNums;
        for (const auto& nums : entry.phones) {
            if (!valid(nums)) {
                badNums << " " << nums;
            }
            else
                formatted << " " << format(nums);
        }
        if (badNums.str().empty())
            os << entry.name << " "
            << formatted.str() << endl;
        else
            cerr << "input error:" << entry.name
            << " invalid number(s)" << badNums.str() << endl;
    }
}

ifstream in("phone.txt");
vector<PersonInfo> people;

rw_func6_5(in, people);
rw_func6(people, cout);
```