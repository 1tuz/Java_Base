#sql #базы_данных #db #запросы #select #insert #update #delete

Вот шпаргалка по запросам в стиле краткого справочника:

---

### **SQL-запросы**

- **SELECT**  
  📋 Выбор данных из таблицы:  
  ```sql
  SELECT columns FROM table [WHERE condition];
  ```  
  👉 Пример: `SELECT name, age FROM users WHERE age > 18;`

- **INSERT**  
  📋 Добавление новых записей:  
  ```sql
  INSERT INTO table (columns) VALUES (values);
  ```  
  👉 Пример: `INSERT INTO users (name, age) VALUES ('John', 25);`

- **UPDATE**  
  📋 Обновление существующих записей:  
  ```sql
  UPDATE table SET column = value [WHERE condition];
  ```  
  👉 Пример: `UPDATE users SET age = 26 WHERE name = 'John';`

- **DELETE**  
  📋 Удаление записей:  
  ```sql
  DELETE FROM table [WHERE condition];
  ```  
  👉 Пример: `DELETE FROM users WHERE age < 18;`

---

### **Ключевые моменты**
- **WHERE** – фильтрация данных (обязательна для UPDATE/DELETE, чтобы не повлиять на все записи).  
- **Columns** – можно указать конкретные столбцы или использовать `*` для выбора всех.  
- **Transactions** – используйте BEGIN, COMMIT, ROLLBACK для сложных операций.  
