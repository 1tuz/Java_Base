🔹 **MVC (Model-View-Controller)**  
✅ **Model**: Данные и бизнес-логика (например, сущности `@Entity`, сервисы).  
✅ **View**: Представление (HTML, Thymeleaf, JSON для REST API).  
✅ **Controller**: Обработка HTTP-запросов (`@Controller`, `@RestController`).  
✅ **Spring MVC Flow**:  
1. Запрос → `DispatcherServlet` (front-controller).  
2. Роутинг через `@RequestMapping` (`@GetMapping`, `@PostMapping`).  
3. Вызов сервисного слоя → обновление Model.  
4. Возврат View (или данных через `@ResponseBody`).  

✅ **Ключевые аннотации**:  
- `@Controller` / `@RestController`  
- `@RequestMapping`, `@RequestParam`, `@PathVariable`  
- `@ModelAttribute` (биндинг данных формы).  

❌ **Типовые ошибки**:  
- Смешивание логики в контроллере (нужно выносить в сервисы).  
- Игнорирование валидации входных данных (`@Valid`).  
- Плохая обработка исключений (решается через `@ExceptionHandler`, `@ControllerAdvice`).  

**Пример REST-контроллера:**  
```java  
@RestController  
@RequestMapping("/api/users")  
public class UserController {  
    @Autowired  
    private UserService userService;  

    @GetMapping("/{id}")  
    public User getUser(@PathVariable Long id) {  
        return userService.findById(id);  
    }  
}  
```  

✅ **Плюсы**:  
- Четкое разделение ответственности.  
- Упрощает тестирование (моки сервисов, `MockMvc`).  

❌ **Минусы**:  
- Избыточность для простых задач (можно использовать Spring WebFlux для реактивных сценариев).