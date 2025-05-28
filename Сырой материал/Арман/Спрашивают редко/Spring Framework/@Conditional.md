**@Conditional** — это аннотация в Spring, которая позволяет создавать бины и выполнять конфигурации только при выполнении определённого условия.

Простыми словами:
1. "Если условие выполнено, то бин создаётся."
2. "Если условие не выполнено, бин игнорируется."
Spring проверяет условия, указанные в классе или методе, помеченном аннотацией **@Conditional**, и принимает решение, включать ли этот компонент/бин в контекст приложения.

Зачем это нужно:
1. Для настройки приложения в зависимости от окружения (например, dev/prod).
2. Для включения функционала только при наличии определённых библиотек или классов.
3. Для реализации гибкой конфигурации, зависящей от внешних факторов.

Пример 1: Включение бина в зависимости от профиля

	@Configuration
	public class AppConfig {
	
	    @Bean
	    @Conditional(DevCondition.class)
	    public MyService devService() {
	        return new MyService("Development Service");
	    }
	
	    @Bean
	    @Conditional(ProdCondition.class)
	    public MyService prodService() {
	        return new MyService("Production Service");
	    }
	}
	
	public class DevCondition implements Condition {
	    @Override
	    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
	        return "dev".equals(context.getEnvironment().getProperty("spring.profiles.active"));
	    }
	}
	
	public class ProdCondition implements Condition {
	    @Override
	    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
	        return "prod".equals(context.getEnvironment().getProperty("spring.profiles.active"));
	    }
	}

- Если в **application.properties** указан профиль `spring.profiles.active=dev`, будет создан только бин **devService()**.
- Если профиль `prod`, создаётся бин **prodService()**.


Пример 2: Проверка наличия класса

	@Configuration
	public class AppConfig {
	
	    @Bean
	    @Conditional(ClassCondition.class)
	    public MyService myService() {
	        return new MyService("Service Available");
	    }
	}
	
	public class ClassCondition implements Condition {
	    @Override
	    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
	        try {
	            context.getClassLoader().loadClass("com.example.SomeClass");
	            return true; // Бин создаётся, если класс найден
	        } catch (ClassNotFoundException e) {
	            return false; // Бин не создаётся
	        }
	    }
	}

Если класс `com.example.SomeClass` существует, Spring создаёт бин `myService()`.


Когда полезно:
1. Гибкая конфигурация:
   Вы можете настроить приложение так, чтобы оно адаптировалось к окружению (dev/prod).
2. Легковесность:
   Spring не создаёт ненужные бины, экономя ресурсы.
3. Проверка зависимостей:
   Если библиотека или класс отсутствуют, соответствующий функционал не будет включён.

Итог:
**@Conditional** позволяет гибко управлять конфигурацией приложения, создавая бины только при выполнении определённых условий. Это помогает делать приложение более адаптивным и экономным в плане использования ресурсов.