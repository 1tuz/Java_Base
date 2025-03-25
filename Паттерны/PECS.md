
🔹 **PECS (Producer Extends, Consumer Super)** - принцип использования **дженериков** (generics) в Java при работе с **коллекциями**

✅ `extends` — если объект только **читаем** (`Producer`).  
✅ `super` — если объект только **записываем** (`Consumer`).  
✅ Без wildcard (`<?>`) — если **читаем и пишем**.

🔹 **Пример использования**  
✅ `List<? extends Number>` — читаем `Number` и его наследников.  
✅ `List<? super Integer>` — записываем `Integer` и его предков.