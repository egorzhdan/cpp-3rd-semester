## Shared Pointer

```cpp
shared_ptr<my_type> p(new my_type);

struct wheel {};

struct car {
    shared_ptr<wheel> wheels; // так делать не надо, 
    // потому что колесо может продолжить жить без машины
}
```

Shared pointer-ы могут иметь общий shared_count, но при этом ссылаться на разные части объекта.
Это называется **Aliasing constructor**.

```cpp
shared_ptr<car> p;
shared_ptr<wheel> w(p, &p->wheels[2]);
```

```cpp
shared_ptr<my_type> p(new my_type);
```
– это не идиоматическое использование.

```cpp
auto q = make_shared<my_type>(1, 2, 3);
```

### Weak Pointer

`weak_ptr` – не мешает удалению объекта

Для этого в shared_count еще хранится количество слабых ссылок,
потому что не можем удалить shared_count пока есть хотя бы одна ссылка.

Но когда используем `make_shared`, то память аллоцируется вместе, и освобождается тоже только вместе,
когда никто не ссылается на объект, даже слабые ссылки.

Если хотим прикастовать указатель, тоже нужно использовать aliasing constructor:
```cpp
shared_ptr<derived> p; // ...

static_pointer_cast // also dynamic/reinterpret
```