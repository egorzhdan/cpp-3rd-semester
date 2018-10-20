## `decltype`

```cpp
vector<int> g(int, float);
char g(string&, int);

template <typename T>
// можно написать auto
// можно сделать trait: return_type
f(T&& ...args) {
    return g(std::forward<T>(args)...));
}

template <typename U, typename V>
std::common_type<U, V>::type max(U const& u, V const& v) {
    return u < v ? v : u;
}
```

Почти корректный код:
```cpp
decltype(g(std::forward<T>(args)...)) f(T&& ...args) { // используем args перед объявлением, поэтому не работает
    return g(std::forward<T>(args)...));
}
```

В C++11 можно писать тип возвращаемого значения после аргументов:
```cpp
auto f(T&& ...args) -> decltype(g(std::forward<T>(args)...)) {
    return g(std::forward<T>(args)...));
}
```

**Non-evaluating context** – то, что внутри `sizeof` или `decltype`.

```cpp
template <typename T>
T& declval();
```
– можно использовать только в non-evaluating контексте, иначе не слинкуется.

Есть 2 формы:
* `decltype(variable)`
* `decltype(expr)`

`rvalue`: `prvalue`, `xvalue`

```cpp
T f();
// ...
T a = f();
      ^^^
      prvalue (pure) – не нужно вызывать конструктор
```

```cpp
T&& f();
// ...
T a = f();
      ^^^
      xvalue (expiring) – придется вызывать move конструктор
```

`decltype(expr)`:
* `prvalue` - `T`
* `lvalue`  - `T&`
* `xvalue`  - `T&&`

`nullptr_t` может приводиться в любой тип указателя.

```cpp
struct nullptr_t {
    template <typename T>
    operator T*() const {
        return 0;
    }
    
    template <typename R, typename T>
    operator R T::*() const {
        return 0;
    }
}

static nullptr_t nullptr;
```

Но в реальности в стандарте написано так: `typedef typename(nullptr) nullptr_t;`

Указатель на member-функцию – сложный тип, потому что нельзя просто добавить параметр `this` из-за ambiguity.
```cpp
struct foo { int bar; }

&foo::bar // имеет тип: int foo::* p
          //                ^^^^^^ offset от начала структуры

int foo::*p = &foo::bar // не можем просто обратиться, потому что нужен this
foo v;
v.*p
```
