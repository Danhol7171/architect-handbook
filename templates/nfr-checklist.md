# NFR Checklist: [Название проекта]

> Пример заполнения. Адаптируйте под ваш проект.

## Инструкция

Для каждой категории: определите целевое значение, приоритет (Must/Should/Could) и способ проверки. Не все категории обязательны — отметьте N/A для неприменимых.

## Checklist

| Категория | Требование | Целевое значение | Приоритет | Способ проверки |
|-----------|-----------|------------------|-----------|-----------------|
| **Scalability** | Макс. одновременных пользователей | 10 000 | Must | Нагрузочное тестирование |
| **Scalability** | Рост объёма данных (в год) | до 500 ГБ | Should | Мониторинг |
| **Performance** | Латентность API (p95) | < 200 мс | Must | APM-мониторинг |
| **Performance** | Латентность API (p99) | < 500 мс | Should | APM-мониторинг |
| **Performance** | Пропускная способность | 1000 RPS | Must | Нагрузочное тестирование |
| **Availability** | SLA (уровень доступности) | 99.9% | Must | Мониторинг uptime |
| **Availability** | RTO (время восстановления) | < 1 час | Must | DR-учения |
| **Availability** | RPO (допустимая потеря данных) | < 5 мин | Must | Проверка бэкапов |
| **Recoverability** | Резервное копирование | Ежедневно + WAL | Must | Тест восстановления |
| **Recoverability** | DR (аварийное восстановление) | Standby в другом ДЦ | Should | DR-учения (раз в квартал) |
| **Security** | Аутентификация | OAuth 2.0 + MFA | Must | Пентест |
| **Security** | Шифрование данных at rest | AES-256 | Must | Аудит конфигурации |
| **Security** | Шифрование данных in transit | TLS 1.3 | Must | Сканирование |
| **Security** | Соответствие 152-ФЗ | Да | Must | Аудит |
| **Maintainability** | Покрытие тестами | > 80% | Should | CI pipeline |
| **Maintainability** | Цикломатическая сложность | < 15 | Should | Статический анализ |
| **Operability** | Время развёртывания | < 15 мин | Should | CI/CD метрики |
| **Operability** | Наблюдаемость | Traces + Metrics + Logs | Must | Чеклист observability |
| **Cost** | Ежемесячный бюджет инфраструктуры | < 500 000 ₽ | Should | FinOps мониторинг |
| **Cost** | Стоимость на транзакцию | < 0.5 ₽ | Should | Unit economics dashboard |
| **Sustainability** | SCI (Software Carbon Intensity) | < 5 gCO₂/транзакция | Could | Cloud Carbon Footprint |
| **Sustainability** | Energy efficiency | ARM instances где применимо | Could | Infrastructure audit |
| **Sustainability** | Idle resource ratio | < 10% | Should | Мониторинг утилизации |

## Приоритизация

- **Must** — обязательно к реализации, блокер для запуска
- **Should** — важно, но допускается временный компромисс
- **Could** — желательно, реализуется при наличии ресурсов
