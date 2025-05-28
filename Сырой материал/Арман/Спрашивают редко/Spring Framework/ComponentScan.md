**@ComponentScan** — это аннотация в Spring, которая указывает фреймворку, где искать компоненты, бины и конфигурации для автоматической регистрации в контексте приложения.

Простыми словами:
Spring автоматически сканирует указанные пакеты на наличие аннотаций, таких как:
1. @Component
2. @Service
3. @Repository
4. @Controller
И автоматически добавляет их в контекст приложения, чтобы они стали доступными для использования.

Зачем это нужно:
Когда мы создаём приложение, оно состоит из множества классов. Чтобы Spring знал, какие из них нужно зарегистрировать как бины, используется **@ComponentScan**.

Как это работает:
1. **Вы добавляете @ComponentScan**:
   Указываете пакет или несколько пакетов, которые нужно просканировать.
2. **Spring сканирует эти пакеты**:
   Он ищет классы с аннотациями вроде **@Component**, **@Service**, **@Repository**, **@Controller**.
3. **Spring регистрирует найденные классы как бины**:
   Эти классы становятся доступными для внедрения зависимостей через **@Autowired** или **@Inject**.

Пример:
У вас есть структура пакетов:

com.example.app
 ├── services
 │    └── MyService.java
 ├── controllers
 │    └── MyController.java

// MyService.java
@Service
public class MyService {
    public String getMessage() {
        return "Hello from MyService";
    }
}

// MyController.java
@Controller
public class MyController {
    @Autowired
    private MyService myService;

    public void printMessage() {
        System.out.println(myService.getMessage());
    }
}


@Configuration
@ComponentScan(basePackages = "com.example.app")
public class AppConfig {
}

Что делает @ComponentScan:
1. **Spring сканирует пакет `com.example.app`**.
2. Находит классы с **@Service** и **@Controller**.
3. Автоматически регистрирует **MyService** и **MyController** как бины в контексте приложения.

Без @ComponentScan:
Если мы не используем **@ComponentScan**, нам нужно будет вручную объявить все бины в конфигурации:
@Bean
public MyService myService() {
    return new MyService();
}

@Bean
public MyController myController() {
    return new MyController(myService());
}

Когда @ComponentScan полезен:
1. Когда у вас большое приложение, и вы хотите избежать ручной регистрации бинов.
2. Если используете автоматическую настройку Spring Boot, где он уже включает **@ComponentScan** по умолчанию.

**Итог:**  
**@ComponentScan** — это инструмент автоматизации, который избавляет вас от необходимости вручную регистрировать бины. Вы просто указываете, где их искать, а Spring сделает остальную работу.