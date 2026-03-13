# C4 Container Diagram: [Название системы]

> Пример. Показывает основные технические блоки системы.

```mermaid
graph TD
    User["Клиент<br>(браузер)"] --> WebApp["Веб-приложение<br>(React SPA)"]
    Admin["Администратор<br>(браузер)"] --> AdminUI["Админ-панель<br>(React SPA)"]

    WebApp --> API["API Gateway<br>(Kong/Nginx)"]
    AdminUI --> API

    API --> OrderSvc["Сервис заказов<br>(Java/Spring Boot)"]
    API --> CatalogSvc["Сервис каталога<br>(Go)"]
    API --> UserSvc["Сервис пользователей<br>(Java/Spring Boot)"]

    OrderSvc --> OrderDB[("PostgreSQL<br>заказы")]
    CatalogSvc --> CatalogDB[("PostgreSQL<br>каталог")]
    UserSvc --> UserDB[("PostgreSQL<br>пользователи")]

    OrderSvc --> MQ["Брокер сообщений<br>(RabbitMQ)"]
    MQ --> NotifySvc["Сервис уведомлений<br>(Python)"]
    MQ --> AnalyticsSvc["Аналитика<br>(Python)"]

    AnalyticsSvc --> ClickHouse[("ClickHouse<br>аналитика")]

    style API fill:#3B2066,color:#fff
    style OrderSvc fill:#3B2066,color:#fff
    style CatalogSvc fill:#3B2066,color:#fff
    style UserSvc fill:#3B2066,color:#fff
    style NotifySvc fill:#3B2066,color:#fff
    style AnalyticsSvc fill:#3B2066,color:#fff
    style MQ fill:#E8772E,color:#fff
```

## Описание контейнеров

| Контейнер | Технология | Назначение |
|-----------|-----------|------------|
| Веб-приложение | React SPA | Интерфейс клиента |
| Админ-панель | React SPA | Управление каталогом и заказами |
| API Gateway | Kong/Nginx | Маршрутизация, rate limiting, аутентификация |
| Сервис заказов | Java/Spring Boot | Бизнес-логика заказов |
| Сервис каталога | Go | CRUD каталога, поиск |
| Сервис пользователей | Java/Spring Boot | Регистрация, профили, авторизация |
| Сервис уведомлений | Python | Отправка SMS/email по событиям |
| Аналитика | Python | Обработка и агрегация событий |
| Брокер сообщений | RabbitMQ | Асинхронное взаимодействие между сервисами |
