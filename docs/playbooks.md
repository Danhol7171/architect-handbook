# Engagement Playbooks

## Назначение

Playbook — это инструкция «как провести архитектурную работу от начала до конца» для конкретного типа engagement. Каждый playbook содержит: входы, шаги, артефакты, quality gates и критерии завершения.

Playbook отвечает на вопрос: **«Я — архитектор на проекте типа X. Что мне делать?»**

---

## Типы engagement

| Тип | Описание | Уровень rigor | Timebox |
|-----|---------|:---:|---------|
| [Audit / Assessment](#audit--assessment) | Обследование существующей архитектуры | L0–L1 | 1–4 недели |
| [Discovery / Solutioning](#discovery--solutioning) | Проектирование целевого решения | L1 | 2–8 недель |
| [Delivery Support](#delivery-support) | Архитектурное сопровождение разработки | L2 | Ongoing |
| [Platform Engineering](#platform-engineering) | Проектирование и внедрение IDP/платформы | L2–L3 | 3–6 месяцев |
| [Application Modernization](#application-modernization) | Модернизация legacy-приложений (6R) | L1–L2 | 2–6 месяцев |
| [Cloud Migration](#cloud-migration) | Миграция в облако | L2–L3 | 3–9 месяцев |
| [Enterprise Transformation](#enterprise-transformation) | Архитектурное управление программой трансформации | L3 | 6+ месяцев |

---

## Матрица deliverables

| Артефакт | Audit | Discovery | Delivery | Platform | Modernization | Cloud Migration | Transformation |
|----------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| C4 Context (L1) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| C4 Container (L2) | — | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| ADR (3+) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Solution Concept | — | ✅ | — | ✅ | — | — | — |
| Solution Architecture Doc | — | — | ✅ | ✅ | — | — | — |
| NFR Checklist | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| Assessment Report | ✅ | — | — | — | ✅ | — | — |
| Evidence Pack | ✅ | — | — | — | ✅ | — | — |
| Risk Register | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Tech Debt Register | ✅ | — | ✅ | ✅ | ✅ | — | ✅ |
| Integration Design | — | ✅* | ✅* | ✅ | ✅* | ✅* | — |
| API Specs | — | — | ✅* | ✅ | — | — | — |
| Threat Model | — | — | ✅* | ✅ | — | ✅ | — |
| SLO/SLI Profile | — | — | ✅ | ✅ | — | — | — |
| Fitness Functions | — | — | ✅ | ✅ | ✅ | ✅ | — |
| AI Policy | ✅** | ✅** | ✅** | ✅ | ✅** | ✅** | ✅** |
| Cost Model | — | — | — | — | ✅ | ✅ | — |
| FinOps Assessment | — | — | — | — | — | ✅ | — |
| Migration Plan | — | — | — | — | ✅ | ✅ | — |
| Capability Map | — | — | — | — | — | — | ✅ |
| Waiver Register | — | — | — | — | — | — | ✅ |
| Presentation (для заказчика) | ✅ | ✅ | — | ✅ | ✅ | ✅ | ✅ |

\* При наличии соответствующего контекста.
\*\* При использовании AI-инструментов.

---

## Audit / Assessment

### Цель
Оценить текущее состояние архитектуры, выявить риски и техдолг, сформировать рекомендации.

### Входы
- Запрос заказчика (что болит, что хотят понять)
- Доступ к документации, коду, инфраструктуре
- Контакты стейкхолдеров для интервью

### Шаги

**Неделя 1: Discovery**
1. Kick-off: согласовать scope, ожидания, формат результата
2. Определить [уровень rigor](core-standard.md) (обычно L0 или L1)
3. Инициализировать [Starter Kit](artifacts.md#starter-kit) (минимально: ADR + NFR checklist)
4. Собрать и изучить имеющуюся документацию
5. Провести интервью с ключевыми стейкхолдерами (3–5 интервью)
6. Reverse engineering: анализ кода, инфраструктуры, мониторинга

**Неделя 2–3: Анализ**
7. Построить C4 Context diagram (as-is)
8. Заполнить NFR gap analysis (текущее vs best practice)
9. Идентифицировать архитектурные риски → [Risk Register](../templates/risk-register.md)
10. Оценить технический долг → [Tech Debt Register](../templates/tech-debt-register.md)
11. Зафиксировать ключевые findings в ADR

**Неделя 3–4: Рекомендации**
12. Сформировать рекомендации: quick wins + среднесрочные + стратегические
13. Приоритизировать по impact / effort
14. Оформить [Assessment Report](../templates/assessment-report.md)
15. Подготовить презентацию для руководства (нетехническая аудитория)
16. Провести финальную встречу с заказчиком

### Артефакты
- [Assessment Report](../templates/assessment-report.md) — основной deliverable
- [Evidence Pack](../templates/evidence-pack.md) — чеклист источников данных (каждый finding ссылается на evidence item)
- [C4 Context](../templates/c4-context.md) (as-is)
- [Risk Register](../templates/risk-register.md)
- [Tech Debt Register](../templates/tech-debt-register.md)
- ADR (ключевые findings)
- Презентация

### Quality Gates

- [ ] Scope согласован с заказчиком
- [ ] ≥3 интервью проведены
- [ ] C4 Context покрывает весь scope
- [ ] NFR gap analysis по всем категориям
- [ ] Рекомендации приоритизированы и конкретны
- [ ] Assessment Report прошёл внутренний review

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Недостаточный доступ к системам | Оговорить в kick-off, зафиксировать ограничения в отчёте |
| Стейкхолдеры не дают интервью | Эскалация через PM, перенос сроков |
| Scope creep | Жёсткий timebox, изменения scope — через change request |

---

## Discovery / Solutioning

### Цель
Спроектировать целевую архитектуру решения: от бизнес-контекста до технических решений.

### Входы
- Бизнес-требования / RFP / vision document
- Constraints (бюджет, сроки, технологические ограничения)
- Результаты assessment (если был)

### Шаги

**Фаза 1: Контекст (1–2 недели)**
1. Kick-off: scope, constraints, success criteria
2. Определить [уровень rigor](core-standard.md) (обычно L1)
3. Stakeholder mapping: кто, что хочет, приоритеты
4. Инициализировать [Starter Kit](artifacts.md#starter-kit)
5. Собрать и приоритизировать требования
6. Заполнить [NFR Checklist](../templates/nfr-checklist.md) — согласовать targets с заказчиком

**Фаза 2: Архитектура (2–4 недели)**
7. [Solution Concept](../templates/solution-concept.md): C4 Context + высокоуровневое описание
8. Определить [Architecture Principles](../templates/architecture-principles.md) (первые ADR)
9. Оценка и выбор технологий → ADR на ключевые решения
10. C4 Container diagram: основные компоненты
11. [Integration Design](../templates/integration-design.md) (если есть интеграции)
12. Предварительный cost model

**Фаза 3: Финализация (1–2 недели)**
13. [Risk Register](../templates/risk-register.md)
14. Roadmap реализации
15. Design review (внутренний — SA/Lead Architect)
16. Презентация заказчику
17. Handoff в delivery (если продолжается)

### Артефакты
- [Solution Concept](../templates/solution-concept.md) — основной deliverable
- [C4 Context](../templates/c4-context.md) + [C4 Container](../templates/c4-container.md)
- [NFR Checklist](../templates/nfr-checklist.md)
- ADR (≥5 ключевых решений)
- [Architecture Principles](../templates/architecture-principles.md)
- [Risk Register](../templates/risk-register.md)
- Cost estimate
- Презентация

### Quality Gates

- [ ] Requirements собраны и приоритизированы
- [ ] NFR targets количественные и согласованы
- [ ] C4 L1+L2 покрывают solution scope
- [ ] ≥5 ADR на ключевые решения
- [ ] Рассмотрены альтернативы (≥2 варианта для каждого ADR)
- [ ] Cost model определён
- [ ] Design review пройден

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Требования меняются в процессе | Итеративный подход, фиксация baseline, change log |
| Нереалистичные expectations | Управление ожиданиями с kick-off, constraints в ADR |
| Выбор технологий без PoC | Для high-risk решений — обязательный PoC / spike |

---

## Delivery Support

### Цель
Обеспечить архитектурное качество в процессе разработки: governance, review, conformance, эволюция архитектуры.

### Входы
- Solution architecture (результат Discovery/Solutioning)
- Команда разработки
- Backlog / roadmap продукта

### Шаги

**Старт (первый спринт)**
1. Определить [уровень rigor](core-standard.md) (обычно L2)
2. Инициализировать [Starter Kit](artifacts.md#starter-kit) в репозитории проекта
3. Настроить CI gates: lint, policy-as-code, API lint (см. [Quality Gates](quality-gates.md))
4. Определить [fitness functions](../templates/fitness-functions.md) (5–15 штук)
5. Установить ритм architecture review: еженедельные [office hours](governance-charter.md#office-hours-проектный-review)
6. Определить [SLO/SLI](../templates/slo-sli-profile.md) targets

**Ongoing (каждый спринт)**
7. Участие в planning: оценка архитектурного impact задач
8. Design review для задач с архитектурным impact
9. Code review с архитектурным фокусом
10. Обновление ADR при принятии новых решений
11. Мониторинг CI gates и fitness functions
12. Обновление [Tech Debt Register](../templates/tech-debt-register.md)

**Регулярно (ежемесячно/ежеквартально)**
13. Architecture conformance review: соответствует ли код архитектуре
14. NFR review: соблюдаются ли targets
15. Tech debt review: приоритизация и планирование устранения
16. SLO/SLI review: error budget, capacity planning
17. Update C4 diagrams при изменениях архитектуры

### Артефакты (поддерживаемые непрерывно)
- [Solution Architecture Doc](../templates/solution-architecture-doc.md)
- C4 diagrams (актуализированные)
- ADR (прирастают по мере решений)
- [Fitness Functions](../templates/fitness-functions.md)
- [Tech Debt Register](../templates/tech-debt-register.md)
- [Risk Register](../templates/risk-register.md)
- [SLO/SLI Profile](../templates/slo-sli-profile.md)
- [API Specs](../templates/api-specification.md)

### Quality Gates

- [ ] CI gates настроены и зелёные
- [ ] ≥5 fitness functions в CI
- [ ] ADR покрывают все значимые решения
- [ ] Tech debt register актуален (пересмотр каждый квартал)
- [ ] SLO/SLI определены и мониторятся
- [ ] Architecture review проводится регулярно

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Архитектор не вовлечён в повседневную работу | Обязательное участие в planning + review |
| Architecture drift (код расходится с архитектурой) | Fitness functions + регулярный conformance review |
| Tech debt накапливается | 15–20% capacity на техдолг, register с владельцами |

---

## Platform Engineering

### Цель
Спроектировать и внедрить Internal Developer Platform (IDP): стандартизация, golden paths, self-service, снижение cognitive load команд.

### Входы
- Организационный контекст: количество команд, текущий инструментарий
- Боли команд (developer experience survey)
- Бизнес-драйверы (скорость delivery, quality, compliance)

### Шаги

**Фаза 1: Assessment (2–4 недели)**
1. Определить [уровень rigor](core-standard.md) (L2 или L3)
2. Platform maturity assessment (текущее состояние)
3. Developer experience survey: что болит, что замедляет
4. Stakeholder mapping: platform users, sponsors, operations
5. Определить MVP scope платформы

**Фаза 2: Vision & Design (4–6 недель)**
6. Target platform architecture: C4 L1 + L2
7. Golden paths definition: что стандартизируем в первую очередь
8. Service catalog design (Backstage catalog-info.yaml)
9. Ownership model: кто владеет чем
10. ADR на ключевые решения платформы
11. [NFR Checklist](../templates/nfr-checklist.md) для платформы
12. Cost model + ROI оценка

**Фаза 3: MVP Build (6–12 недель)**
13. Реализация MVP: базовый каталог, 1–2 golden paths, CI/CD templates
14. Onboarding первых 2–3 команд-early adopters
15. Feedback loop: итерация по результатам onboarding
16. Документация: getting started guide, contribution guide
17. [Fitness functions](../templates/fitness-functions.md) для платформы

**Фаза 4: Scale (ongoing)**
18. Расширение golden paths и шаблонов
19. Onboarding остальных команд
20. Метрики DevEx: DORA, developer satisfaction, time-to-first-deploy
21. Platform roadmap и backlog
22. Community of Practice для platform users

### Артефакты
- Platform Vision Document (Solution Concept)
- C4 diagrams (platform architecture)
- Golden Path templates
- Backstage catalog-info.yaml schema
- ADR (platform decisions)
- Platform Runbook / Getting Started Guide
- DevEx metrics dashboard

### Quality Gates

- [ ] Maturity assessment проведён
- [ ] Developer survey (≥70% response rate)
- [ ] MVP scope согласован с sponsors
- [ ] ≥2 early adopter команды onboarded
- [ ] DORA metrics baseline измерен
- [ ] Golden paths задокументированы

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Platform becomes a bottleneck | Self-service first, platform as product (не gatekeeping) |
| Low adoption | Early adopters, quick wins, developer satisfaction metrics |
| Over-engineering MVP | Strict MVP scope, итеративный подход |
| Нет ownership | Выделенная platform team с product owner |

---

## Application Modernization

### Цель
Модернизация legacy-приложений: от оценки текущего состояния до выбора стратегии (6R) и реализации по волнам.

### Входы
- As-is assessment (или результат [Audit](#audit--assessment))
- Бизнес-драйверы модернизации (стоимость, скорость, масштабируемость, compliance)
- Constraints (бюджет, сроки, компетенции команды, зависимости)

### Шаги

**Фаза 1: Assessment (2-4 недели)**
1. Определить [уровень rigor](core-standard.md) (обычно L1-L2)
2. Инвентаризация приложений: компоненты, зависимости, технологический стек
3. Построить C4 Context + Container (as-is)
4. Оценить техдолг и архитектурные риски
5. Собрать NFR targets для целевого состояния
6. [Evidence Pack](../templates/evidence-pack.md): код, runtime-метрики, стоимость

**Фаза 2: Стратегия (2-3 недели)**
7. Для каждого компонента определить стратегию по 6R framework:
   - **Retain** -- оставить как есть (не трогаем)
   - **Retire** -- вывести из эксплуатации
   - **Rehost** -- перенести без изменений (lift & shift)
   - **Replatform** -- перенести с минимальными изменениями (lift & reshape)
   - **Refactor** -- существенная переработка для новой платформы
   - **Replace** -- заменить на SaaS/managed/новое решение
8. Зафиксировать стратегию в ADR (отдельный ADR на каждое значимое решение)
9. Сформировать [Cost Model](../templates/cost-model.md): TCO as-is vs to-be

**Фаза 3: Wave Planning (1-2 недели)**
10. Сгруппировать компоненты в волны миграции по зависимостям и рискам
11. Определить quick wins (первая волна -- низкий риск, высокий impact)
12. Для каждой волны: scope, зависимости, prerequisites, rollback plan
13. C4 Container (to-be) для целевого состояния

**Фаза 4: Execution (ongoing)**
14. Реализация по волнам с validation после каждой волны
15. Fitness functions для проверки NFR после миграции каждого компонента
16. Обновление C4 diagrams после каждой волны
17. Retrospective после каждой волны → корректировка плана

### Артефакты
- [Assessment Report](../templates/assessment-report.md) (as-is)
- ADR (стратегия 6R для каждого компонента)
- [C4 Context](../templates/c4-context.md) + [C4 Container](../templates/c4-container.md) (as-is → to-be)
- [Cost Model](../templates/cost-model.md) (TCO comparison)
- Migration Plan (волны, зависимости, timeline)
- [Risk Register](../templates/risk-register.md)
- [NFR Checklist](../templates/nfr-checklist.md) (targets для to-be)

### Quality Gates

- [ ] As-is assessment завершён, все компоненты инвентаризированы
- [ ] Стратегия 6R зафиксирована в ADR для каждого компонента
- [ ] Cost model определён (as-is vs to-be)
- [ ] Волны приоритизированы по risk/impact
- [ ] Rollback plan определён для каждой волны
- [ ] NFR targets для to-be определены и измеримы

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Скрытые зависимости между компонентами | Thorough dependency mapping, интервью с разработчиками |
| Big bang вместо инкрементальной миграции | Строгое волновое планирование, quick wins first |
| Недооценка сложности refactor | PoC/spike для высокорискованных компонентов перед commitment |
| Data migration failures | Dry run миграции данных, rollback procedures, validation scripts |
| Бизнес не готов к downtime | Zero-downtime migration patterns (strangler fig, feature flags) |

---

## Cloud Migration

### Цель
Миграция инфраструктуры и приложений в облако: от landing zone до cutover по волнам.

### Входы
- As-is инфраструктура (результат [Audit](#audit--assessment) или отдельное обследование)
- Целевой облачный провайдер и его ограничения
- Бюджет и timeline
- Compliance-требования (152-ФЗ, PCI DSS, отраслевые)

### Шаги

**Фаза 1: Landing Zone (3-4 недели)**
1. Определить [уровень rigor](core-standard.md) (обычно L2-L3)
2. Landing zone design: account/subscription structure, networking, identity
3. Security baseline: encryption, IAM, network segmentation, logging
4. Определить tagging strategy (FinOps)
5. IaC foundation: Terraform modules для базовых ресурсов
6. ADR на ключевые решения (провайдер, multi-region, DR strategy)
7. [Policy-as-code](../templates/policy-rules.md): OPA/Conftest правила для cloud

**Фаза 2: Migration Assessment (2-3 недели)**
8. Инвентаризация workloads для миграции
9. Оценка по 6R (retain/retire/rehost/replatform/refactor/replace)
10. Зависимости между workloads
11. [Cost Model](../templates/cost-model.md): прогноз облачных затрат
12. Compliance mapping: какие требования влияют на размещение

**Фаза 3: Wave Planning (1-2 недели)**
13. Группировка workloads в волны по зависимостям
14. Для каждой волны: prerequisites, migration method, validation criteria, rollback
15. PoC: миграция 1-2 workloads из первой волны для validation подхода
16. Networking: connectivity между on-prem и cloud (VPN/Direct Connect)

**Фаза 4: Execution & Cutover (ongoing)**
17. Миграция по волнам
18. Validation после каждой волны: функциональность, NFR, cost
19. Оптимизация: right-sizing, reserved instances, spot/preemptible
20. Cutover planning для каждой волны: DNS switch, data sync, rollback window
21. Decommission on-prem ресурсов после подтверждения стабильности

### Артефакты
- Landing Zone Design (ADR + C4)
- [Security Baseline](../templates/threat-model.md) (cloud security)
- [Cost Model](../templates/cost-model.md) (прогноз + фактические затраты)
- [Policy Rules](../templates/policy-rules.md) (cloud policies)
- Migration Plan (волны, зависимости, timeline)
- [Risk Register](../templates/risk-register.md)
- [FinOps Assessment](../templates/finops-assessment.md)

### Quality Gates

- [ ] Landing zone развёрнута и прошла security review
- [ ] Policy-as-code настроен (encryption, tagging, network)
- [ ] PoC: 1-2 workloads успешно мигрированы
- [ ] Cost model валидирован по результатам PoC
- [ ] Rollback procedures протестированы
- [ ] Compliance requirements подтверждены для cloud-размещения

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Cost overrun | FinOps с первого дня, budget alerts, right-sizing |
| Security gaps в cloud | Security baseline до миграции, policy-as-code |
| Vendor lock-in | ADR на решения vendor-specific vs portable, абстракции через IaC |
| Latency при гибридной работе | Network PoC, latency testing до миграции production |
| Data residency / compliance | Mapping требований к регионам cloud, legal review |
| Downtime при cutover | Blue-green / canary migration, DNS с низким TTL |

---

## Enterprise Transformation

### Цель
Архитектурное управление программой трансформации: от capability mapping до стандартизации и portfolio governance.

### Входы
- Стратегия трансформации (бизнес-цели, vision)
- Портфель систем (as-is landscape)
- Организационная структура (команды, зоны ответственности)
- Бюджет и timeline программы

### Шаги

**Фаза 1: Assessment & Vision (4-6 недель)**
1. Определить [уровень rigor](core-standard.md) (L3)
2. Business Capability Mapping: карта бизнес-возможностей → привязка к системам
3. Application portfolio assessment: lifecycle, tech debt, стоимость владения
4. As-is architecture landscape: C4 Context для ключевых систем
5. Gap analysis: текущие capabilities vs целевые
6. Stakeholder mapping: sponsors, architects, teams

**Фаза 2: Target Architecture (4-6 недель)**
7. Target Architecture Principles (ADR)
8. Target state: C4 Context (to-be), ключевые интеграции
9. Technology standards: ADR на стандартные стеки, платформы, паттерны
10. Architecture decision framework: какие решения на каком уровне
11. [Governance Charter](governance-charter.md): ARB, decision rights, waivers
12. Dependency mapping между workstreams

**Фаза 3: Standards Rollout (ongoing)**
13. Architecture standards publication + communication
14. [Starter Kit](artifacts.md#starter-kit) для новых проектов в программе
15. Onboarding архитекторов workstreams
16. [Waiver process](governance-charter.md#waivers-исключения-из-стандартов) для обоснованных отклонений
17. Template library: ADR, C4, NFR checklist, assessment report

**Фаза 4: Portfolio Governance (ongoing)**
18. Regular portfolio review cadence (ежемесячно / ежеквартально)
19. Architecture conformance tracking по workstreams
20. Cross-workstream dependency management
21. Architecture debt portfolio view
22. Metrics: adoption, conformance, decision velocity, rework rate

### Артефакты
- Capability Map (бизнес-возможности → системы)
- Target Architecture (C4 Context to-be, Architecture Principles)
- Architecture Standards (набор ADR, governance charter)
- Portfolio Roadmap (workstreams, milestones, dependencies)
- [Waiver Register](../templates/waiver-register.md)
- [Risk Register](../templates/risk-register.md) (портфельный уровень)
- Conformance Reports

### Quality Gates

- [ ] Capability map согласована с бизнес-стейкхолдерами
- [ ] Target architecture одобрена ARB
- [ ] Architecture standards опубликованы и коммуницированы
- [ ] Governance process работает (ARB + office hours)
- [ ] ≥80% workstreams используют starter kit
- [ ] Conformance tracking настроен

### Типовые риски

| Риск | Mitigation |
|------|-----------|
| Ivory tower architecture (оторванность от delivery) | Архитекторы embedded в workstreams, regular feedback loops |
| Слишком много стандартов, adoption буксует | Начать с 3-5 ключевых, расширять по мере adoption |
| Зависимости между workstreams блокируют progress | Dependency board, regular sync, buffer в планах |
| Governance замедляет delivery | Timeboxed reviews, async по умолчанию, waiver process |
| Resistance to change | Quick wins, success stories, sponsor support |
