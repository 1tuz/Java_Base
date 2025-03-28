#hibernate #cache #кэш
### **Кэш первого уровня (L1 Cache)**

- **Работает в пределах сессии** (`Session`).
- **Включён по умолчанию**, не требует настройки.
- **Хранит загруженные сущности**, повторные запросы к ним идут из кэша.
- **Очищается при закрытии сессии**.

### **Кэш второго уровня (L2 Cache)**

- **Разделяется между сессиями**, снижая нагрузку на БД.
- **Не включён по умолчанию**, требует настройки (`hibernate.cfg.xml`).
- **Использует сторонние кэш-провайдеры** (EhCache, Redis, Infinispan).
- **Работает только для сущностей**, но можно настроить кэширование коллекций и запросов.

Кэш **L1** быстрый, но краткоживущий. **L2** повышает производительность, но требует настройки.