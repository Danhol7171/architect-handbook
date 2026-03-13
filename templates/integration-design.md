# Integration Design: [Название интеграции]

> Пример описания интеграции. Адаптируйте под ваш проект.

## Обзор

**Системы:** Система управления заказами ↔ ERP (1С)
**Направление:** двустороннее
**Паттерн:** асинхронный (event-driven) + синхронный (запрос остатков)

## Потоки данных

### Поток 1: Синхронизация остатков (ERP → Заказы)

**Паттерн:** Async (batch)
**Триггер:** Изменение остатков в ERP (каждые 15 мин)

```mermaid
sequenceDiagram
    participant ERP as ERP (1С)
    participant MQ as Брокер сообщений
    participant Catalog as Сервис каталога
    participant DB as БД каталога

    ERP->>MQ: StockUpdated (batch)
    MQ->>Catalog: Consume StockUpdated
    Catalog->>DB: Обновить остатки
    Catalog-->>MQ: ACK
```

### Поток 2: Создание заказа (Заказы → ERP)

**Паттерн:** Async (event)
**Триггер:** Подтверждение заказа клиентом

```mermaid
sequenceDiagram
    participant Order as Сервис заказов
    participant MQ as Брокер сообщений
    participant Adapter as ERP-адаптер
    participant ERP as ERP (1С)

    Order->>MQ: OrderConfirmed
    MQ->>Adapter: Consume OrderConfirmed
    Adapter->>ERP: POST /orders (SOAP/REST)
    ERP-->>Adapter: OrderID
    Adapter->>MQ: OrderSyncedToERP
```

### Поток 3: Проверка остатков в реальном времени

**Паттерн:** Sync (request-response)
**Триггер:** Добавление товара в корзину

```mermaid
sequenceDiagram
    participant UI as Фронтенд
    participant API as API Gateway
    participant Catalog as Сервис каталога
    participant Cache as Redis (кэш)

    UI->>API: GET /products/{id}/stock
    API->>Catalog: getStock(productId)
    Catalog->>Cache: Проверить кэш
    alt Кэш актуален
        Cache-->>Catalog: stockData
    else Кэш устарел
        Catalog->>Catalog: Вернуть из БД
    end
    Catalog-->>API: stockData
    API-->>UI: { available: true, quantity: 42 }
```

## Контракты

| Поток | Формат | Спецификация |
|-------|--------|-------------|
| StockUpdated | JSON (AsyncAPI) | `api/events/stock-updated.yaml` |
| OrderConfirmed | JSON (AsyncAPI) | `api/events/order-confirmed.yaml` |
| GET /products/{id}/stock | JSON (OpenAPI) | `api/openapi.yaml#/products/{id}/stock` |

## Error Handling

| Сценарий | Стратегия | Retry | DLQ |
|----------|----------|-------|-----|
| ERP недоступна | Retry с exponential backoff | 3 попытки, макс 5 мин | Да |
| Невалидные данные из ERP | Логирование + алерт | Нет | Да |
| Timeout синхронного запроса | Fallback на кэш | — | — |

## SLA интеграции

| Метрика | Целевое значение |
|---------|-----------------|
| Задержка синхронизации остатков | < 15 мин |
| Доставка события OrderConfirmed | < 30 сек |
| Латентность проверки остатков (sync) | < 100 мс |
