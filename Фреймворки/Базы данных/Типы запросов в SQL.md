#sql #базы_данных #db #запросы #select #insert #update #delete #drop #alter


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

- **DROP**  
  📋 Удаление таблицы или других объектов:  
  ```sql
  DROP TABLE table_name;
  ```  
  👉 Пример: `DROP TABLE users;`  
  💡 Осторожно! Это действие необратимо.

- **ALTER**  
  📋 Изменение структуры таблицы:  
  ```sql
  ALTER TABLE table_name ADD|DROP|MODIFY column_name data_type;
  ```  
  👉 Примеры:  
  - Добавить столбец: `ALTER TABLE users ADD email VARCHAR(255);`  
  - Удалить столбец: `ALTER TABLE users DROP COLUMN email;`  
  - Изменить тип столбца: `ALTER TABLE users MODIFY age INT;`
