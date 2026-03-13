# Data Contract

> Контракт данных между поставщиком и потребителем. Определяет schema, ownership, quality SLA и правила использования. Ведёт [Data Architect](../docs/roles.md#data-architect) или владелец домена.

## Метаданные контракта

| Параметр | Значение |
|----------|---------|
| **Название** | [Название data product / dataset] |
| **Версия** | 1.0.0 |
| **Владелец (domain)** | [Команда / домен] |
| **Data Owner** | [Имя, роль] |
| **Статус** | Draft / Active / Deprecated |
| **Дата создания** | YYYY-MM-DD |
| **Дата последнего обновления** | YYYY-MM-DD |

---

## Описание

[1–2 абзаца: что за данные, для чего предназначены, бизнес-контекст]

### Бизнес-назначение

- [Какие бизнес-задачи решают эти данные]
- [Кто основные потребители]

### Классификация данных

| Параметр | Значение |
|----------|---------|
| **Sensitivity** | Public / Internal / Confidential / Restricted |
| **PII** | Да / Нет (если да — какие поля) |
| **Регуляторика** | GDPR / ФЗ-152 / PCI DSS / Нет |
| **Retention** | [Срок хранения] |

---

## Schema

### Формат

- [ ] JSON Schema
- [ ] Avro
- [ ] Protobuf
- [ ] SQL DDL
- [ ] Другое: ___

### Определение полей

| Поле | Тип | Обязательное | Описание | Constraints | PII |
|------|-----|:---:|---------|------------|:---:|
| `id` | string (UUID) | ✅ | Уникальный идентификатор | Unique, not null | — |
| `customer_name` | string | ✅ | Имя клиента | Max 255 chars | ✅ |
| `email` | string | ✅ | Email клиента | Valid email format | ✅ |
| `order_total` | decimal | ✅ | Сумма заказа | ≥ 0 | — |
| `created_at` | timestamp | ✅ | Дата создания | ISO 8601, UTC | — |
| `status` | enum | ✅ | Статус | `pending`, `confirmed`, `shipped`, `delivered` | — |
| `metadata` | object | — | Дополнительные данные | Free-form JSON | — |

### Пример данных

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "customer_name": "Иван Петров",
  "email": "ivan@example.com",
  "order_total": 15000.00,
  "created_at": "2025-03-15T10:30:00Z",
  "status": "confirmed",
  "metadata": {
    "source": "web",
    "campaign_id": "spring2025"
  }
}
```

---

## Quality SLA

| Метрика | Target | Метод проверки | Алерт при нарушении |
|---------|--------|---------------|-------------------|
| **Freshness** | Данные обновляются каждые 5 мин | Timestamp последней записи | 🟡 Warning > 10 мин, 🔴 Critical > 30 мин |
| **Completeness** | ≥ 99.5% обязательных полей заполнены | % NULL в обязательных полях | 🟡 < 99.5%, 🔴 < 99% |
| **Uniqueness** | 0 дубликатов по `id` | Duplicate check по PK | 🔴 > 0 дубликатов |
| **Timeliness** | Pipeline завершается за ≤ 15 мин | Pipeline duration metric | 🟡 > 15 мин, 🔴 > 30 мин |
| **Accuracy** | Reconciliation error < 0.1% | Cross-source check (monthly) | 🔴 > 0.1% |

---

## Доступ

### Потребители

| Потребитель | Назначение | Уровень доступа | Способ доступа |
|------------|-----------|----------------|---------------|
| Analytics Team | Отчётность | Read-only | SQL / API |
| ML Platform | Training data | Read-only, filtered (no PII) | API + batch export |
| CRM Service | Синхронизация | Read-only | Event stream |

### Механизм доступа

- [ ] API (REST / GraphQL)
- [ ] SQL query (read replica)
- [ ] Event stream (Kafka topic)
- [ ] Batch export (S3 / GCS)
- [ ] Другое: ___

### Авторизация

- Механизм: [RBAC / ABAC / API keys]
- Запрос доступа: [процедура]
- Ревью доступов: [ежеквартально]

---

## Lineage

### Источники данных

```
[Source System A] → [ETL/Streaming] → [This Dataset] → [Consumer 1]
                                                      → [Consumer 2]
```

| Источник | Тип интеграции | Частота | Трансформации |
|----------|---------------|---------|--------------|
| Orders DB | CDC (Debezium) | Real-time | Denormalization, enrichment |
| Customer API | REST poll | Every 5 min | Mapping, deduplication |

### Impact Analysis

При изменении этого контракта будут затронуты:
- [Список downstream consumers]
- [Процедура уведомления]

---

## Versioning

### Правила

- Добавление опционального поля → minor version (1.0.0 → 1.1.0)
- Изменение типа / удаление поля → major version (1.0.0 → 2.0.0)
- Major version = новый контракт + deprecation period для старого (≥ 3 месяца)
- Все изменения фиксируются в [ADR](../docs/practices.md#1-architecture-decision-records-adr)

### Changelog

| Версия | Дата | Изменение | Backward compatible |
|--------|------|----------|:---:|
| 1.0.0 | 2025-01-15 | Initial version | — |
| 1.1.0 | 2025-03-01 | Added `metadata` field (optional) | ✅ |

---

## Мониторинг и поддержка

### Контакты

| Роль | Контакт | Канал |
|------|---------|-------|
| Data Owner | [Имя] | Slack: #data-orders |
| On-call | [Команда] | PagerDuty |

### Процедура при инциденте

1. Алерт → on-call получает уведомление
2. Triage: определить impact на consumers
3. Fix или fallback
4. Post-mortem → обновление SLA/мониторинга при необходимости
