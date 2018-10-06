```cpp
struct function {
    template <typename F>
    function(F f) : ptr(new F(f)), destroy(&destroy_impl<F>) {
        
    }
}
// не можем сделать шаблонный function, потому что у каждой будет свой тип
// позволяет делать small object optimization
```

**Type erasure** – не шаблонный интерфейс и шаблонная релизация.

## Variardic template

template <typename T, typename ...A> // не обязательно один тип
unique_ptr<T> make_unique(A && ...a) {
    sizeof... // количество аргументов

    return unique_ptr<T>(new T(forward<A>(a)...));
}

```cpp
template <size_t N, typename T0, typename ...T>
struct at {
    typedef typename at<N-1, Ts...>::type type;
};

template <typename T0, typename ...T>
struct at<0, T0, Ts...> {
    typedef T0 type;
};
```
