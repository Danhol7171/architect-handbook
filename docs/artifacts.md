# Каталог архитектурных артефактов

## Источники

Каталог сформирован на основе лучших практик архитектурного консалтинга, включает современные артефакты (ADR, fitness functions, API specs, FinOps, threat model).

## Принцип минимального набора

Не все артефакты нужны на каждом проекте. Набор масштабируется:

| Уровень | Когда | Артефакты |
|---------|-------|-----------|
| Минимальный | Аудит, экспресс-оценка | C4 Context (L1), ADR, Assessment Report |
| Базовый | Средний проект | + Solution Architecture (C4 L2), NFR Checklist, Integration Design, API Specs |
| Полный | Крупная трансформация | + все Blueprints, Component Designs, Data Architecture, Fitness Functions, Threat Model, FinOps Assessment |

---

## Стратегические артефакты

### Solution Concept
**Назначение:** высокоуровневое видение решения для пресейла и инициации.
**Содержание:** бизнес-контекст, ключевые компоненты, границы, предварительная оценка, cost model.
**Формат:** C4 Context diagram (Mermaid) + текстовое описание.
[Шаблон](../templates/solution-concept.md)

### Architecture Decision Records (ADR)
**Назначение:** фиксация каждого значимого решения с контекстом и обоснованием.
**Содержание:** контекст, варианты, решение, последствия.
**Формат:** Markdown в Git (`/decisions/`).
**Инструменты:** CLI-утилита для ведения ADR (adr-tools и аналоги), генератор веб-UI для decision log.
**Обязательность:** на всех проектах, начиная с минимального уровня.
[Шаблон](../templates/adr-template.md) | [Пример](../templates/adr-example.md)

### Architecture Principles
**Назначение:** набор принципов, определяющих архитектурные решения проекта.
**Содержание:** принципы + обоснование, оформляются как ADR.

**Пример:** "ADR-0001: Используем managed services вместо self-hosted, потому что..."
[Пример](../templates/architecture-principles.md)

---

## Solution Architecture (C4-based)

Центральный документ. Структура на базе arc42 (адаптированная):

### C4 Context Diagram (Level 1)
**Назначение:** система в окружении пользователей и внешних систем.
**Обязательность:** всегда.
**Формат:** Mermaid / Structurizr DSL.
[Пример](../templates/c4-context.md)

### C4 Container Diagram (Level 2)
**Назначение:** основные технические блоки (приложения, БД, очереди, API).
**Обязательность:** всегда на базовом+ уровне.
**Формат:** Mermaid / Structurizr DSL.
[Пример](../templates/c4-container.md)

### C4 Component Diagram (Level 3)
**Назначение:** внутренняя структура контейнера.
**Обязательность:** для сложных модулей, по необходимости.

### Solution Architecture Document
**Назначение:** текстовое описание архитектуры (arc42-based).
**Содержание:**
1. Введение и цели
2. Ограничения
3. Контекст и scope (C4 L1)
4. Стратегия решения
5. Building blocks (C4 L2-L3)
6. Runtime view (ключевые сценарии)
7. Deployment view
8. Crosscutting concerns (security, logging, error handling)
9. Architecture decisions (ссылки на ADR)
10. Quality requirements (NFR)
11. Risks and technical debt

[Шаблон](../templates/solution-architecture-doc.md)

---

## Архитектурные блюпринты

Для крупных проектов. На базовом уровне достаточно Solution Architecture.

### Development Architecture Blueprint
**Назначение:** среда и инструменты разработки.
**Содержание:** CI/CD pipeline, branching strategy, code quality tools, стандарты, репозитории.


### Runtime Architecture Blueprint
**Назначение:** архитектура времени выполнения.
**Содержание:** сервисы, контейнеры, оркестрация, масштабирование, service mesh.


### Operations Architecture Blueprint
**Назначение:** эксплуатация и мониторинг.
**Содержание:** OpenTelemetry setup, alerting, logging, runbooks, SLO definitions.


### Infrastructure Architecture Blueprint
**Назначение:** инфраструктурный слой.
**Содержание:** сети, хранилища, облачные сервисы, IaC (Terraform), policy-as-code.


---

## Артефакты качества

### NFR Checklist
**Назначение:** систематический сбор и приоритизация нефункциональных требований.
**Категории:**

| Категория | Примеры метрик |
|-----------|---------------|
| Scalability | Макс. одновременных пользователей, рост объёма данных |
| Performance | Латентность p50/p95/p99, пропускная способность |
| Availability | SLA (уровень доступности) %, RTO (время восстановления), RPO (допустимая потеря данных) |
| Recoverability | Расписание резервного копирования, план аварийного восстановления (DR) |
| Security | Способ аутентификации, шифрование, соответствие регуляторным требованиям |
| Maintainability | Лимиты сложности кода, покрытие тестами |
| Operability | Требования к наблюдаемости, процесс дежурства |
| Cost | Ежемесячный бюджет, стоимость на транзакцию |
| Sustainability | SCI (углеродная интенсивность ПО) - при необходимости |

**Обязательность:** на базовом+ уровне.
[Пример](../templates/nfr-checklist.md)

### Fitness Functions
**Назначение:** автоматизированные проверки архитектурных характеристик.
**Содержание:** правила в формате "если X, то Y", встроенные в CI/CD.
**Примеры:** latency budget, dependency rules, API linting, encryption policy, cost alerts.
**Обязательность:** полный уровень.
[Пример](../templates/fitness-functions.md)

### Threat Model
**Назначение:** анализ угроз и контрмер.
**Содержание:** attack surface, threat actors, mitigations.
**Когда:** проекты с требованиями безопасности, обработка PII, финансовые системы.
[Пример](../templates/threat-model.md)

### FinOps Assessment
**Назначение:** архитектурная оценка стоимости владения.
**Содержание:** cost model, optimization recommendations, budget alerts.
**Когда:** облачные проекты, трансформации.
[Пример](../templates/finops-assessment.md)

### Cost Model
**Назначение:** модель стоимости архитектурного решения с unit economics и прогнозом роста.
**Содержание:** компоненты стоимости, сравнение вариантов, оптимизация, мониторинг.
**Когда:** включается в ADR для значимых решений, в assessment report, при FinOps-оценке.
**Обязательность:** [уровень L2+](core-standard.md), рекомендуется на L1 при облачных проектах.
[Шаблон](../templates/cost-model.md)

---

## Артефакты интеграции

### Integration Design
**Назначение:** описание межсистемных взаимодействий.
**Содержание:** потоки данных, протоколы, паттерны (sync/async/batch), error handling.
**Формат:** Mermaid sequence diagrams + текстовое описание.
[Пример](../templates/integration-design.md)

### API Specifications
**Назначение:** контракты API.
**Формат:** OpenAPI 3.1 (REST), AsyncAPI (events).
**Инструменты:** линтер API-спецификаций для валидации, платформа управления API lifecycle для коллаборативного дизайна.
**Обязательность:** для всех публичных API на базовом+ уровне.
[Пример](../templates/api-specification.md)

### System Integration Flow
**Назначение:** карта потоков данных между системами.


### Interface Agreement
**Назначение:** согласованный контракт между командами/системами.


---

## Артефакты данных

### Data Model / Data Definition
**Назначение:** модель данных решения.
[Пример](../templates/data-model.md)

### Data Mapping
**Назначение:** маппинг данных между системами.


### Data Conversion Design
**Назначение:** проектирование миграции данных.


### Data Contract
**Назначение:** формализованное соглашение между поставщиком и потребителем данных.
**Содержание:** schema, ownership, quality SLA, доступ, lineage, versioning.
**Когда:** data mesh, интеграции через данные, ML pipelines.
**Обязательность:** [уровень L3](core-standard.md), рекомендуется на L2 при наличии data products.
[Шаблон](../templates/data-contract.md)

### Data Product Spec (для data mesh)
**Назначение:** спецификация data product.
**Когда:** крупные проекты с data mesh подходом.
**Содержание:** schema, SLA, owner, access policies, quality metrics.

### EDA Decision Record
**Назначение:** ADR для решений по event-driven архитектуре.
**Содержание:** паттерн EDA, event design, топология, error handling, мониторинг.
**Когда:** проектирование event-driven интеграций.
[Шаблон](../templates/eda-decision-record.md)

---

## Артефакты принятия решений

### DME Workbook - Alternative Components
**Назначение:** формализованная оценка альтернативных технологий.
**Когда:** выбор СУБД, message broker, cloud provider и т.п.
**Современная альтернатива:** ADR с таблицей сравнения.

### Fit/Gap Analysis
**Назначение:** анализ соответствия решения/платформы требованиям.
**Когда:** выбор packaged solution, оценка существующей системы.
[Шаблон](../templates/fitgap-analysis.md)

---

## Стандарты и governance

### Development Standards
**Назначение:** стандарты разработки проекта.
**Содержание:** code style, branching, PR process, review checklist.
[Пример](../templates/development-standards.md)

### Architecture Standards
**Назначение:** критерии принятия архитектурных решений.

### Waiver Request
**Назначение:** оформление исключения из архитектурного стандарта.
**Содержание:** какой стандарт, причина отклонения, риски, план возврата, срок действия.
**Когда:** проект не может следовать стандарту по объективным причинам.
**Процесс:** см. [Governance Charter](governance-charter.md#waivers-исключения-из-стандартов).
[Шаблон](../templates/waiver-request.md) | [Реестр waivers](../templates/waiver-register.md)

### Policy Rules (policy-as-code)
**Назначение:** архитектурные политики в виде кода.
**Формат:** OPA/Rego, Conftest, Terraform Sentinel.
**Содержание:** сетевые правила, encryption, tagging, region restrictions.
[Пример](../templates/policy-rules.md)

### AI Policy
**Назначение:** правила использования AI на проекте.
**Содержание:** разрешённые сценарии, красные зоны, верификация, audit trail.
**Обязательность:** при использовании AI-инструментов (L1+), обязательно на L3.
[Пример](../templates/ai-policy.md)

---

## Артефакты оценки и управления

### Platform Assessment
**Назначение:** оценка текущего состояния платформы разработки и рекомендации по внедрению IDP.
**Содержание:** as-is инструменты, DevEx survey, DORA baseline, maturity assessment, боли, рекомендации, adoption strategy.
**Когда:** запуск IDP-инициативы, assessment Developer Experience.
**Обязательность:** [уровень L2+](core-standard.md) при наличии > 5 продуктовых команд.
[Шаблон](../templates/platform-assessment.md)

### Architecture Assessment Report
**Назначение:** стандартный отчёт по архитектурному аудиту / assessment.
**Содержание:** as-is архитектура, NFR gap analysis, риски, технический долг, рекомендации (quick wins + roadmap).
**Когда:** аудит, assessment, due diligence — основной deliverable на [уровнях L0–L1](core-standard.md).
**Обязательность:** для всех assessment engagement.
[Шаблон](../templates/assessment-report.md)

### Evidence Pack
**Назначение:** стандартизированный чеклист источников данных для аудита.
**Содержание:** репозитории, runtime-данные, инфраструктура, данные, стоимость, документация, интервью.
**Когда:** сопровождает [Assessment Report](../templates/assessment-report.md) — каждый finding ссылается на evidence item.
**Обязательность:** для всех assessment engagement (L0–L1).
[Шаблон](../templates/evidence-pack.md)

### Tech Debt Register
**Назначение:** реестр архитектурного технического долга с классификацией, оценкой и владельцами.
**Содержание:** элементы долга, категория, severity, стоимость устранения, риск, owner, план.
**Когда:** delivery-проекты, сопровождение — ведётся непрерывно.
**Обязательность:** [уровень L2+](core-standard.md).
[Шаблон](../templates/tech-debt-register.md)

### Architecture Risk Register
**Назначение:** реестр архитектурных рисков с оценкой и планом mitigation.
**Содержание:** риски, вероятность, импакт, mitigation strategy, owner, статус.
**Когда:** все проекты начиная с L1.
**Обязательность:** [уровень L1+](core-standard.md).
[Шаблон](../templates/risk-register.md)

### SLO/SLI Profile
**Назначение:** профиль надёжности и производительности системы.
**Содержание:** SLO targets, SLI определения, error budget policy, алертинг, capacity planning.
**Когда:** delivery и сопровождение — определяется при [развёртывании](process.md#7-развёртывание).
**Обязательность:** [уровень L2+](core-standard.md).
[Шаблон](../templates/slo-sli-profile.md)

---

## Starter Kit

Шаблон репозитория для быстрого старта:

```
project-repo/
├── architecture/
│   ├── README.md              # Solution Architecture (arc42)
│   ├── context.mmd            # C4 Context (Mermaid)
│   ├── containers.mmd         # C4 Container (Mermaid)
│   └── components/            # C4 Component (по необходимости)
├── decisions/
│   ├── README.md              # Decision log
│   ├── 0001-template.md       # ADR template
│   └── 0002-example.md        # Пример заполненного ADR
├── quality/
│   ├── nfr-checklist.md       # NFR checklist
│   ├── fitness-functions.md   # Определения fitness functions
│   ├── security.md            # Security requirements / threat model
│   └── ai-policy.md           # AI policy проекта
├── api/
│   └── openapi.yaml           # API спецификация (если есть API)
└── .spectral.yaml             # API linting rules (если есть API)
```

Архитектор копирует starter kit в новый проект и начинает работу за 15 минут.
