В Spring есть три основных способа, как объявить Bean, чтобы Spring знал, что он должен управлять этим объектом:
1. @Bean (Через Java-конфигурацию) - аннотация `@Bean` используется в классе с аннотацией `@Configuration`. Это означает, что ты сам пишешь метод, который создает объект, и Spring будет использовать его как Bean.
   Когда использовать:
	   1) Когда нужно вручную настроить Bean.
	   2) Если мы хотим контролировать, что именно передать в конструктор или вызвать методы объекта перед его использованием.
2. @Component (Автоматическое сканирование) - аннотация `@Component` говорит Spring: "Этот класс — Bean". Spring сам найдет такие классы (если настроено сканирование пакетов) и создаст Bean.
   Плюсы: Не нужно явно указывать, что делать с объектом, Spring сам создаст его.
   Когда использовать: Если объект простой, и его не нужно специально конфигурировать.
3. XML-конфигурация (Старый способ) - в этом случае мы указываем Bean в XML-файле, и Spring читает этот файл для создания объектов.

![[Pasted image 20241209174726.png]]
