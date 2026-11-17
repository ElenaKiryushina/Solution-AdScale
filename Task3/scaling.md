**Масштабирование баз данных**

1.      Campaign Service.
a.       Репликация Master‑Slave
b.      CQRS (Command‑Query Responsibility Segregation) – запись в PostgreSQL, чтение Redis или read-replica
c.       Шардирование – если 1 инстанс БД не справляется с нагрузкой – ключ id кампании
2.      RTB Service
a.       Шардирование – если 1 инстанс БД не справляется с нагрузкой – ключ id request DSP
b.       Redis‑cache (read‑only) – для быстрого чтения кампаний, бюджетов и правил.
c.       CQRS (Command‑Query Responsibility Segregation) – чтение входящего потока из Redis, запись победителя в PostgreSQL, передача данных о победителе для потребителей данных – через Kafka.
3.       Statistics
a.       Шардирование – ключ время события (timestamp)
b.       ClickHouse реплицирует каждый шард (ReplicaSet), поэтому отдельные read‑replica не нужны.
4.       Analytics
a.       ClickHouse реплицирует каждый шард (ReplicaSet), поэтому отдельные read‑replica не нужны.
5.       Financial
a.       Репликация Master‑Slave
b.      Шардирование – если 1 инстанс БД не справляется с нагрузкой – ключ id transaction