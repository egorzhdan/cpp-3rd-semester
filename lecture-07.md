# Signals

```cpp
struct signal {
    struct connection;
    typedef function<void()> slot;
    vector<slot> slots; // обработчики
    
    connection connect(slot s) {
        slots.push_back(move(s));
    }
    
    void operator()() const {
        for (auto&& s : slots) try { s(); } catch (...) { log("..."); } // плохо
    }
}

struct signal::connection {
    // ctor
    
    void disconnect() {
        sig->slots.disconnect() // NPE?
    }
}
```

**Reentrancy** – 
