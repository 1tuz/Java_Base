#sql #базы_данных #db #архитектура #jpa
JPA(Java Persistent API) — это стандарт для работы с базами данных в Java-приложениях. В Spring Boot для работы с JPA часто используется Hibernate.

#### **Основные аннотации JPA:**

- **@Entity**: Обозначает, что класс является сущностью.
- **@Id**: Указывает на поле, которое будет использоваться как идентификатор (ключ).
- **@GeneratedValue**: Определяет стратегию генерации значений для идентификаторов.
- **@Table**: Указывает имя таблицы в базе данных.
- **@Column**: Указывает на сопоставление с колонкой в таблице.
- **@OneToMany, @ManyToOne**: Описывают отношения между сущностями.