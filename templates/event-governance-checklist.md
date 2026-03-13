# Event Governance Checklist: [Название проекта]

Чеклист архитектурного качества для event-driven интеграций. Применяется на уровне [L2+](../docs/core-standard.md#уровни-rigor) при использовании [EDA-паттернов](../docs/practices.md#12-event-driven-architecture-eda).

## Event Design

- [ ] Все бизнес-события описаны в [AsyncAPI](../docs/artifacts.md#api-specifications) спецификации
- [ ] Используется naming convention для событий (рекомендуется [CloudEvents](https://cloudevents.io/))
- [ ] Каждое событие имеет уникальный `type` и `source`
- [ ] Payload содержит только необходимые данные (не "fat events")
- [ ] Версионирование событий определено (header `specversion` или отдельный topic)

## Schema Management

- [ ] Schema зарегистрирована в schema registry (Confluent Schema Registry, Apicurio и др.)
- [ ] Backward compatibility проверяется автоматически при изменении schema
- [ ] Стратегия совместимости определена: BACKWARD / FORWARD / FULL
- [ ] Schema evolution rules зафиксированы в [ADR](../docs/artifacts.md#architecture-decision-records-adr)

## Надёжность

- [ ] Dead Letter Queue (DLQ) настроена для каждого consumer
- [ ] Retry policy определена (количество попыток, backoff strategy)
- [ ] Idempotency обеспечена (idempotency key или дедупликация)
- [ ] Ordering guarantees определены (по partition key / без гарантий)
- [ ] Timeout для обработки сообщений настроен

## Observability

- [ ] Consumer lag мониторится (алерт при превышении порога)
- [ ] Error rate по consumer group отслеживается
- [ ] DLQ мониторится (алерт при появлении сообщений)
- [ ] End-to-end latency измеряется (от publish до consume)
- [ ] Distributed tracing пробрасывает correlation ID через события

## Операционная готовность

- [ ] Runbook для обработки DLQ (replay / manual fix / skip)
- [ ] Процедура schema rollback определена
- [ ] Capacity planning: оценён пиковый объём событий
- [ ] Retention policy для топиков/очередей определена
- [ ] Права доступа к топикам настроены (ACL / RBAC)

## Итоговая оценка

| Категория | Статус | Комментарий |
|-----------|:------:|------------|
| Event Design | OK / Частично / Не готово | |
| Schema Management | OK / Частично / Не готово | |
| Надёжность | OK / Частично / Не готово | |
| Observability | OK / Частично / Не готово | |
| Операционная готовность | OK / Частично / Не готово | |

**Решение:** готово к production / требуется доработка (указать что)

**Дата проверки:** YYYY-MM-DD
**Проверил:** [Имя, роль]
