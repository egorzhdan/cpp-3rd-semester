```cpp
struct big {
    std::array<int, 10> data;
}

// ссылка передается как указатель

void foo(big_struct); // копируем, в функцию передастся ссылка
```

```cpp
big_struct a;
f(a);
```

Транслируется в одно из двух:
```cpp
big_struct a;
big_struct copy = a;
f(copy);
```
– c++
или
```cpp
big_struct a;
f(a); // копия внутри
```
– pascal

Если передаем rvalue, то не будет копирования.
Для маленьких структур будет передаваться через регистр.

Возвращаемые значения:
```cpp
big_struct f();

void g() {
    big_struct a = f();
}
```
Транслируется в:
```cpp
void f(big_struct);

void f(void* result) {
    // ...
    ctor_big_struct(result);
}

void g() {
    char a_buf[sizeof(big_struct)];
    f(a_buf);
    bug_struct &a = (big_struct&) a_buf;
}
```

В таком коде не будет копирований:
```cpp
big_struct g();
void f(big_struct);

f(g());
```

### RVO -- return value optimization

```cpp
struct big_struct {
    big_struct(int, int, int) {}
}

big_struct g() {
    // ...
    return big_struct(1, 2, 3);
}
```
наивно транслируется в:
```cpp
void g(void* result) {
    char tmp[sizeof(big_struct)];
    ctor_big_struct(tmp, 1, 2, 3);
    ctor_big_struct(result, (big_struct&) tmp);
}
```
а на самом деле транслируется в:
```cpp
void g(void* result) {
    ctor_big_struct(result, 1, 2, 3);
}
```

### Named RVO

Можно не создавать переменную до копирования:
```cpp
big_struct g() {
    big_struct tmp(1, 2, 3);
    // ...
    result->f();
    // ...
    return tmp;
}
```
Но это плохо с точки зрения исключений (`f` может упасть), поэтому оборачиваем в try/catch.

### _

```cpp
vector<string> v;

string s;
v.push_back(s);
```

```cpp
vector<file_descriptor> v; // в C++03 не работает, приходится хранить указатель или boost::shared_ptr
```

# Move semantics

Многие операции, которые не могут копироваться, можно перемещать.

Можно сделать конструктор `auto_ptr(auto_ptr&)`, но тогда например сломается `std::sort`.

```cpp
push_back(arg);
```
Если `arg` -- rvalue, то его можно испортить.

В C++11 появился способ перегружать функции для lvalue и rvalue.

`&` -- lvalue ссылки
`&&` -- rvalue ссылки

```cpp
int a = 5;
int& b = a; // ok
int&& c = a; // error

int& d = f(); // error
int&& e = f(); // ok
```

```cpp
void push_back(T const&); // если const
void push_back(T&&); // если rvalue

string(string const&) // copy ctor
string(string&&) // move ctor
```

Именованная переменная -- всегда lvalue.
```cpp
void pb(T&& a) {
    a; // lvalue
}
```

Иногда хотим разрешить портить переменную, если больше не планируем ее использовать:
```cpp
void pb(T&& a) {
    foo(static_cast<T&&>(a));
    // ...
    // no usages of a
}
```

В stdlib есть функция для этого:
```cpp
template <typename T>
T&& move(T& obj) {
    return static_cast<T&&>(obj);
}
```

```cpp
string foo() {
    string res = "hello ";
    res += "world";
    // which:
    //    return res; 
    // or
    //    return 
}
```
Move срабатывает даже если просто вернем локальную переменную.
А если явно напишем `move`, то подавим RVO -- плохо.
