@Primary - аннотация, указывающая, какой бин (компонент) использовать **по умолчанию**, если есть несколько кандидатов для внедрения.

Когда использовать?
Когда у вас есть несколько бинов одного типа, и вы хотите указать **основной** бин.

Пример:
@Component
@Primary
public class MainService implements Service {
    // Основная реализация
}

@Component
public class BackupService implements Service {
    // Резервная реализация
}

@Autowired
private Service service; // Всегда будет использовать MainService


@Qualifier - аннотация, позволяющая точно указать, какой бин из нескольких внедрять.

Когда использовать?
Когда у вас несколько бинов одного типа, и вы хотите явно указать, какой из них использовать.

Пример:
@Component("mainService")
public class MainService implements Service {
    // Основная реализация
}

@Component("backupService")
public class BackupService implements Service {
    // Резервная реализация
}

@Autowired
@Qualifier("backupService")
private Service service; // Явно указываем BackupService

![[Pasted image 20241219014230.png]]

Итог:
1. Используйте `@Primary` для указания основного бина.
2. Используйте `@Qualifier`, когда нужно внедрить определённый бин.