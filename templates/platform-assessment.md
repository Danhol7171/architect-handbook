# Platform Assessment: [Название организации / подразделения]

> Шаблон оценки текущего состояния платформы разработки и рекомендаций по внедрению IDP. Используется в [Playbook: Platform Engineering](../docs/playbooks.md#platform-engineering). Ведёт [Technical Architect](../docs/roles.md#technical-architect-technology-architect).

---

## Метаданные

| Параметр | Значение |
|----------|---------|
| **Организация** | [Название] |
| **Кол-во продуктовых команд** | [N] |
| **Кол-во разработчиков** | [N] |
| **Дата оценки** | YYYY-MM-DD |
| **Автор** | [Имя, роль] |

---

## 1. Текущее состояние (As-Is)

### Инфраструктура и инструменты

| Категория | Текущее решение | Кол-во вариантов | Стандартизировано |
|-----------|---------------|:----------------:|:----------------:|
| CI/CD | [Jenkins / GitLab CI / GitHub Actions / ...] | [N] | ✅ / ❌ |
| Container orchestration | [K8s / ECS / Nomad / none] | [N] | ✅ / ❌ |
| IaC | [Terraform / Pulumi / CloudFormation / none] | [N] | ✅ / ❌ |
| Мониторинг | [Prometheus / Datadog / CloudWatch / ...] | [N] | ✅ / ❌ |
| Логирование | [ELK / Loki / CloudWatch Logs / ...] | [N] | ✅ / ❌ |
| Secret management | [Vault / AWS SM / env vars / ...] | [N] | ✅ / ❌ |
| Service catalog | [Backstage / wiki / spreadsheet / none] | [N] | ✅ / ❌ |

### Developer Experience Survey

> Провести опрос разработчиков (target: ≥70% response rate).

| Вопрос | Оценка (1–5) | Комментарии |
|--------|:------------:|-------------|
| Как быстро можно создать новый сервис и задеплоить в prod? | | |
| Насколько легко найти документацию по сервисам других команд? | | |
| Насколько стандартизированы CI/CD pipelines? | | |
| Как часто вы блокированы из-за инфраструктурных запросов? | | |
| Насколько легко настроить мониторинг для нового сервиса? | | |
| Как вы оцениваете cognitive load при работе с инфраструктурой? | | |

### DORA Baseline

| Метрика | Текущее значение | Industry benchmark (medium) |
|---------|:----------------:|:--------------------------:|
| Deployment Frequency | [N/день, N/неделю, N/мес] | 1/неделю – 1/мес |
| Lead Time for Changes | [дни / недели] | 1 день – 1 неделя |
| Change Failure Rate | [%] | 0–15% |
| MTTR | [часы / дни] | < 1 дня |
| Deployment Rework Rate | [%] | < 10% |

---

## 2. Maturity Assessment

> На основе CNCF Platform Engineering Maturity Model.

| Аспект | Текущий уровень | Target | Gap |
|--------|:--------------:|:------:|:---:|
| **Provisioning** | [1–4] | [1–4] | [Δ] |
| **Security** | [1–4] | [1–4] | [Δ] |
| **Observability** | [1–4] | [1–4] | [Δ] |
| **Developer portal** | [1–4] | [1–4] | [Δ] |
| **Documentation** | [1–4] | [1–4] | [Δ] |
| **Golden paths** | [1–4] | [1–4] | [Δ] |
| **Measurement** | [1–4] | [1–4] | [Δ] |

**Уровни:** 1 = Provisional, 2 = Operationalized, 3 = Scalable, 4 = Optimizing

---

## 3. Боли и возможности

### Top-5 болей

| # | Боль | Кого затрагивает | Impact (1–5) | Частота |
|---|------|-----------------|:------------:|---------|
| 1 | [Долгий onboarding новых сервисов] | [Все команды] | [N] | [Каждый проект] |
| 2 | [Нестандартные CI/CD pipelines] | [DevOps, разработчики] | [N] | [Постоянно] |
| 3 | [Нет единого каталога сервисов] | [Архитекторы, новые сотрудники] | [N] | [Постоянно] |
| 4 | | | | |
| 5 | | | | |

### Возможности

| Возможность | Потенциальная экономия | Сложность |
|------------|:----------------------:|:---------:|
| Golden path для REST API | ~2 дня на каждый новый сервис | Medium |
| Стандартный CI/CD pipeline | ~1 день на каждый проект | Low |
| Service catalog (ownership) | Снижение MTTR на ~30% | Medium |
| Self-service infrastructure | ~3 дня на каждый запрос | High |

---

## 4. Рекомендации

### MVP Scope (Quick Wins)

| Capability | Описание | Effort | Impact |
|-----------|---------|:------:|:------:|
| Service catalog | Backstage с catalog-info.yaml для существующих сервисов | 2–4 недели | High |
| CI/CD templates | 2–3 стандартных pipeline для основных стеков | 2–3 недели | High |
| Golden path: REST API | Шаблон создания нового REST-сервиса | 3–4 недели | High |

### Дорожная карта

| Горизонт | Что | Предпосылки | Метрика успеха |
|---------|-----|-------------|---------------|
| 0–3 мес | MVP: catalog + CI/CD templates + 1 golden path | Platform team (≥2 чел) | 2–3 early adopter команды |
| 3–6 мес | Self-service infra, observability, TechDocs | MVP в проде | 50% команд используют |
| 6–12 мес | Полный self-service, DevEx dashboard, cost visibility | Feedback от команд | 80%+ adoption, NPS > 30 |

### Cost & ROI

| Категория | Стоимость (год) | Примечание |
|-----------|:--------------:|------------|
| Platform Team (2–3 FTE) | [N] | Выделенная команда |
| Backstage hosting | [N] | Managed / self-hosted |
| Tooling licenses | [N] | Если применимо |
| **Итого** | **[N]** | |
| **Ожидаемая экономия** | **[N]** | На основе DevEx survey + DORA improvement |

---

## 5. Adoption Strategy

| Фаза | Подход | Команды | Критерий перехода |
|------|--------|---------|-------------------|
| Pilot | Voluntary, early adopters | 2–3 | Положительный feedback, DORA ↑ |
| Scale | Incentivized (упрощение compliance) | 50% | Platform stable, golden paths ready |
| Standard | Required для новых проектов | 80%+ | Self-service работает, DevEx > 3.5/5 |

---

## Связанные артефакты

- [Playbook: Platform Engineering](../docs/playbooks.md#platform-engineering) — пошаговая инструкция
- [NFR Checklist](nfr-checklist.md) — NFR для платформы
- [Cost Model](cost-model.md) — модель стоимости платформы
- [SLO/SLI Profile](slo-sli-profile.md) — SLO для платформенных сервисов
- [Architecture Principles](architecture-principles.md) — принципы платформы (ADR)
