1. **Настройка Spring Security**:
	1) **Добавление зависимости**: В проект нужно добавить зависимость для Spring Security в `pom.xml` (для Maven) или `build.gradle` (для Gradle).
	2) **Конфигурация безопасности**: Создаём класс конфигурации, который будет управлять всеми аспектами безопасности, например, настройкой аутентификации и авторизации.
	
	Пример:
		@Configuration
		@EnableWebSecurity
		public class SecurityConfig extends WebSecurityConfigurerAdapter {
		    @Override
		    protected void configure(HttpSecurity http) throws Exception {
		        http
		            .authorizeRequests()
		                .antMatchers("/admin/**").hasRole("ADMIN")
		                .antMatchers("/user/**").authenticated()
		                .anyRequest().permitAll()
		            .and()
		            .formLogin();
		    }
		}
		
2. **Создание собственных фильтров**: В Spring Security можно создать свой фильтр для обработки запросов (например, для аутентификации, логирования, проверки прав доступа и других действий):
	   1) **Реализация фильтра**: Создаём класс, который расширяет `OncePerRequestFilter` или реализует интерфейс `Filter`.
	
	Пример:
		@Component
		public class CustomFilter extends OncePerRequestFilter {
		    @Override
		    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
		        // Логика фильтра (например, логирование или аутентификация)
		        System.out.println("Request received at " + request.getRequestURI());
		
		        // Передаем запрос в следующий фильтр или контроллер
		        filterChain.doFilter(request, response);
		    }
		}
		
	2) **Регистрация фильтра**: Фильтр нужно зарегистрировать в конфигурации безопасности.
	   
	Пример:
		@Configuration
		public class SecurityConfig extends WebSecurityConfigurerAdapter {
		    @Autowired
		    private CustomFilter customFilter;
		
		    @Override
		    protected void configure(HttpSecurity http) throws Exception {
		        http.addFilterBefore(customFilter, UsernamePasswordAuthenticationFilter.class);
		    }
		}

Кратко:
1. **Настройка безопасности**: Создаём конфигурационный класс с настройками авторизации и аутентификации.
2. **Собственные фильтры**: Создаём класс фильтра для обработки запросов и регистрируем его в конфигурации безопасности.


