# Architecture Risk Register

> Реестр архитектурных рисков проекта. Ведёт [Solution Architect](../docs/roles.md#solution-architect), пересматривается регулярно (спринт/месяц).

## Матрица оценки рисков

|  | **Low Impact** | **Medium Impact** | **High Impact** | **Critical Impact** |
|--|:---:|:---:|:---:|:---:|
| **High Probability** | 🟡 Medium | 🟡 High | 🔴 Critical | 🔴 Critical |
| **Medium Probability** | 🟢 Low | 🟡 Medium | 🟡 High | 🔴 Critical |
| **Low Probability** | 🟢 Low | 🟢 Low | 🟡 Medium | 🟡 High |

---

## Реестр

| ID | Риск | Категория | Вероятность | Импакт | Критичность | Mitigation | Owner | Статус | Дата |
|----|------|-----------|-----------|--------|-------------|-----------|-------|--------|------|
| R-001 | Внешний API-провайдер изменит контракт без предупреждения | Integration | Medium | High | 🟡 High | Contract testing в CI, fallback на кэш, SLA в договоре | @sa | 🟢 Mitigated | 2025-01-15 |
| R-002 | Рост нагрузки в 5x к Q3 при текущей архитектуре single-instance | Scalability | High | High | 🔴 Critical | Планируем горизонтальное масштабирование в Q2 (см. ADR-0012) | @ta | 🔧 In progress | 2025-01-20 |
| R-003 | Утечка данных через AI-ассистент (отправка PII в LLM) | Security | Medium | Critical | 🔴 Critical | AI policy, data classification, корпоративный LLM endpoint | @sa | 🟢 Mitigated | 2025-02-01 |
| R-004 | Vendor lock-in на облачного провайдера | Architecture | Low | High | 🟡 Medium | Абстракция через контейнеры, избегаем проприетарных сервисов для core logic | @sa | 👁️ Monitoring | 2025-02-05 |
| R-005 | Потеря ключевого архитектора (bus factor = 1) | Organizational | Medium | Medium | 🟡 Medium | ADR покрывают все решения, парное ревью, документация в Git | @sa | 🟢 Mitigated | 2025-02-10 |

---

## Категории рисков

| Категория | Описание |
|-----------|---------|
| **Security** | Уязвимости, утечки, несоответствие требованиям безопасности |
| **Scalability** | Невозможность масштабирования под рост нагрузки |
| **Integration** | Риски внешних зависимостей, breaking changes, SLA |
| **Architecture** | Vendor lock-in, технологический долг, устаревание стека |
| **Performance** | Деградация производительности, несоблюдение SLO |
| **Reliability** | Единые точки отказа, отсутствие DR, потеря данных |
| **Compliance** | Несоответствие регуляторным требованиям (ПДн, PCI DSS) |
| **Organizational** | Bus factor, потеря знаний, нехватка компетенций |
| **Cost** | Неконтролируемый рост расходов, budget overrun |

## Статусы

| Статус | Описание |
|--------|---------|
| 🔴 Open | Риск идентифицирован, mitigation не определён |
| 🔧 In progress | Mitigation в процессе реализации |
| 🟢 Mitigated | Mitigation реализован, риск под контролем |
| 👁️ Monitoring | Риск принят, отслеживается |
| ✅ Closed | Риск устранён или стал неактуальным |
| 💥 Realized | Риск реализовался (incident) |

---

## Процесс управления рисками

### Идентификация
- При [architecture review](../docs/practices.md#2-архитектурное-ревью) и [design review](../docs/quality-gates.md)
- При изменении требований или контекста
- При инцидентах (post-mortem → новые риски)
- При [threat modeling](../docs/practices.md#3-работа-с-нефункциональными-требованиями)

### Оценка
- Вероятность: Low / Medium / High (на основе опыта и контекста)
- Импакт: Low / Medium / High / Critical (по матрице выше)
- Определить mitigation strategy: Avoid / Mitigate / Transfer / Accept

### Мониторинг
- Пересмотр реестра: минимум раз в месяц (или каждый спринт для Critical)
- Обновлять статусы и переоценивать вероятности
- Escalate новые Critical риски на [ARB](../docs/governance-charter.md#architecture-review-board-arb)

### Связь с другими артефактами
- Риски → [ADR](../docs/practices.md#1-architecture-decision-records-adr) (решения по mitigation)
- Риски → [NFR](../docs/practices.md#3-работа-с-нефункциональными-требованиями) (требования к mitigation)
- Риски → [Tech Debt Register](tech-debt-register.md) (если risk = результат техдолга)
- Риски → [SLO/SLI Profile](slo-sli-profile.md) (если risk = reliability/performance)
