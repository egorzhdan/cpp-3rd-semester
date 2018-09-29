Пусть `T` не поддерживает rvalue ссылки
```cpp
T a;

T b = std::move(a); // вызовется отдельный конструктор
```

Когда вектор расширяется, если move элементов может бросить исключения, то будет просто копироваться.

```cpp
string(string&&) noexcept;

enable_if<noexcept(a+b)>::type
```

## Forwarding

```cpp
void f(int);
void f(char);

template <typename T>
void g(T const& a) {
    f(a); // копирование, а он может не уметь копироваться
}

template <typename T>
void g(T& a) {
    f(a); // копирование, а он может не уметь копироваться
}

// сделали 2 перегрузки
```

**Perfect forwarding** -- сохраняет lvalue/rvalue.

### Reference collapsing rule
```cpp
typedef int& foo;
typedef foo& bar; // bar is int&
```
Правило расширено для rvalue:
& и & = &
& и && = &
&& и & = &&
&& и && = &&

```cpp
void f(int&& a) {
    g(static_cast<int&&>(a));
}

template <typename T>
T&& forward(T&& a) {
    return static_cast<T&&>(a);
}
```

Forward для произвольного числа аргументов:
```cpp
template <typename ...T>
T&& forward(T&& ...a) {
    return g(forward<T>(a) ...);
}
```
