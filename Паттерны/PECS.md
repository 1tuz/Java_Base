**PECS** — это принцип использования **дженериков** (generics) в Java при работе с **коллекциями**. Аббревиатура расшифровывается как:
1. **P**roducer — **extends**
2. **C**onsumer — **super**

Принцип PECS простыми словами:
1. **Producer (Источник данных)** → используй `? extends T`. Это когда коллекция **производит данные**, то есть отдаёт (читает) элементы.
2. **Consumer (Потребитель данных)** → используй `? super T`. Это когда коллекция **потребляет данные**, то есть принимает (записывает) элементы.