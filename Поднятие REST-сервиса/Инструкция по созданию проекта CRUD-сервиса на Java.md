#rest #spring #crud #db #orm #гайд #mapstruct

### 1. Создание структуры проекта

```
my-app/
│── src/
│   ├── main/
│   │   ├── java/com/example/myapp/
│   │   │   ├── controller/
│   │   │   │   └── UserController.java
│   │   │   ├── dto/
│   │   │   │   └── UserDto.java
│   │   │   ├── entity/
│   │   │   │   └── User.java
│   │   │   ├── mapper/
│   │   │   │   └── UserMapper.java
│   │   │   ├── repository/
│   │   │   │   └── UserRepository.java
│   │   │   ├── service/
│   │   │   │   └── UserService.java
│   │   │   ├── config/
│   │   │   │   └── SwaggerConfig.java
│   │   │   └── MyAppApplication.java
│   └── resources/
│       ├── application.yml
│       └── data.sql
└── pom.xml
```

### 2. Подключение зависимостей в `pom.xml`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.5.3.Final</version>
</dependency>
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.0.2</version>
</dependency>
```

### 3. Код классов

#### Entity: `User.java`

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    // getters/setters
}
```

#### DTO: `UserDto.java`

```java
public class UserDto {
    private Long id;
    private String name;
    // getters/setters
}
```

#### Mapper: `UserMapper.java`

```java
@Mapper(componentModel = "spring")
public interface UserMapper {
    UserDto toDto(User user);
    User toEntity(UserDto dto);
}
```

#### Repository: `UserRepository.java`

```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```

#### Service: `UserService.java`

```java
@Service
public class UserService {
    @Autowired
    private UserRepository repo;
    @Autowired
    private UserMapper mapper;

    public List<UserDto> getAll() {
        return repo.findAll().stream().map(mapper::toDto).collect(Collectors.toList());
    }
    public UserDto getById(Long id) {
        return mapper.toDto(repo.findById(id).orElseThrow());
    }
    public UserDto create(UserDto dto) {
        return mapper.toDto(repo.save(mapper.toEntity(dto)));
    }
    public UserDto update(Long id, UserDto dto) {
        User user = repo.findById(id).orElseThrow();
        user.setName(dto.getName());
        return mapper.toDto(repo.save(user));
    }
    public void delete(Long id) {
        repo.deleteById(id);
    }
}
```

#### Controller: `UserController.java`

```java
@RestController
@RequestMapping("/api/users")
@Api(tags = "User API")
public class UserController {
    @Autowired
    private UserService service;

    @GetMapping
    public List<UserDto> getAll() {
        return service.getAll();
    }
    @GetMapping("/{id}")
    public UserDto getById(@PathVariable Long id) {
        return service.getById(id);
    }
    @PostMapping
    public UserDto create(@RequestBody UserDto dto) {
        return service.create(dto);
    }
    @PutMapping("/{id}")
    public UserDto update(@PathVariable Long id, @RequestBody UserDto dto) {
        return service.update(id, dto);
    }
    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) {
        service.delete(id);
    }
}
```

### 4. Конфигурация `application.yml`

```yaml
server:
  port: 8080

spring:
  application:
    name: my-app

  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: myuser
    password: mypassword
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
        format_sql: true
        show_sql: true
    open-in-view: false

  flyway:
    enabled: true

  logging:
    level:
      org:
        springframework:
          web: INFO
      com:
        example: DEBUG
      org.hibernate.SQL: DEBUG
      org.hibernate.type.descriptor.sql: TRACE

  openapi:
    swagger-ui:
      path: /swagger-ui.html
      enabled: true

springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    operationsSorter: method
    tagsSorter: alpha
```

### 5. Запуск Swagger UI

После запуска приложения Swagger UI будет доступен по адресу:

```
http://localhost:8080/swagger-ui.html
```