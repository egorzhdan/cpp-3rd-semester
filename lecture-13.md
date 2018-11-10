## Высокоуровневая многопоточность

### `promise` / `future`

Тот, кто считает оперирует с `future`, а тот кто запросил получает `promise`.

`promise`:
* `set_value`
* `set_exception`

`future`:
* `get_value`

`shared_future` – может разделяться между пользователями.

### `std::packaged_task`
Обертка над promise.

`void ()` – ничего не принимает и не возвращает, вместо этого выставляет `promise<T>`.

### `std::async`

`T f()` – выполняет и возвращает `future<T>`

Разрешаем посчитать лениво, внутри `.get()`.

### Thread pool
Обычно делаются на фиксированное число потоков.

Нет в stdlib.

### `boost::thread`

`cancellation_point()` – по идее должен быть в библиотеке, но нет,
потому что в stdlib многие примитивы реализованы через OS

### Thread Local Storage
Например когда нужен singleton, но не хочется делать его thread safe.

## Непрактические решения:
### Software Transactional Memory
Запоминаем, что поменяли, чтобы можно было откатить операции.

### Hardware Transactional Memory
То же самое, только на уровне CPU.
