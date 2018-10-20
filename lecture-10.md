## C++17 stdlib

### `optional`

```cpp
optional<int> a = 5;
a = nullopt;
if (a) { }

struct foo {
    foo(int, int, int);
}

optional<foo> a(in_place, 1, 2, 3);
a.emplace(4, 5, 6);
```

Похож на `unique_ptr`, иногда лучше использовать `unique_ptr`:
* когда большие объекты
* polymorphic objects
* меньше зависимостей – pimpl (использование указателей позволяет избегать include)
* объект не поменяет свой адрес

### `variant`

```cpp
variant<A, B, C> v;

visit(visitor(), v);

struct visitor {
    void operator()(A) const;
    // ...
}
```
