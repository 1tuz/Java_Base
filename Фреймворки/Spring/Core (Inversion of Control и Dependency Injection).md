#Spring #di #IoC #паттерны #архитектура 
### **Core (IoC и DI)**

**IoC (Inversion of Control)** — это подход, при котором управление зависимостями передается контейнеру, а не прописывается в коде.

- **Dependency Injection (DI)**: Автоматическое внедрение зависимостей между компонентами.
- **ApplicationContext**: Центральный интерфейс Spring для конфигурации приложения.
- Основные способы настройки: XML, аннотации или Java Config.
- Преимущества: модульность, простота тестирования и расширяемость.


✅ IoC (Inversion of Control) — контейнер управляет созданием и внедрением бинов.  
✅ DI (Dependency Injection) — внедрение зависимостей (`@Component`, `@Service`, `@Repository`, `@Autowired`).  
✅ ApplicationContext — основной контейнер Spring.