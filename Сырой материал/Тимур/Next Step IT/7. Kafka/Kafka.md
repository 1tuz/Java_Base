Спрашивают постоянно
[[1. Зачем нужна. Какие проблемы решает. Какие преимущества. Какие основные компоненты в архитектуре Kafka]]
[[2. Основные понятия(Топики, партиции, зукипер, consumer, producer)]] 
[[3. Механизмы гарантированной доставки сообщений. Как не терять данные (никак) Как гарантировать сохранность сообщений в Kafka при сбоях]]
[[4. Порядок чтения. Partition Key, offset]]
[[5. Что такое consumer group]]
[[6. CAP теорема]]
[[7. За счёт чего достигается отказоустойчивость и масштабируемость]]
Иногда спрашивают
[[1. Как данные распределяются по партициям]]
[[2. Репликация в кафке. Leader-Follower. Что куда записывается, откуда читается (Acks min.insync.replicas, replication-factor)]]
[[3. Семантики доставки(Delivery semantic)]]
[[4. Можно ли менять кол-во партиций]]
[[5. Как удалить данные из топика]]
Спрашивают редко
[[1. Какие еще конфигурационные параметры можно настроить для оптимизации работы]]
[[2. Как задать кол-во партиций]]
[[3. Какие факторы могут повлиять на производительность Kafka-кластера]]
[[4. Как хранятся данные в кафке. Механизм хранения, Сценарии использования]]
[[5. Типы commit-ов в кафке]]
Очень редкие: 
1. Дефолтное время хранения сообщения в топике(7 дней) 
2. Какие метрики доступны для мониторинга и отслеживания производительности Kafka? 
3. Как прочитать сообщения в нужном порядке 
4. Стратегии сбалансированного потребления сообщений в Kafka? 
5. Что такое Kafka Connect и для чего он используется?