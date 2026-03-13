# Quality Gates Catalog

## Назначение

Каталог проверок, которые обеспечивают качество архитектурных артефактов. Разделён на **автоматические** (CI/CD) и **ручные** (review). Каждый gate привязан к фазе [процесса](process.md) и [уровню rigor](core-standard.md#уровни-rigor).

---

## Автоматические gates (CI/CD)

Включаются в pipeline проекта. Блокируют merge при нарушении.

### Документация и модели

| Проверка | Инструмент | Что ловит | Уровень |
|----------|-----------|-----------|---------|
| Markdown lint | markdownlint | Битая разметка, inconsistent formatting | L1+ |
| Проверка ссылок | markdown-link-check | Битые ссылки, несуществующие якоря | L1+ |
| ADR format validation | Custom script / adr-tools | ADR без обязательных секций (контекст, решение, последствия) | L1+ |
| C4 diagram validation | Structurizr CLI lint | Несогласованность модели, orphan elements | L2+ |
| Mermaid syntax check | mermaid-cli | Невалидный синтаксис диаграмм | L1+ |

### API контракты

| Проверка | Инструмент | Что ловит | Уровень |
|----------|-----------|-----------|---------|
| OpenAPI lint | Spectral | Нарушения API style guide, missing descriptions | L2+ |
| AsyncAPI lint | Spectral + AsyncAPI parser | Невалидные event-контракты | L2+ |
| Breaking change detection | oasdiff / optic | Несогласованные breaking changes в API | L2+ |
| Schema validation | ajv / openapi-validator | Невалидные schema definitions | L2+ |

### Инфраструктура и политики

| Проверка | Инструмент | Что ловит | Уровень |
|----------|-----------|-----------|---------|
| Policy-as-code | OPA / Conftest | Нарушения политик безопасности, сетевых правил, encryption | L2+ |
| IaC validation | terraform validate + tflint | Невалидная инфраструктура | L2+ |
| Drift detection | terraform plan / Crossplane drift | Расхождение infra с кодом | L3 |
| Container security | Trivy / Grype | Уязвимости в образах | L2+ |

### Security (DevSecOps)

| Проверка | Инструмент | Что ловит | Уровень |
|----------|-----------|-----------|---------|
| SAST (static analysis) | Semgrep / CodeQL / SonarQube | Уязвимости в коде (SQLi, XSS, etc.) | L2+ |
| SCA (dependency check) | Snyk / Trivy fs / Dependabot | Известные CVE в зависимостях | L2+ |
| Secret scanning | GitLeaks / TruffleHog | Секреты в коде и истории Git | L1+ |
| DAST (dynamic analysis) | OWASP ZAP | Runtime-уязвимости (OWASP Top 10) | L3 |
| License compliance | FOSSA / Trivy license | Несовместимые лицензии в зависимостях | L2+ |

### Архитектурные fitness functions

| Проверка | Инструмент | Что ловит | Уровень |
|----------|-----------|-----------|---------|
| Dependency boundaries | ArchUnit / dependency-cruiser | Нарушение границ модулей | L2+ |
| Latency budget | k6 / Gatling в CI | p99 > target | L2+ |
| Cost alerts | Cloud cost API + budget threshold | Превышение бюджета | L2+ |
| Coverage threshold | Jest / pytest coverage | Coverage < target для critical paths | L2+ |

---

## Ручные gates (Review)

Проводятся людьми на ключевых checkpoint'ах проекта.

### Design Review

**Когда:** перед началом реализации значимого изменения.
**Уровень:** L1+ (для решений [Класса 2+](governance-charter.md#класс-2-проектные-решения-кросс-модульные))

**Чеклист:**

- [ ] Решение зафиксировано в ADR с контекстом, вариантами и обоснованием
- [ ] Рассмотрены альтернативы (минимум 2 варианта для значимых решений)
- [ ] Последствия решения описаны явно
- [ ] C4-диаграммы обновлены и отражают изменение
- [ ] NFR impact оценён (performance, security, cost, reliability)
- [ ] Соответствие architecture principles проекта
- [ ] Нет конфликта с существующими ADR

### Architecture Assessment Review

**Когда:** перед сдачей результатов аудита/assessment.
**Уровень:** L0–L1

**Чеклист:**

- [ ] As-is состояние задокументировано (C4 L1, ключевые компоненты)
- [ ] Риски идентифицированы и классифицированы (критичность, вероятность)
- [ ] Техдолг оценён (категория, стоимость устранения)
- [ ] NFR gaps выявлены
- [ ] Рекомендации конкретны и приоритизированы
- [ ] Quick wins выделены отдельно
- [ ] Результат понятен нетехнической аудитории

### Security Review

**Когда:** для решений, затрагивающих безопасность, обработку ПДн, внешние интеграции.
**Уровень:** L2+

**Чеклист:**

- [ ] Threat model актуален
- [ ] Принцип Zero Trust соблюдён (authentication, authorization, encryption)
- [ ] Данные классифицированы (PII, конфиденциально, публично)
- [ ] Секреты не хранятся в коде / конфигурации
- [ ] Audit trail обеспечен для критичных операций
- [ ] AI-политика соблюдена (если используются LLM)
- [ ] Нет выявленных уязвимостей из OWASP Top 10

### Event Governance Review

**Когда:** при проектировании или изменении event-driven интеграций.
**Уровень:** L2+

**Чеклист:**

- [ ] Все события описаны в AsyncAPI спецификации
- [ ] Schema в registry, backward compatibility проверяется
- [ ] Naming convention соблюдена (CloudEvents)
- [ ] DLQ настроена для каждого consumer
- [ ] Consumer lag мониторится
- [ ] Idempotency обеспечена
- [ ] Retry policy определена

Подробнее: [Event Governance Checklist](../templates/event-governance-checklist.md)

### Integration Review

**Когда:** при добавлении / изменении интеграции с внешней системой.
**Уровень:** L2+

**Чеклист:**

- [ ] API-контракт определён и залинтован (OpenAPI/AsyncAPI)
- [ ] Error handling и retry strategy определены
- [ ] Circuit breaker / fallback предусмотрены
- [ ] SLA интеграции зафиксированы
- [ ] Мониторинг интеграции настроен (latency, error rate)
- [ ] Data mapping задокументирован

Подробнее: [Integration Review Checklist](../templates/integration-review-checklist.md)

---

## Gates по фазам процесса

Привязка к [фазам архитектурного процесса](process.md).

### Фаза 1–2: Пресейл и инициация

| Gate | Тип | Описание |
|------|-----|----------|
| Scope определён | Ручной | Зафиксированы границы, уровень rigor, тип engagement |
| Starter kit инициализирован | Автоматический | Репозиторий создан из шаблона, структура на месте |
| AI Policy определена | Ручной | Если используются AI-инструменты |

### Фаза 3: Анализ

| Gate | Тип | Описание |
|------|-----|----------|
| C4 L1 готов | Ручной | Context diagram отражает всех stakeholders и системы |
| NFR собраны | Ручной | Чеклист заполнен, приоритеты согласованы с заказчиком |
| Ключевые ADR зафиксированы | Ручной | ≥3 ADR для L1+, включая architecture principles |

### Фаза 4: Проектирование

| Gate | Тип | Описание |
|------|-----|----------|
| C4 L2 готов | Ручной | Container diagram покрывает все ключевые компоненты |
| Solution Architecture Doc | Ручной | arc42-based документ завершён (L2+) |
| API specs определены | Автоматический | OpenAPI/AsyncAPI проходят lint |
| Design review пройден | Ручной | SA/Lead Architect одобрил ключевые решения |

### Фаза 5–6: Реализация и тестирование

| Gate | Тип | Описание |
|------|-----|----------|
| CI gates зелёные | Автоматический | Все автоматические проверки проходят |
| Fitness functions активны | Автоматический | ≥5 fitness functions в CI для L2+ |
| Architecture conformance | Ручной | Код соответствует архитектуре (code review с арх. фокусом) |

### Фаза 7–8: Развёртывание и сопровождение

| Gate | Тип | Описание |
|------|-----|----------|
| SLO/SLI определены | Ручной | Targets и мониторинг настроены |
| Runbooks готовы | Ручной | Процедуры эксплуатации задокументированы |
| Tech debt register актуален | Ручной | Долг зафиксирован, владельцы назначены |
| Drift detection активен | Автоматический | L3: расхождение infra отслеживается |

---

## Definition of Done для артефактов

Каждый артефакт считается «готовым», когда выполнены его критерии качества.

| Артефакт | DoD |
|----------|-----|
| **ADR** | Есть контекст, ≥2 варианта, решение с обоснованием, последствия, ссылка на NFR/fitness |
| **C4 Context** | Все внешние системы и акторы, стрелки подписаны, нет orphan elements |
| **C4 Container** | Все контейнеры с технологиями, коммуникации подписаны (протокол), хранилища выделены |
| **NFR Checklist** | Все категории проверены, targets количественные (не «быстро», а «p99 < 200ms»), приоритеты согласованы |
| **API Spec** | Проходит Spectral lint, все endpoints описаны, есть examples, error responses определены |
| **Threat Model** | Attack surface определён, threats классифицированы, mitigations привязаны к конкретным мерам |
| **Solution Architecture Doc** | Все секции arc42 заполнены (для актуального уровня), ссылки на ADR и C4, review пройден |
| **Assessment Report** | As-is, риски, техдолг, NFR gaps, quick wins, roadmap — всё с приоритетами |
| **Tech Debt Register** | Каждый элемент: категория, стоимость, риск, владелец, план |
| **Risk Register** | Каждый риск: вероятность, импакт, mitigation, owner, статус |
| **SLO/SLI Profile** | SLO targets определены, SLI с источниками данных, алертинг настроен |

---

## Внедрение gates на проекте

### Шаг 1: Определить уровень rigor

По [матрице Core Standard](core-standard.md#уровни-rigor).

### Шаг 2: Настроить автоматические gates

Из starter kit или по чеклисту выше — включить соответствующие уровню проверки в CI.

### Шаг 3: Назначить ответственных за ручные gates

SA отвечает за проведение design review и assessment review. TA — за security review и CI gate configuration.

### Шаг 4: Зафиксировать в Architecture Principles

Первый ADR проекта фиксирует уровень rigor и набор gates.
