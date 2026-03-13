# Cost Model: [Название решения / проекта]

> Модель стоимости архитектурного решения. Используется при проектировании, в [ADR](adr-template.md), при [FinOps Assessment](finops-assessment.md) и в [Architecture Assessment Report](assessment-report.md). Ведёт [Solution Architect](../docs/roles.md#solution-architect).

---

## Метаданные

| Параметр | Значение |
|----------|---------|
| **Проект** | [Название] |
| **Решение / ADR** | ADR-NNNN: [Заголовок] |
| **Облачный провайдер** | [AWS / Azure / GCP / On-prem / Hybrid] |
| **Период оценки** | [12 / 24 / 36 месяцев] |
| **Дата оценки** | YYYY-MM-DD |
| **Автор** | [Имя, роль] |

---

## Компоненты стоимости

### Compute

| Компонент | Конфигурация | Кол-во | Стоимость/мес | Модель оплаты | Примечание |
|-----------|-------------|:------:|--------------:|--------------|------------|
| App servers (K8s) | 4 vCPU / 16 GB | 3 | 45 000 ₽ | Reserved 1yr | Baseline нагрузка |
| Workers | 2 vCPU / 8 GB | 0–5 | 0–25 000 ₽ | Spot | Autoscaling, fault-tolerant |
| Serverless functions | 256 MB, ~100K invoc/мес | — | 3 000 ₽ | Pay-per-use | Event processing |

### Data

| Компонент | Конфигурация | Объём | Стоимость/мес | Примечание |
|-----------|-------------|------|-------------:|------------|
| PostgreSQL (managed) | 4 vCPU / 16 GB × 2 | 200 ГБ | 30 000 ₽ | Primary + replica |
| Redis (cache) | 2 vCPU / 8 GB | 10 ГБ | 8 000 ₽ | Cluster mode |
| Object Storage | — | 500 ГБ | 5 000 ₽ | Hot + lifecycle to cold |

### Network & Integration

| Компонент | Конфигурация | Объём | Стоимость/мес | Примечание |
|-----------|-------------|------|-------------:|------------|
| Load Balancer (L7) | 1 шт | — | 3 000 ₽ | |
| Исходящий трафик | — | 2 ТБ | 4 000 ₽ | |
| CDN | — | 500 ГБ | 5 000 ₽ | Статика + API caching |
| Message Broker | Managed Kafka, 3 брокера | — | 15 000 ₽ | Integration events |

### Observability & Operations

| Компонент | Конфигурация | Стоимость/мес | Примечание |
|-----------|-------------|-------------:|------------|
| Monitoring (Prometheus + Grafana) | Managed | 8 000 ₽ | Metrics + dashboards |
| Logging | — | 5 000 ₽ | 30 дней retention |
| Tracing | — | 3 000 ₽ | Sampling 10% |

### Итого (baseline)

| Категория | Стоимость/мес |
|-----------|-------------:|
| Compute | 73 000 ₽ |
| Data | 43 000 ₽ |
| Network | 27 000 ₽ |
| Observability | 16 000 ₽ |
| **Итого** | **159 000 ₽** |

---

## Прогноз роста

| Период | Пользователи (MAU) | RPS | Стоимость/мес | Ключевые действия |
|--------|:------------------:|:---:|--------------:|-------------------|
| Запуск | 1 000 | 100 | 159 000 ₽ | Базовая конфигурация |
| +6 мес | 5 000 | 500 | 220 000 ₽ | Горизонтальное масштабирование compute |
| +12 мес | 10 000 | 1 000 | 350 000 ₽ | + read-реплика, CDN, caching |
| +24 мес | 50 000 | 5 000 | 600 000 ₽ | Шардирование БД, data tiering |

---

## Unit Economics

| Метрика | Текущее значение | Target | Тренд |
|---------|:----------------:|:------:|:-----:|
| Cost per transaction | 0.3 ₽ | < 0.5 ₽ | ✅ |
| Cost per MAU | 159 ₽ | < 100 ₽ (при 10K) | ➡️ |
| Infra / Revenue ratio | — | < 20% | — |

---

## Сравнение вариантов (для ADR)

> Заполняется при выборе между архитектурными альтернативами.

| Компонент | Вариант A: [Managed] | Вариант B: [Self-hosted] | Вариант C: [Serverless] |
|-----------|--------------------:|------------------------:|------------------------:|
| Compute | 45 000 ₽ | 30 000 ₽ | 15 000 ₽ |
| Operations (FTE) | 0 ₽ | 80 000 ₽ (0.5 FTE) | 0 ₽ |
| Licensing | 0 ₽ | 0 ₽ | 0 ₽ |
| **Итого/мес** | **45 000 ₽** | **110 000 ₽** | **15 000 ₽** |
| **TCO 12 мес** | **540 000 ₽** | **1 320 000 ₽** | **180 000 ₽** |
| Риски | Vendor lock-in | Экспертиза, обновления | Cold start, лимиты |

---

## Оптимизация

| Рекомендация | Потенциальная экономия | Сложность | Приоритет |
|-------------|:----------------------:|:---------:|:---------:|
| Reserved instances (1 год) на compute | ~30% (13 500 ₽/мес) | Low | P1 |
| Spot instances для workers | ~70% (до 17 500 ₽/мес) | Medium | P1 |
| Data lifecycle policies (cold storage после 90д) | ~60% на storage | Low | P2 |
| CDN для статики | ~50% на трафик | Low | P2 |
| Rightsizing после 1 мес метрик | ~20% на compute | Low | P2 |

---

## Мониторинг и алерты

| Метрика | Warning | Critical | Ответственный |
|---------|---------|----------|---------------|
| Ежемесячные расходы | > 70% бюджета | > 90% бюджета | Архитектор, PM |
| Cost per transaction | > 0.5 ₽ | > 1 ₽ | Архитектор |
| Daily spend anomaly | +30% от нормы | +50% от нормы | DevOps |
| Unused resources | Любые (7д idle) | — | DevOps |

---

## Связанные артефакты

- [ADR](adr-template.md): решение, для которого создана модель
- [FinOps Assessment](finops-assessment.md): полная оценка стоимости владения
- [NFR Checklist](nfr-checklist.md): Cost и Sustainability категории
- [SLO/SLI Profile](slo-sli-profile.md): Cost SLO
- [Tech Debt Register](tech-debt-register.md): стоимость техдолга в деньгах
