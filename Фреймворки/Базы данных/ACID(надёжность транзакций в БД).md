#sql #базы_данных #db #архитектура
**ACID** — это набор свойств, обеспечивающих надёжность транзакций в базах данных.

### **1. Atomicity (Атомарность)**

Транзакция выполняется **либо полностью, либо не выполняется вообще**. Если одна из операций внутри транзакции не удалась, все предыдущие изменения откатываются (rollback).

**Пример:** перевод денег между счетами — если списание со счёта A прошло, но зачисление на счёт B не удалось, вся операция отменяется.

### **2. Consistency (Согласованность)**

Транзакция переводит базу данных **из одного корректного состояния в другое**. Данные не должны оставаться в промежуточном или некорректном состоянии.

**Пример:** сумма денег на счетах до и после перевода должна быть одинаковой — если в одном месте сумма уменьшилась, в другом должна увеличиться.

### **3. Isolation (Изолированность)**

Параллельно выполняющиеся транзакции **не должны мешать друг другу** и изменять данные друг друга в некорректном виде. Уровни изоляции регулируют доступ к данным во время выполнения транзакций.

**Пример:** два пользователя одновременно меняют баланс на одном счёте — один не должен видеть "половинчатые" изменения другого.

### **4. Durability (Долговечность)**

Изменения, внесённые успешной транзакцией, **должны сохраняться даже при сбое системы** (например, после перезапуска сервера).

**Пример:** если перевод завершился успешно, сумма не должна "пропасть" из-за отключения света.

Все эти свойства вместе гарантируют, что работа с базой данных будет надёжной, предсказуемой и защищённой от ошибок.