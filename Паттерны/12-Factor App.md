---
aliases:
  - "**12-Factor App**"
---
🔹  (принципы разработки облачных приложений)  
✅ **Кодовая база** — один репозиторий, несколько деплоев.  
✅ **Зависимости** — явное управление (`Maven/Gradle`).  
✅ **Конфигурация** — хранение в окружении (`env vars`).  
✅ **Бэкапы** — хранение в БД, а не в коде.  
✅ **Сервис как процесс** — запуск в изолированном окружении.  
✅ **Порт-биндинг** — приложение само управляет HTTP-сервером.  
✅ **Конкаррентность** — масштабирование через процессы.  
✅ **Разделение сред** — одинаковые образы для dev/prod.  
✅ **Логирование** — поток stdout, сбор через `ELK/Grafana`.  
✅ **Администрирование** — задачи через одноразовые процессы.
