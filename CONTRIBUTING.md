# Участие в проекте

Спасибо за интерес к Architect Handbook! Мы рады вкладу любого уровня — от исправления опечаток до новых шаблонов и практик.

## Как внести вклад

1. Форкните репозиторий
2. Создайте ветку для ваших изменений (`git checkout -b feature/my-improvement`)
3. Внесите изменения
4. Создайте Pull Request с описанием того, что и зачем вы изменили

## Что можно улучшить

- Исправления и уточнения в существующих документах
- Новые шаблоны артефактов в `templates/`
- Дополнения к практикам на основе реального опыта
- Переводы на другие языки

## Коммиты

Используем [Conventional Commits](https://www.conventionalcommits.org/). Сообщение коммита на английском.

### Формат

```
<type>(<scope>): <description>
```

### Типы

| Тип | Когда использовать |
|-----|-------------------|
| `docs` | Изменение содержания методики или шаблонов |
| `feat` | Новый раздел, шаблон или практика |
| `fix` | Исправление ошибок, битых ссылок, опечаток |
| `chore` | Репозиторий, CI, .gitignore, лицензия |
| `refactor` | Реструктуризация без изменения содержания |

### Scope (опционально)

`practices`, `templates`, `roles`, `process`, `playbooks`, `governance`, `tools`, `research`

### Примеры

```
docs(practices): add observability practice details
feat(templates): add migration plan template
fix(templates): broken link in threat-model
chore: update .gitignore
refactor(docs): split practices into separate files
```

## Рекомендации по оформлению

- Документация пишется на русском языке
- Формат файлов — Markdown (.md)
- Диаграммы — в нотации Mermaid
- Без формализма: пишите так, как объясняли бы коллеге

## Структура каталогов

- `docs/` — основные разделы методики (русский, primary)
- `templates/` — шаблоны архитектурных артефактов
- `research/` — исследовательские материалы
- `i18n/<lang>/` — переводы (зеркальная структура `docs/` и `templates/`)

## Переводы

Основной язык — русский. Переводы живут в `i18n/<lang>/`.

### Как переводить

1. Создайте файл с той же структурой: `i18n/en/docs/practices.md`
2. Добавьте frontmatter с указанием источника:

```yaml
---
source: docs/practices.md
source_hash: <first 7 chars of `git hash-object docs/practices.md`>
---
```

3. `source_hash` позволяет отслеживать, когда перевод устарел относительно оригинала

### Scope для коммитов переводов

```
docs(i18n): translate core-standard to English
fix(i18n): update outdated translation of roles
```

## Лицензия

Внося вклад в этот проект, вы соглашаетесь с тем, что ваши изменения будут распространяться под лицензией [CC BY-SA 4.0](LICENSE).