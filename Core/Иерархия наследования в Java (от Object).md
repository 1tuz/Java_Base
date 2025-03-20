1️⃣ **`Object`** — корневой класс для всех классов в Java.  
✅ Методы: `toString()`, `equals()`, `hashCode()`, `clone()`, `wait()`, `notify()`, `finalize()`.

2️⃣ **Базовые классы-обёртки и коллекции**  
✅ `Number` → `Integer`, `Double`, `Float`, `Long`, `Short`, `Byte`.  
✅ `Collection` → `List`, `Set`, `Queue`.  
✅ `Map` (не `Collection`, но тоже от `Object`).

3️⃣ **Иерархия исключений**  
✅ `Throwable` → `Exception` → `RuntimeException` (unchecked).  
✅ `Throwable` → `Error` (например, `OutOfMemoryError`).

4️⃣ **Интерфейсы не наследуются от `Object`, но их имплементации — да**  
✅ `Comparable`, `Serializable`, `Cloneable` и другие.

📌 Всякий класс в Java **неявно наследует `Object`**, если не указывает другого родителя.