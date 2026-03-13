# Evidence Pack: [Название проекта/системы]

Чеклист источников данных для архитектурного аудита / assessment. Каждый finding в [Assessment Report](assessment-report.md) должен ссылаться на конкретный evidence item.

## Статус сбора

| Категория | Статус | Комментарий |
|-----------|:------:|------------|
| Репозитории | TODO | |
| Runtime | TODO | |
| Инфраструктура | TODO | |
| Данные | TODO | |
| Стоимость | TODO | |
| Документация | TODO | |
| Интервью | TODO | |

---

## 1. Репозитории (код и конфигурации)

- [ ] Основные репозитории кода (список, доступ)
- [ ] IaC-репозитории (Terraform, Ansible, Helm)
- [ ] CI/CD pipeline configs (Jenkinsfile, .gitlab-ci.yml, GitHub Actions)
- [ ] Dockerfile / docker-compose
- [ ] Конфигурация feature flags
- [ ] Dependency manifests (package.json, pom.xml, go.mod)

**Evidence ID:** REPO-NNN

---

## 2. Runtime (наблюдаемость)

- [ ] Метрики: Prometheus / Grafana dashboards (скриншоты, JSON export)
- [ ] Логи: структура логирования, уровни, centralized logging (ELK/Loki)
- [ ] Трассировки: distributed tracing (Jaeger/Tempo), примеры трейсов для ключевых сценариев
- [ ] Алерты: настроенные алерты, история срабатываний за последние 3 месяца
- [ ] SLO/SLI: текущие targets и фактические значения
- [ ] Error rates: статистика ошибок за последний месяц

**Evidence ID:** RUN-NNN

---

## 3. Инфраструктура

- [ ] Облачная консоль: list of resources, resource groups, subscriptions
- [ ] Сетевая топология: VPC/VNet, subnets, security groups, firewall rules
- [ ] DNS и балансировка: записи, load balancers, CDN
- [ ] Хранилища: типы, размеры, replication, backup schedule
- [ ] Compute: типы инстансов, autoscaling policies, utilization
- [ ] Kubernetes: cluster info, namespaces, resource quotas, HPA/VPA

**Evidence ID:** INFRA-NNN

---

## 4. Данные

- [ ] Схемы БД: ERD или DDL export
- [ ] Data flows: откуда приходят данные, куда уходят (ETL/ELT, streaming)
- [ ] Retention policies: сроки хранения, архивация
- [ ] PII mapping: какие данные являются персональными, где хранятся
- [ ] Backup/restore: политики, последний тест восстановления
- [ ] Data volumes: объёмы, темпы роста

**Evidence ID:** DATA-NNN

---

## 5. Стоимость

- [ ] Cloud billing: расходы за последние 3-6 месяцев (по сервисам)
- [ ] Cost allocation: теги, распределение по командам/сервисам
- [ ] Reserved instances / savings plans: текущее покрытие
- [ ] Idle resources: неиспользуемые ресурсы (диски, IP, LB)
- [ ] License costs: коммерческое ПО, managed services

**Evidence ID:** COST-NNN

---

## 6. Документация (существующая)

- [ ] Архитектурные документы: ADR, design docs, RFC
- [ ] Wiki / Confluence: ссылки на ключевые страницы
- [ ] Runbooks: операционные инструкции
- [ ] Postmortems / incident reports: за последние 6 месяцев
- [ ] API-документация: Swagger/OpenAPI, Postman collections
- [ ] Onboarding guides

**Evidence ID:** DOC-NNN

---

## 7. Интервью

| # | Стейкхолдер | Роль | Дата | Ключевые темы | Статус |
|---|------------|------|------|---------------|:------:|
| 1 | TODO | TODO | TODO | TODO | TODO |
| 2 | TODO | TODO | TODO | TODO | TODO |
| 3 | TODO | TODO | TODO | TODO | TODO |

**Минимум:** 3-5 интервью для L0, 5-10 для L1.

### Типовые вопросы для интервью

**Для разработчиков:**
- Какие части системы сложнее всего менять? Почему?
- Какие инциденты были за последние 6 месяцев?
- Что замедляет delivery?

**Для ops/SRE:**
- Как устроен деплой? Сколько времени занимает?
- Какие алерты срабатывают чаще всего?
- Есть ли задокументированные runbooks?

**Для бизнеса:**
- Какие бизнес-процессы зависят от системы?
- Планы по росту (пользователи, транзакции, данные)?
- Какие compliance-требования?

**Evidence ID:** INT-NNN

---

## Привязка evidence к findings

| Finding # | Описание | Evidence items | Критичность |
|:---------:|---------|---------------|:-----------:|
| F-001 | TODO | REPO-001, RUN-003 | Высокая / Средняя / Низкая |
