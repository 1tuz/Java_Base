#rest #spring #crud #db #orm #гайд #mapstruct

## 1. Создание структуры проекта

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
│   │   │   │   ├── UserService.java
│   │   │   │   └── impl/
│   │   │   │       └── UserServiceImpl.java
│   │   │   ├── exception/
│   │   │   │   ├── ResourceNotFoundException.java
│   │   │   │   └── GlobalExceptionHandler.java
│   │   │   ├── config/
│   │   │   │   └── SwaggerConfig.java
│   │   │   └── MyAppApplication.java
│   └── resources/
│       ├── application.yml
│       └── data.sql
└── pom.xml
```

## 2. Подключение зависимостей в pom.xml

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
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

## 3. Код классов

### Entity: User.java

```java
@Entity
@Table(name = "users")
@Data // Lombok аннотация для генерации геттеров, сеттеров, equals, hashCode и toString
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    // При необходимости добавьте дополнительные поля
}
```

### DTO: UserDto.java

```java
@Data // Lombok аннотация
public class UserDto {
    private Long id;
    private String name;
    
    // При необходимости добавьте дополнительные поля
}
```

### Mapper: UserMapper.java

```java
@Mapper(componentModel = "spring")
public interface UserMapper {
    UserDto toDto(User user);
    User toEntity(UserDto dto);
    
    // Можно добавить методы для коллекций
    List<UserDto> toDtoList(List<User> users);
}
```

### Repository: UserRepository.java

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Можно добавить кастомные методы поиска
    List<User> findByNameContaining(String name);
}
```

### Service Interface: UserService.java

```java
public interface UserService {
    List<UserDto> getAll();
    UserDto getById(Long id);
    UserDto create(UserDto dto);
    UserDto update(Long id, UserDto dto);
    void delete(Long id);
}
```

### Service Implementation: UserServiceImpl.java

```java
@Service
@Transactional
public class UserServiceImpl implements UserService {
    private final UserRepository userRepository;
    private final UserMapper userMapper;
    
    // Внедрение зависимостей через конструктор
    public UserServiceImpl(UserRepository userRepository, UserMapper userMapper) {
        this.userRepository = userRepository;
        this.userMapper = userMapper;
    }
    
    @Override
    @Transactional(readOnly = true)
    public List<UserDto> getAll() {
        return userRepository.findAll().stream()
                .map(userMapper::toDto)
                .collect(Collectors.toList());
    }
    
    @Override
    @Transactional(readOnly = true)
    public UserDto getById(Long id) {
        return userMapper.toDto(userRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("User not found with id: " + id)));
    }
    
    @Override
    public UserDto create(UserDto dto) {
        // Валидация данных
        if (dto == null || StringUtils.isEmpty(dto.getName())) {
            throw new IllegalArgumentException("User data is invalid");
        }
        
        User user = userMapper.toEntity(dto);
        User savedUser = userRepository.save(user);
        return userMapper.toDto(savedUser);
    }
    
    @Override
    public UserDto update(Long id, UserDto dto) {
        // Проверка существования пользователя
        User existingUser = userRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("User not found with id: " + id));
        
        // Обновление полей
        existingUser.setName(dto.getName());
        // Обновление других полей при необходимости
        
        User updatedUser = userRepository.save(existingUser);
        return userMapper.toDto(updatedUser);
    }
    
    @Override
    public void delete(Long id) {
        // Проверка существования пользователя перед удалением
        if (!userRepository.existsById(id)) {
            throw new ResourceNotFoundException("User not found with id: " + id);
        }
        userRepository.deleteById(id);
    }
}
```

### Exception: ResourceNotFoundException.java

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

### Exception Handler: GlobalExceptionHandler.java

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<Object> handleResourceNotFoundException(ResourceNotFoundException ex) {
        Map<String, Object> body = new HashMap<>();
        body.put("timestamp", LocalDateTime.now());
        body.put("message", ex.getMessage());
        
        return new ResponseEntity<>(body, HttpStatus.NOT_FOUND);
    }
    
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<Object> handleIllegalArgumentException(IllegalArgumentException ex) {
        Map<String, Object> body = new HashMap<>();
        body.put("timestamp", LocalDateTime.now());
        body.put("message", ex.getMessage());
        
        return new ResponseEntity<>(body, HttpStatus.BAD_REQUEST);
    }
    
    // Другие обработчики исключений...
}
```

### Controller: UserController.java

```java
@RestController
@RequestMapping("/api/users")
@Api(tags = "User API")
public class UserController {
    private final UserService userService;
    
    // Внедрение зависимостей через конструктор
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    @GetMapping
    public List<UserDto> getAll() {
        return userService.getAll();
    }
    
    @GetMapping("/{id}")
    public UserDto getById(@PathVariable Long id) {
        return userService.getById(id);
    }
    
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public UserDto create(@RequestBody UserDto dto) {
        return userService.create(dto);
    }
    
    @PutMapping("/{id}")
    public UserDto update(@PathVariable Long id, @RequestBody UserDto dto) {
        return userService.update(id, dto);
    }
    
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void delete(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

### Main Application Class: MyAppApplication.java

```java
@SpringBootApplication
public class MyAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyAppApplication.class, args);
    }
}
```

## 4. Конфигурация application.yml

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
      hibernate:
        SQL: DEBUG
        type.descriptor.sql: TRACE
    com:
      example: DEBUG
      
springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    path: /swagger-ui.html
    operationsSorter: method
    tagsSorter: alpha
```

## 5. Запуск и использование Swagger UI

После запуска приложения Swagger UI будет доступен по адресу:

```
http://localhost:8080/swagger-ui.html
```

Этот инструмент позволит вам:

- Просматривать все доступные эндпоинты
- Видеть требуемые параметры и тела запросов
- Тестировать API непосредственно из браузера
- Получать образцы запросов и ответов

## 6. Расширенные возможности

### Пагинация результатов

```java
@GetMapping
public Page<UserDto> getAllPaginated(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size) {
    return userService.getAllPaginated(page, size);
}

// В UserService добавить метод:
Page<UserDto> getAllPaginated(int page, int size);

// В UserServiceImpl реализация:
@Override
@Transactional(readOnly = true)
public Page<UserDto> getAllPaginated(int page, int size) {
    Pageable pageable = PageRequest.of(page, size);
    return userRepository.findAll(pageable)
            .map(userMapper::toDto);
}
```

### Фильтрация по имени

```java
@GetMapping("/search")
public List<UserDto> searchByName(@RequestParam String name) {
    return userService.findByNameContaining(name);
}

// В UserService добавить метод:
List<UserDto> findByNameContaining(String name);

// В UserServiceImpl реализация:
@Override
@Transactional(readOnly = true)
public List<UserDto> findByNameContaining(String name) {
    return userRepository.findByNameContaining(name).stream()
            .map(userMapper::toDto)
            .collect(Collectors.toList());
}
```

### Асинхронные операции

```java
@Async
@Override
public CompletableFuture<List<UserDto>> getAllAsync() {
    List<User> users = userRepository.findAll();
    List<UserDto> userDtos = users.stream()
            .map(userMapper::toDto)
            .collect(Collectors.toList());
    return CompletableFuture.completedFuture(userDtos);
}
```

Не забудьте добавить `@EnableAsync` в конфигурационный класс или в класс приложения.

### Кэширование с Redis

#### 1. Добавьте конфигурацию Redis:

```java
@Configuration
@EnableCaching
public class RedisConfig {
    
    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        // Настройка сериализатора для значений кэша
        Jackson2JsonRedisSerializer<Object> serializer = new Jackson2JsonRedisSerializer<>(Object.class);
        
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.activateDefaultTyping(
                objectMapper.getPolymorphicTypeValidator(),
                ObjectMapper.DefaultTyping.NON_FINAL
        );
        serializer.setObjectMapper(objectMapper);
        
        // Настройка конфигурации кэша Redis
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofMinutes(10))  // Время жизни записей
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(serializer))
                .disableCachingNullValues();  // Не кэшировать null-значения
        
        // Создаем разные конфигурации для разных кэшей
        Map<String, RedisCacheConfiguration> cacheConfigurations = new HashMap<>();
        cacheConfigurations.put("users", config.entryTtl(Duration.ofMinutes(5)));  // Кэш пользователей на 5 минут
        
        return RedisCacheManager.builder(connectionFactory)
                .cacheDefaults(config)
                .withInitialCacheConfigurations(cacheConfigurations)
                .build();
    }
    
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        
        // Настройка сериализаторов
        Jackson2JsonRedisSerializer<Object> serializer = new Jackson2JsonRedisSerializer<>(Object.class);
        ObjectMapper mapper = new ObjectMapper();
        mapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        mapper.activateDefaultTyping(
                mapper.getPolymorphicTypeValidator(),
                ObjectMapper.DefaultTyping.NON_FINAL
        );
        serializer.setObjectMapper(mapper);
        
        // Сериализаторы для ключей и значений
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(serializer);
        template.setHashKeySerializer(new StringRedisSerializer());
        template.setHashValueSerializer(serializer);
        
        template.afterPropertiesSet();
        return template;
    }
}
```

#### 2. Добавьте настройки Redis в application.yml:

```yaml
spring:
  # Существующие настройки...
  redis:
    host: localhost
    port: 6379
    # password: если настроен
    timeout: 2000
  cache:
    type: redis
    redis:
      time-to-live: 600000
      cache-null-values: false
```

#### 3. Используйте аннотации кэширования в сервисе:

```java
@Service
@CacheConfig(cacheNames = "users")  // Указываем название кэша для всех методов класса
public class UserServiceImpl implements UserService {
    // ...
    
    @Override
    @Cacheable(key = "#id")  // Кэширование по ID пользователя
    public UserDto getById(Long id) {
        return userMapper.toDto(userRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("User not found")));
    }
    
    @Override
    @CachePut(key = "#result.id")  // Обновление кэша после создания 
    public UserDto create(UserDto dto) {
        User user = userMapper.toEntity(dto);
        return userMapper.toDto(userRepository.save(user));
    }
    
    @Override
    @CachePut(key = "#id")  // Обновление кэша после изменения
    public UserDto update(Long id, UserDto dto) {
        // ...код обновления...
        return userMapper.toDto(updatedUser);
    }
    
    @Override
    @CacheEvict(key = "#id")  // Удаление из кэша после удаления сущности
    public void delete(Long id) {
        // ...код удаления...
    }
    
    @CacheEvict(allEntries = true)  // Очистка всего кэша пользователей
    @Scheduled(fixedRate = 86400000)  // Раз в день (в мс)
    public void clearCache() {
        // Метод будет автоматически очищать кэш каждые 24 часа
    }
}
```

#### 4. Пример прямой работы с Redis:

```java
@Service
public class CustomCacheService {
    private final RedisTemplate<String, Object> redisTemplate;
    
    public CustomCacheService(RedisTemplate<String, Object> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }
    
    public void saveToCache(String key, Object value, Duration ttl) {
        redisTemplate.opsForValue().set(key, value, ttl);
    }
    
    public <T> T getFromCache(String key, Class<T> type) {
        return type.cast(redisTemplate.opsForValue().get(key));
    }
    
    public void removeFromCache(String key) {
        redisTemplate.delete(key);
    }
    
    // Использование для пользовательских сессий
    public void saveUserSession(Long userId, UserSession session) {
        String key = "session:user:" + userId;
        redisTemplate.opsForValue().set(key, session, Duration.ofHours(2));
    }
}
```