
#Spring #rest #Аннотации 

## Общая архитектура проекта:

### Структура проекта и назначение файлов:

- **Spring Boot** – фреймворк, который упрощает настройку и запуск приложения.
- **REST API** – слой контроллера, который принимает HTTP-запросы и возвращает ответы в виде JSON или другого формата.
- **Сервис (Service)** – интерфейс, определяющий бизнес-логику приложения.
- **ServiceImpl** – реализация интерфейса сервиса, содержащая конкретную бизнес-логику, обрабатывающую данные от репозитория.
- **Репозиторий (Repository)** – отвечает за доступ к базе данных. Spring Data JPA автоматически генерирует реализации CRUD-операций.
- **Сущность (Entity)** – класс, отображаемый на таблицу в БД; определяет структуру данных, которые хранятся в базе.
- **DTO (Data Transfer Object)** – объекты для передачи данных между слоями приложения (например, между контроллером и сервисом).
- **MapStruct** – библиотека, которая автоматически генерирует код для преобразования между Entity и DTO.
- **Swagger** – инструмент для автодокументации REST API, позволяющий визуально тестировать и изучать API через веб-интерфейс.
- **Redis** – распределенное хранилище данных в памяти, используемое для кэширования, хранения сессий и в качестве брокера сообщений.

### MyAppApplication.java

Основной класс приложения, содержащий метод `main()`, который запускает Spring Boot приложение.

### Пакет controller/ (например, UserController.java)

- Обрабатывает входящие HTTP-запросы по URL, заданным через аннотацию `@RequestMapping` и связанные с ними HTTP-методы (`@GetMapping`, `@PostMapping` и т.д.).
- Пример: При обращении к `/api/users` метод получает список пользователей через сервис.

### Пакет service/ (например, UserService.java и UserServiceImpl.java)

- **UserService.java**: Интерфейс, который определяет контракт бизнес-логики - какие операции должны быть доступны.
- **UserServiceImpl.java**: Реализация интерфейса, содержащая конкретную бизнес-логику. Здесь данные запрашиваются из репозитория, обрабатываются и преобразуются (например, с помощью MapStruct) в нужный формат (DTO).
- Разделение на интерфейс и реализацию улучшает тестируемость, обеспечивает гибкость и следует принципу инверсии зависимостей.

### Пакет repository/ (например, UserRepository.java)

- Интерфейс, расширяющий `JpaRepository`, предоставляет базовые CRUD-операции для сущности User.
- Пример: Сохранение, поиск, обновление и удаление пользователей в базе данных.

### Пакет entity/ (например, User.java)

- Класс-сущность, соответствующий таблице в БД. Определяет поля, которые будут храниться в таблице.
- Пример: Поля id и name в таблице users.

### Пакет dto/ (например, UserDto.java)

- Объект для передачи данных. Используется для обмена информацией между клиентом (API) и сервером, не раскрывая внутреннюю структуру сущности.

### Пакет mapper/ (например, UserMapper.java)

- Интерфейс для MapStruct, который автоматически генерирует код для преобразования между User и UserDto.
- Пример: Метод toDto() преобразует объект User в UserDto, а toEntity() – обратно.

### Пакет config/ (например, SwaggerConfig.java)

- Файл для конфигурации Swagger, который задаёт параметры документации API.

### Пакет exception/ (например, ResourceNotFoundException.java)

- Содержит пользовательские исключения, которые могут возникать в приложении.
- Позволяет единообразно обрабатывать ошибки и возвращать структурированные сообщения клиенту.

### application.yml

- Файл с настройками приложения: порт сервера, параметры подключения к PostgreSQL, настройки Hibernate, логирование, конфигурация Swagger и прочее.

### pom.xml

- Файл сборки Maven, где прописаны зависимости проекта: Spring Boot, Spring Data JPA, PostgreSQL драйвер, MapStruct, Swagger/OpenAPI и т.д.

### data.sql (при наличии)

- Скрипт для начальной загрузки данных в базу.

## Разбор основных аннотаций:

1. **@Entity**
    
    - Применяется к классу, чтобы обозначить его как сущность JPA.
    - Hibernate будет использовать этот класс для отображения таблицы в базе данных.
2. **@Table(name = "users")**
    
    - Указывает, с какой таблицей БД связана данная сущность.
    - Здесь класс User соответствует таблице users.
3. **@Id**
    
    - Помечает поле, которое является первичным ключом таблицы.
4. **@GeneratedValue(strategy = GenerationType.IDENTITY)**
    
    - Задает способ генерации значений для первичного ключа (например, автоинкремент в PostgreSQL).
5. **@Mapper(componentModel = "spring")**
    
    - Используется в интерфейсе MapStruct.
    - Позволяет Spring автоматически зарегистрировать сгенерированный маппер как bean, чтобы потом можно было его инжектить.
6. **Расширение JpaRepository в UserRepository.java**
    
    - Не требует аннотации, но интерфейс наследует CRUD-методы для работы с БД.
    - Spring Data автоматически создаёт реализацию этого интерфейса.
7. **@Service**
    
    - Обозначает класс-сервис, который содержит бизнес-логику.
    - Делает его Spring bean для автоматического внедрения в другие компоненты (например, в контроллер).
8. **@Autowired**
    
    - Используется для автоматического внедрения зависимостей (например, репозитория или маппера).
    - В современном коде рекомендуется использовать внедрение через конструктор вместо аннотации на поле.
9. **@RestController**
    
    - Объединяет аннотации `@Controller` и `@ResponseBody`.
    - Обозначает класс как контроллер, возвращающий данные напрямую (в JSON) вместо представлений.
10. **@RequestMapping("/api/users")**
    
    - Задает базовый URL для всех методов контроллера.
    - Все эндпоинты в UserController будут начинаться с /api/users.
11. **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping**
    
    - Определяют HTTP-методы, которые обрабатывает соответствующий метод контроллера.
12. **@PathVariable**
    
    - Извлекает часть URL (например, идентификатор пользователя) и передает его в параметр метода.
13. **@RequestBody**
    
    - Автоматически преобразует тело HTTP-запроса в объект Java (например, в UserDto).
14. **@Api(tags = "User API")**
    
    - Аннотация Swagger для группировки и описания API в документации.
    - Позволяет быстро найти и протестировать эндпоинты через Swagger UI.
15. **@Transactional**
    
    - Управляет транзакцией для метода или класса.
    - Позволяет указать такие параметры как readOnly, propagation, isolation и т.д.
16. **@ResponseStatus**
    
    - Указывает HTTP-статус ответа для методов контроллера или исключений.
17. **@ControllerAdvice**
    
    - Позволяет создать глобальный обработчик исключений.

## Более подробное описание каждого пункта

### 1. Spring Boot

**Что такое:** Фреймворк, упрощающий настройку и запуск Java-приложений. Он автоматически настраивает компоненты и запускает встроенный сервер.

**Как работает:**

- Приложение стартует через метод `main()` в классе с аннотацией `@SpringBootApplication`.
- Spring Boot сканирует классы в пакете, регистрирует их как бины (компоненты), если они аннотированы (например, `@RestController`, `@Service`).

**Основные понятия:**

- Бины и DI: Spring управляет объектами (бинами) и их зависимостями через инъекции (например, с помощью `@Autowired`).

### 2. REST API

**Что такое:** Архитектурный стиль для создания веб-сервисов, где ресурсы (например, пользователь) доступны через URL, а действия над ними – через HTTP методы.

**Как работает в проекте:**

- Контроллеры (например, UserController) обрабатывают запросы по URL `/api/users`.
- Методы в контроллере аннотированы как `@GetMapping`, `@PostMapping` и т.д. – это указывает, какой HTTP метод они обрабатывают.

**Основные принципы:**

- GET: Получение данных.
- POST: Создание нового ресурса.
- PUT: Обновление существующего ресурса.
- DELETE: Удаление ресурса.

### 3. Spring Data JPA и Hibernate

**Что такое:**

- JPA (Java Persistence API): Стандарт для работы с базами данных через объектно-реляционное отображение (ORM).
- Hibernate: Одна из реализаций JPA.

**Как работает в проекте:**

- Класс User помечен аннотациями `@Entity` и `@Table`, что означает: этот класс отображается на таблицу в базе (таблица users).
- Поле id помечено `@Id` и `@GeneratedValue` – это первичный ключ, значение которого генерируется автоматически.
- Репозитории: Интерфейс UserRepository наследуется от JpaRepository, что дает готовые методы для работы с базой (сохранение, поиск, удаление и т.п.).

**Основные понятия:**

- Сущности: Классы, отражающие структуру таблиц.
- ORM: Автоматическое преобразование объектов в записи таблиц и наоборот.
- CRUD-операции: Create, Read, Update, Delete.

### 4. Паттерн Service/ServiceImpl

**Что такое:** Архитектурный паттерн, разделяющий определение сервиса (интерфейс) от его реализации.

**Зачем нужен:**

- Разделение абстракции и реализации
- Повышение тестируемости через возможность мокирования
- Возможность иметь несколько реализаций одного интерфейса
- Следование принципу инверсии зависимостей (DIP)

**Как работает в проекте:**

- UserService.java: Интерфейс с методами getAll(), getById(), create(), update(), delete()
- UserServiceImpl.java: Класс, реализующий интерфейс UserService, содержащий конкретную бизнес-логику

**Основные аспекты:**

- Зависимости через конструктор: Лучше использовать конструкторы для внедрения зависимостей вместо @Autowired для полей
- Транзакции: Аннотации @Transactional для управления транзакциями
- Валидация: Проверка данных перед выполнением операций
- Обработка исключений: Генерация и обработка специфических исключений

### 5. MapStruct

**Что такое:** Библиотека для автоматического преобразования между объектами.

**Зачем нужен:**

- Позволяет отделить внутреннюю модель данных (Entity) от данных, передаваемых клиенту (DTO).
- Не нужно вручную копировать поля – MapStruct генерирует код для маппинга.

**Как работает в проекте:**

- Интерфейс UserMapper с аннотацией `@Mapper(componentModel = "spring")` имеет методы toDto() и toEntity().
- При сборке проекта MapStruct генерирует реализацию, которая внедряется в сервис через Spring.

### 6. Swagger/OpenAPI

**Что такое:** Инструмент для автоматической документации REST API.

**Зачем нужен:**

- Позволяет просматривать и тестировать эндпоинты через веб-интерфейс.
- Помогает понять, какие запросы принимает API, какие данные возвращает.

**Как работает в проекте:**

- Зависимость из springdoc-openapi добавляет документацию.
- Аннотация @Api в контроллере помогает группировать и описывать эндпоинты.
- После запуска приложения документация доступна по URL (например, /swagger-ui.html).

### 7. PostgreSQL

**Что такое:** СУБД (система управления базами данных), широко используемая в промышленном программировании.

**Как работает в проекте:**

- Подключение к базе настраивается в файле application.yml: задается URL, имя пользователя, пароль и драйвер.
- Hibernate через JPA создает или обновляет таблицы на основе сущностей (например, таблица users из класса User).

**Основные понятия:**

- SQL: Язык запросов для работы с данными.
- JDBC: Интерфейс для подключения Java-приложений к базам данных.

### 8. Maven (pom.xml)

**Что такое:** Система сборки, которая управляет зависимостями и сборкой проекта.

**Как работает в проекте:**

- В файле pom.xml перечислены все зависимости (Spring Boot, Hibernate, PostgreSQL, MapStruct, Swagger и т.д.).
- Maven скачивает нужные библиотеки и организует компиляцию, тестирование и запуск приложения.

**Основные понятия:**

- Зависимости: Библиотеки, необходимые для работы приложения.
- Плагины: Инструменты для выполнения задач (например, сборка jar-файла).

### 9. Обработка исключений

**Что такое:** Механизм для унифицированной обработки ошибок в приложении.

**Как работает в проекте:**

- ResourceNotFoundException: Пользовательское исключение, возникающее при попытке найти несуществующий ресурс.
- GlobalExceptionHandler: Класс с аннотацией @ControllerAdvice, который перехватывает и обрабатывает исключения.

**Основные компоненты:**

- @ControllerAdvice: Аннотация для глобальной обработки исключений.
- @ExceptionHandler: Аннотация для методов, обрабатывающих конкретные типы исключений.
- ResponseEntity: Класс для формирования HTTP-ответа с нужным статусом и телом.

### 10. Redis для кэширования

**Что такое:** Распределенное хранилище данных в памяти, используемое преимущественно для кэширования.

**Зачем нужен:**

- Повышение производительности за счет кэширования часто запрашиваемых данных
- Распределенный кэш, доступный всем экземплярам приложения
- Снижение нагрузки на базу данных

**Как работает в проекте:**

- Настройка через Spring Boot Data Redis и Spring Cache
- Аннотации @Cacheable, @CachePut и @CacheEvict для управления кэшем
- RedisCacheManager для конфигурации параметров кэша (TTL, стратегии сериализации)

**Основные компоненты:**

- RedisTemplate: Класс для прямого взаимодействия с Redis
- @EnableCaching: Аннотация для включения механизма кэширования
- Spring Session + Redis: Хранение сессий пользователей в распределенном хранилище