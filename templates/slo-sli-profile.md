# SLO/SLI Profile

> Профиль надёжности и производительности системы. Определяет целевые показатели (SLO), способы их измерения (SLI) и правила реагирования. Ведёт [Technical Architect](../docs/roles.md#technical-architect-technology-architect).

## Система

| Параметр | Значение |
|----------|---------|
| **Система** | [Название] |
| **Владелец** | [Команда / архитектор] |
| **Критичность** | Tier 1 (business-critical) / Tier 2 / Tier 3 |
| **Пользователи** | [~N пользователей, ~M RPS] |
| **Дата определения** | [YYYY-MM-DD] |
| **Пересмотр** | Ежеквартально |

---

## SLO / SLI

### Availability

| Параметр | Значение |
|----------|---------|
| **SLO** | 99.9% за календарный месяц |
| **SLI** | (успешные запросы / все запросы) × 100% |
| **Что считается успешным** | HTTP 2xx, 3xx, 4xx (клиентские ошибки не считаются отказом) |
| **Что считается отказом** | HTTP 5xx, timeout > 10s, connection refused |
| **Error budget** | 43.2 минуты / месяц |
| **Источник данных** | Prometheus: `http_requests_total{status=~"5.."}` |
| **Dashboard** | [ссылка на Grafana dashboard] |

### Latency

| Параметр | Значение |
|----------|---------|
| **SLO (p50)** | < 100ms |
| **SLO (p95)** | < 300ms |
| **SLO (p99)** | < 500ms |
| **SLI** | Гистограмма latency HTTP-ответов |
| **Scope** | Все API endpoints (исключая health checks) |
| **Источник данных** | OpenTelemetry → Prometheus: `http_request_duration_seconds` |
| **Dashboard** | [ссылка] |

### Throughput

| Параметр | Значение |
|----------|---------|
| **SLO** | Система выдерживает ≥ 1000 RPS без деградации latency SLO |
| **SLI** | Текущий RPS + latency p99 |
| **Источник данных** | Prometheus: `rate(http_requests_total[5m])` |

### Data Durability

| Параметр | Значение |
|----------|---------|
| **SLO** | 0 потерь подтверждённых транзакций |
| **RPO** | ≤ 1 час |
| **RTO** | ≤ 30 минут |
| **SLI** | Успешность репликации, возраст последнего бэкапа |
| **Источник данных** | PostgreSQL replication lag, backup job status |

---

## Error Budget Policy

### Принципы

- Error budget = допустимое количество «плохих минут» за период
- Пока budget не исчерпан → приоритет у feature development
- Когда budget < 50% → усиленный мониторинг, архитектор оценивает риски
- Когда budget < 20% → приостановка feature work, фокус на reliability
- Когда budget = 0% → только reliability work до восстановления

### Таблица error budget

| SLO | Период | Допустимый downtime | Budget burned | Статус |
|-----|--------|-------------------|---------------|--------|
| 99.9% availability | Месяц | 43.2 мин | 15 мин (35%) | 🟢 OK |
| p99 < 500ms | Месяц | 0.1% запросов | 0.03% | 🟢 OK |

---

## Алертинг

| Алерт | Условие | Severity | Действие |
|-------|---------|----------|----------|
| High error rate | 5xx > 1% за 5 мин | 🔴 Critical | Страница дежурному, эскалация через 15 мин |
| Latency degradation | p99 > 500ms за 10 мин | 🟡 Warning | Уведомление в канал, расследование |
| Error budget burn rate | Budget расходуется 10x быстрее нормы | 🔴 Critical | Страница дежурному + архитектору |
| Backup failure | Бэкап не выполнен > 2 часов | 🟡 Warning | Уведомление DevOps |
| Replication lag | Lag > 30 сек | 🟡 Warning | Уведомление DBA |

---

## Зависимости и их SLO

| Зависимость | Тип | SLA провайдера | Наш SLO | Fallback |
|------------|-----|---------------|---------|----------|
| PostgreSQL (managed) | Database | 99.95% | 99.9% | Read replica |
| Redis | Cache | 99.9% | 99.5% | Graceful degradation (без кэша) |
| External API A | Integration | 99.5% | — | Circuit breaker + кэш |
| Message Broker | Async | 99.95% | 99.9% | Retry queue |

---

## Capacity Planning

| Метрика | Текущее | Лимит | Headroom | Действие при >80% |
|---------|---------|-------|----------|-------------------|
| CPU utilization | 40% | 80% | 50% | Autoscale |
| Memory | 60% | 85% | 29% | Добавить ноду |
| Disk I/O | 30% | 70% | 57% | Оптимизация запросов |
| Connections | 200 | 500 | 60% | Connection pooling |

---

## Процесс пересмотра

1. **Ежемесячно**: проверка error budget, анализ инцидентов, корректировка алертов
2. **Ежеквартально**: пересмотр SLO targets (не чаще — SLO должен быть стабильным)
3. **При инциденте**: post-mortem → обновление SLI/алертов → возможная корректировка SLO
4. **При изменении архитектуры**: переоценка зависимостей и capacity planning
