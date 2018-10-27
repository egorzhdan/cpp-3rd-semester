### atomic

Операции:
* load
* store
* `compare_exchange`

```cpp
struct queue {
    deque<T> q;
    condition_variable non_empty;

    void push(T val) {
        lock_guard<mutex> lg(m);
        q.push_back(move(val));
    }
}
```

`condition_variable`:
* `.wait()` // заснем
* `notify_one()` // пробудим 1 поток
* `notify_all()`
