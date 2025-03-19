#Spring #spring_security #jwt #oauth #Аннотации 
🔹 **Spring Security – фреймворк для аутентификации и авторизации**

✅ **Базовая настройка:**
- Используется аннотация `@EnableWebSecurity` для конфигурации безопасности.
- Создается класс, наследующий `SecurityFilterChain`, где настраиваются правила доступа.

✅ **Поддержка различных методов аутентификации:**
- **OAuth2** – аутентификация через внешние сервисы (Google, GitHub и др.).
- **JWT (JSON Web Token)** – токенизированная аутентификация для REST API.
- **Базовая авторизация (Basic Auth)** – передача логина/пароля в заголовке запроса.

✅ **Гранулярный контроль доступа:**
- `@PreAuthorize("hasRole('ADMIN')")` – ограничение доступа к методам.
- `@Secured("ROLE_USER")` – альтернатива `@PreAuthorize`, проверяет роли.
- `@RolesAllowed({"USER", "ADMIN"})` – аннотация из JSR-250 для проверки ролей.

🔹 **Аутентификация**  
✅ `UsernamePasswordAuthenticationToken` — стандартная аутентификация.  
✅ `UserDetailsService` — загрузка пользователей.  
✅ `AuthenticationManager` — проверка логина/пароля.  
✅ `JwtAuthenticationFilter` — аутентификация через JWT.

🔹 **Авторизация**  
✅ `@PreAuthorize/@PostAuthorize` — проверка ролей на методах.  
✅ `@Secured` — ограничение доступа по ролям.  
✅ `HttpSecurity` — настройка доступа к URL.

🔹 **Защита**  
✅ CSRF — защита от подделки запросов (`csrf().disable()` если API).  
✅ CORS — настройка доступа между доменами.  
✅ `BCryptPasswordEncoder` — хеширование паролей.