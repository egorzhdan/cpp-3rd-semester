## Intrusive containers

```cpp
set<unit *> units;
set<unit *> moving_units;
```
– нехорошо по скорости

Сделаем linked list по moving.
Такой список – intrusive list.

```cpp
template <typename Tag=default_tag>
struct node {
    node* next;
    node* prev;
}

unit *to_unit(node *p) {
    return (unit *)( (char*) p - offsetof(unit, link) );
}


intrusive_list<unit, struct all_units_tag>;
```

Intrusive могут быть хуже по памяти и делают модификации объекта, 
но выигрывают по числу аллокаций.

Intrusive не используют, потому что пользователь должен сам контроллировать время жизни

## Multi-index containers

* `bimap` – например, 2 дерева
* `lru_cache` – например, дерево и двусвязный список

`boost::multi_index`

```cpp
multimap<string, person *> by_name;
multimap<person_id, person *> by_id;

std::string_ref name;
// хочется достать person*
```

**Гетерогенный поиск** – поиск по ключу не такого типа, какого на самом деле контейнер.
`map` и `set` его поддерживат.