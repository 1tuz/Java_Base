---
aliases: []
---
#Исключения
### **Зачем?**

- Для описания специфических ошибок, недоступных через стандартные исключения.
- Улучшает читаемость и управление ошибками.

---

### **Как создать?**

1. Наследуйтесь от `Exception` (для checked) или `RuntimeException` (для unchecked).
2. Определите конструктор с сообщением.

---

#### **Пример 1: Checked Exception**

```java
class CustomCheckedException extends Exception {
    public CustomCheckedException(String message) {
        super(message);
    }
}

public class Main {
    public static void validateAge(int age) throws CustomCheckedException {
        if (age < 18) throw new CustomCheckedException("Возраст < 18");
    }
}
```

---

#### **Пример 2: Unchecked Exception**

```java
class CustomUncheckedException extends RuntimeException {
    public CustomUncheckedException(String message) {
        super(message);
    }
}

public class Main {
    public static void validateName(String name) {
        if (name.isEmpty()) throw new CustomUncheckedException("Имя пустое");
    }
}
```

---

### **Когда использовать?**

- **Checked:** Обрабатываемые ошибки, которые нужно ловить.
- **Unchecked:** Ошибки, связанные с неверным кодом.