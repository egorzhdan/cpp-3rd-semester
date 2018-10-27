# Многопоточность

* instruction level parallelism (ILP) – внутри одного потока процессор пытается оптимайзить (для программиста это не заметно)
* single instruction multiple data (SIMD)
* hyper-threading
* multicore

```cpp
#include <thread>

int main() {
    std::thread t([] {
        std::cout << "h";
    });
  
    t.join(); // ждем пока закончится
    // t.detach(); // отвяжись от ОС
}
```

**Race condition** – результат вычисления зависит от того, кто быстрее вычисляет.

```cpp
int accounts[10000];
std::mutex m;

void transfer(size_t from, size_t to, int amount) {
    m.lock();
    if (...) throw runtime_error();
    
    m.unlock(); // bad!
}
```
Нужно использовать `std::lock_guard` – разлочит в деструкторе.
