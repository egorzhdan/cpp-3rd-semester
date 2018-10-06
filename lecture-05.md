# Рандомные фичи языка
## `auto`

```cpp
auto a = f();
```

Тип выводится так же, как у шаблонных параметров.
```cpp
template <typename T>
void b(T);

b(f(x));
```

У `auto` не выводится `const`, то есть если `b` вернет `const int`, то тип все равно выведется `int`.

```cpp
auto *a = f(), &b = g();
// auto выведется для первого, и для каждого следующего тоже, и проверяется что он совпадает
```

## Анонимные функции

```cpp
template <typename It, typename Comp>
void sort(It first, It last, Comp comp) {
    // ...
    comp(a, b);
    // ...
}

sort(v.begin(), v.end(), /* std::less / functional object / custom func */ [](int a, int b) {
    return abs(a) < abs(b);
});

// [] - lambda introducer
// [a, b, c]
// [&a, &b, &c]
// [=, &a, &b, &c] - все по значению, конкретные по ссылке
// [&, a, b, c] - наоборот
```

Можно указать тип возвращаемого значения:
```cpp
[]() -> int
```
