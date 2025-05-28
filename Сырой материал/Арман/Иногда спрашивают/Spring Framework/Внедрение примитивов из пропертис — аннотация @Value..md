Аннотация `@Value` в Spring используется для внедрения значений из файла конфигурации (например, `application.properties` или `application.yml`) или других источников в поля бина. Это простой способ привязать настройки из конфигурации к вашему коду.

Как работает @Value:
1. Spring считывает значения из конфигурационных файлов.
2. С помощью `@Value` вы указываете, какое значение нужно внедрить в конкретное поле.

Пример:
Файл `application.properties`:
app.name=MyApplication
app.version=1.0

Класс:
@Component
public class AppConfig {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String appVersion;

    public void printConfig() {
        System.out.println("App Name: " + appName);
        System.out.println("App Version: " + appVersion);
    }
}

Результат:
1. `appName` будет равно `"MyApplication"`.
2. `appVersion` будет равно `"1.0"`.

Внедрение примитивов напрямую
Мы можем внедрять значения:
1. Строки
2. Числа
3. Булевы значения

Пример:
@Value("Hello, world!") // Прямое значение
private String message;

@Value("42") // Целое число
private int number;

@Value("true") // Логическое значение
private boolean isActive;


Дополнительные возможности @Value
1. **Значения по умолчанию:** Если ключ отсутствует в конфигурации, вы можете указать значение по умолчанию.
   @Value("${app.port:8080}") // Если app.port отсутствует, будет использоваться 8080
	private int port;
2. Выражения Spring Expression Language (SpEL):
   @Value("#{5 + 10}") // Вставит результат выражения: 15
	private int result;
	
	@Value("#{systemProperties['user.home']}") // Путь к домашней директории
	private String userHome;
3. **Чтение списка или массива:**
   app.languages=en,fr,de
   
   @Value("${app.languages}")
   private String[] languages; // ["en", "fr", "de"]


Преимущества использования @Value:
1. Простое привязка конфигурации к коду.
2. Уменьшение хардкода.
3. Гибкость и поддержка значений по умолчанию.

Недостатки:
1. Сложность управления большими конфигурациями (лучше использовать `@ConfigurationProperties` для сложных случаев).