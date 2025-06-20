**Singleton (Одиночка)** — это шаблон проектирования, который гарантирует, что **в приложении будет только один экземпляр класса**, и предоставляет глобальную точку доступа к этому экземпляру.

Зачем нужен Singleton:
1. Чтобы **контролировать доступ** к ресурсу, который должен существовать в единственном экземпляре (например, соединение с базой данных, логгер или конфигурационный файл).
2. Чтобы обеспечить **глобальный доступ** к объекту (например, настройки приложения).

Как работает Singleton:
1. Класс создает **единственный экземпляр** объекта.
2. Этот объект доступен через специальный метод (обычно `getInstance()` или аналог).
3. Повторные вызовы этого метода возвращают один и тот же экземпляр.

Как реализовать Singleton в Java:
1. Простой (ленивый) Singleton - экземпляр создается **только при первом запросе**.
	public class Singleton {
	    private static Singleton instance;
	
	    private Singleton() {
	        // Конструктор приватный, чтобы нельзя было создать объект извне
	    }
	
	    public static Singleton getInstance() {
	        if (instance == null) {
	            instance = new Singleton();
	        }
	        return instance;
	    }
	}
	**Недостаток:** Не потокобезопасно — если два потока одновременно вызовут `getInstance()`, могут создать два объекта.
2. Потокобезопасный Singleton (с synchronized) - обеспечивает потокобезопасность, но может быть медленным из-за блокировок.
	public class Singleton {
	    private static Singleton instance;
	
	    private Singleton() { }
	
	    public static synchronized Singleton getInstance() {
	        if (instance == null) {
	            instance = new Singleton();
	        }
	        return instance;
	    }
	}
	**Недостаток:** Синхронизация делает вызовы `getInstance()` медленнее
3. "Double-Checked Locking" (двойная проверка) - балансирует между производительностью и безопасностью.
	public class Singleton {
	    private static volatile Singleton instance;
	
	    private Singleton() { }
	
	    public static Singleton getInstance() {
	        if (instance == null) {
	            synchronized (Singleton.class) {
	                if (instance == null) {
	                    instance = new Singleton();
	                }
	            }
	        }
	        return instance;
	    }
	}
	**Преимущества:** Потокобезопасно и быстрее, так как синхронизация нужна только один раз.
4. Singleton через **Enum** - самый безопасный способ в Java, гарантирует один экземпляр.
	public enum Singleton {
	    INSTANCE;
	
	    public void doSomething() {
	        System.out.println("Singleton через Enum работает!");
	    }
	}
	**Преимущества:** Простота, потокобезопасность "из коробки" и защита от сериализации.

Плюсы и минусы Singleton
Плюсы:
1. Гарантирует единственный экземпляр объекта.
2. Экономит ресурсы при использовании "ленивой" инициализации.
3. Легко организовать глобальный доступ.
Минусы:
1. Может привести к сложности тестирования (трудно "заменить" объект на мок).
2. Нарушает принцип единственной ответственности (S из SOLID), так как объединяет логику управления экземпляром и бизнес-логику.
3. Может стать узким местом в многопоточных приложениях, если неправильно реализован.

Когда использовать Singleton: Когда нужно управлять ресурсом, который должен быть в единственном экземпляре. Примеры: логгирование, настройки приложения, пул соединений с базой данных.


