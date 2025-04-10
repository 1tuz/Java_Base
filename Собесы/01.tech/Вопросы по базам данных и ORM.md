
#orm #db #базы_данных 
## 1. Виды Join’ов. Что лучше использовать: join или подзапросы?

**JOIN** — это операция объединения таблиц в SQL. Основные виды:

- **INNER JOIN**: возвращает только те записи, которые имеют соответствие в обеих таблицах
- **LEFT JOIN (или LEFT OUTER JOIN)**: возвращает все записи из левой таблицы, даже если нет соответствующих записей в правой
- **RIGHT JOIN (или RIGHT OUTER JOIN)**: возвращает все записи из правой таблицы, даже если нет соответствующих записей в левой
- **FULL JOIN (или FULL OUTER JOIN)**: возвращает все записи, когда есть совпадение либо в левой, либо в правой таблице

**Подзапросы** могут быть полезны, но **JOIN**'ы обычно быстрее, так как SQL движки лучше оптимизируют их выполнение

## 2. Индексы

**Индексы** — это структура данных, которая ускоряет поиск строк в таблице. Используются для оптимизации запросов

- **B-Tree индексы**: наиболее распространённый вид индексов, обеспечивающий сбалансированный поиск по ключам
- **Два вида индексов**:
    - **Кластерные (clustered)**: данные таблицы хранятся в порядке ключа индекса
    - **Некластерные (non-clustered)**: создаются отдельные структуры, которые указывают на строки таблицы

## 3. Как оценить сложность запроса и что делать при проблемах с производительностью?

- Использовать **EXPLAIN** или **EXPLAIN ANALYZE** для анализа плана выполнения запроса
- Проверить, работают ли индексы
- Оптимизировать запросы и индексацию

## 4. Транзакции. Уровни изолированности транзакций

Транзакции обеспечивают **ACID** (атомарность, согласованность, изолированность, долговечность). Уровни изоляции:

- **Read Uncommitted**
- **Read Committed**
- **Repeatable Read**
- **Serializable**

### N+1 и LazyInitException

- **N+1 проблема**: возникает, когда для каждой записи основного запроса выполняется ещё один запрос для связанных данных
- **LazyInitException**: ошибка, возникающая при попытке доступа к лениво загруженным данным за пределами контекста сессии

### Оптимистические и пессимистические блокировки

- **Оптимистическая блокировка (@Version)**: предполагает, что конфликты редки, и проверяет наличие изменений при сохранении данных
- **Пессимистическая блокировка**: предполагает, что конфликты возможны, и блокирует записи на время изменений

---

# Иногда спрашивают

## 5. ACID

**ACID** — это свойства транзакций:

- **Atomicity** (атомарность)
- **Consistency** (согласованность)
- **Isolation** (изолированность)
- **Durability** (долговечность)

## 6. Нормализация и денормализация

**Нормализация** — процесс устранения избыточности данных для повышения целостности. **Денормализация** — добавление избыточности для улучшения производительности

## 7. Маппинг ассоциаций

- **ManyToMany**
- **OneToOne**
- **ManyToOne**
- **OneToMany**
- Аннотации: **@JoinColumn** и **@JoinTable**

## 8. ORM JPA

**ORM (Object-Relational Mapping)** — это технология, которая позволяет преобразовывать данные между несовместимыми типами систем (реляционные базы данных и объекты в программировании). **JPA** — спецификация для ORM в Java

## 9. Что такое селективность?

**Селективность** — это отношение числа уникальных значений к общему числу записей в столбце таблицы. Высокая селективность улучшает производительность индексов

## 10. Что такое Criteria API и QueryDSL?

**Criteria API** и **QueryDSL** — это инструменты для динамического построения запросов в Java

---

# Спрашивают редко

## 11. Constraints. Какие вы знаете?

**Constraints** — это ограничения в БД, такие как:

- **PRIMARY KEY**
- **FOREIGN KEY**
- **UNIQUE**
- **CHECK**
- **NOT NULL**

## 12. Какие @GeneratedValue вы знаете?

- **AUTO**
- **IDENTITY**
- **SEQUENCE**
- **TABLE**

## 13. JPQL/HQL. Чем отличается от SQL?

**JPQL** (Java Persistence Query Language) и **HQL** (Hibernate Query Language) — это языки запросов, которые работают с сущностями и их свойствами, а не с таблицами и столбцами, как SQL

## 14. DDL, DML, TCL, DCL

- **DDL** (Data Definition Language): операторы для определения структуры базы данных (CREATE, ALTER)
- **DML** (Data Manipulation Language): операторы для работы с данными (SELECT, INSERT, UPDATE, DELETE)
- **TCL** (Transaction Control Language): операторы для управления транзакциями (COMMIT, ROLLBACK)
- **DCL** (Data Control Language): операторы для управления доступом (GRANT, REVOKE)

## 15. Триггер

**Триггер** — это объект базы данных, который автоматически выполняется при определённых событиях (например, INSERT, UPDATE)

## 16. Три нормальные формы

- **1NF**: каждая ячейка таблицы содержит одно значение
- **2NF**: таблица находится в 1NF и все неключевые атрибуты зависят от первичного ключа
- **3NF**: таблица находится в 2NF и все атрибуты зависят только от первичного ключа, а не от других неключевых атрибутов

## 17. Как пользоваться JDBC?

Ключевые интерфейсы:

- **Statement**: для выполнения простых SQL-запросов
- **PreparedStatement**: для выполнения параметризованных SQL-запросов
- **Connection**: для управления соединением с БД
- **ResultSet**: для обработки результатов запросов

## 18. SQL-инъекции

**SQL-инъекции** — это атаки, при которых злоумышленник вставляет вредоносный SQL-код через ввод данных. Защита осуществляется через использование **PreparedStatement**

## 19. FetchType: Eager и Lazy

- **Eager**: загружает связанные данные сразу
- **Lazy**: откладывает загрузку связанных данных до их явного запроса

## 20. Каскады, каскадные операции (CascadeType)

Каскадные операции определяют, как изменения в одном объекте распространяются на связанные объекты. Примеры: **PERSIST**, **MERGE**, **REMOVE**, **REFRESH**, **DETACH**

## 21. Стратегии маппинга иерархии наследования (Inheritance Mapping Strategies)

Три основные стратегии:

- **Single Table**
- **Joined**
- **Table per Class**

## 22. Условия для Entity

Класс должен:

- Иметь аннотацию **@Entity**
- Иметь уникальный идентификатор с аннотацией **@Id**
- Иметь конструктор без аргументов
- Не быть final

---

# ЗП 400+

## 23. Шардирование баз данных

**Шардирование** — это горизонтальное разбиение данных по нескольким базам данных для распределения нагрузки и увеличения производительности

## 24. Что такое Persistence Context?

**Persistence Context** — это набор объектов, которые управляются EntityManager в процессе взаимодействия с базой данных

## 25. Что такое EntityManager?

**EntityManager** — интерфейс в JPA, который управляет жизненным циклом сущностей и взаимодействием с базой данных

## 26. Что такое Mapped Superclass?

**Mapped Superclass** — это класс, который не отображается в таблицу, но его поля наследуются сущностями

## 27. @Embedded и @Embeddable

**@Embeddable** — это класс, который может быть встроен в сущность. **@Embedded** — указывает, что данный объект должен быть встроен в сущность как компонент

## 28. Как реализована ленивая инициализация в Hibernate?

Ленивая инициализация в Hibernate реализована через прокси-объекты, которые загружаются только при обращении к связанным данным