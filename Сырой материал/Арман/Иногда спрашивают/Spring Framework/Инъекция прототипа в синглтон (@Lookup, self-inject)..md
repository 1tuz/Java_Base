Инъекция прототипа (Prototype) в синглтон (Singleton) в Spring — это способ, позволяющий использовать бин с областью действия "Prototype" внутри бина с областью действия "Singleton". Здесь есть проблема: синглтон создаётся один раз на всё приложение, а прототип должен создаваться каждый раз, когда он нужен.

Проблема:
1. Синглтон **создаётся один раз** и хранит свои зависимости в неизменном виде.
2. Если вы внедрите бин-прототип напрямую в синглтон, он создаётся **один раз** при создании синглтона, и вы не получите новые экземпляры прототипа.

Способы решения
1. Использование аннотации @Lookup
   Spring создаёт динамический метод, который каждый раз возвращает новый экземпляр прототипа.
   Как это работает:
	   1) Вы создаёте метод в синглтоне, помеченный `@Lookup`, который возвращает бин-прототип.
	   2) Spring автоматически подставляет реализацию этого метода во время выполнения.
	      
	Пример:
		@Component
		public class SingletonBean {
		
		    @Lookup
		    public PrototypeBean getPrototypeBean() {
		        // Реализация подставляется Spring
		        return null;
		    }
		
		    public void usePrototype() {
		        PrototypeBean prototypeBean = getPrototypeBean();
		        System.out.println(prototypeBean);
		    }
		}
		
		@Component
		@Scope("prototype")
		public class PrototypeBean {
		    // Поля и методы прототипа
		}
	Преимущества:
	1. Простое решение.
	2. Не нужно писать дополнительный код для управления зависимостью.
	   
2. Self-injection (внедрение через сам объект)
   Как это работает:
   Синглтон внедряет самого себя через Spring-контекст, чтобы вручную запрашивать экземпляры прототипа.
   
   Пример:
   @Component
	public class SingletonBean {
	
	    @Autowired
	    private ApplicationContext context;
	
	    public void usePrototype() {
	        PrototypeBean prototypeBean = context.getBean(PrototypeBean.class);
	        System.out.println(prototypeBean);
	    }
	}
	
	@Component
	@Scope("prototype")
	public class PrototypeBean {
	    // Поля и методы прототипа
	}
	
	Преимущества:
	1) Полный контроль над созданием экземпляров.
	2) Можно использовать с кастомной логикой.
	
	Недостатки:
	Зависимость от `ApplicationContext` делает ваш код менее гибким.
	
	![[Pasted image 20241219015228.png]]
   
Что выбрать:
1. Если вам нужен простой способ, используйте `@Lookup`.
2. Если нужно больше контроля, используйте self-injection через `ApplicationContext`.