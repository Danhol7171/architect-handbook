# Data Model: [Название проекта]

> Пример модели данных. Адаптируйте под ваш проект.

## ER-диаграмма (основные сущности)

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : "оформляет"
    ORDER ||--|{ ORDER_ITEM : "содержит"
    ORDER_ITEM }o--|| PRODUCT : "ссылается"
    PRODUCT }o--|| CATEGORY : "принадлежит"
    ORDER ||--o| PAYMENT : "оплачивается"
    ORDER ||--o| SHIPMENT : "доставляется"

    CUSTOMER {
        uuid id PK
        string email UK
        string phone
        string name
        timestamp created_at
    }

    ORDER {
        uuid id PK
        uuid customer_id FK
        string status
        decimal total_amount
        timestamp created_at
        timestamp updated_at
    }

    ORDER_ITEM {
        uuid id PK
        uuid order_id FK
        uuid product_id FK
        int quantity
        decimal price
    }

    PRODUCT {
        uuid id PK
        uuid category_id FK
        string name
        decimal price
        int stock
        jsonb attributes
    }

    CATEGORY {
        uuid id PK
        string name
        uuid parent_id FK
    }

    PAYMENT {
        uuid id PK
        uuid order_id FK
        string status
        decimal amount
        string provider
        timestamp paid_at
    }

    SHIPMENT {
        uuid id PK
        uuid order_id FK
        string status
        string tracking_number
        timestamp shipped_at
        timestamp delivered_at
    }
```

## Ключевые решения

| Решение | Обоснование |
|---------|-------------|
| UUID для PK | Генерация на стороне приложения, безопасность при интеграции |
| JSONB для атрибутов товара | Гибкая схема для разных категорий без EAV |
| Decimal для денежных сумм | Точность финансовых вычислений |
| Soft delete (status) | Сохранение истории для аналитики |

## Индексы

| Таблица | Индекс | Тип | Назначение |
|---------|--------|-----|------------|
| orders | (customer_id, created_at DESC) | btree | Список заказов клиента |
| orders | (status) | btree | Фильтрация по статусу |
| products | (category_id) | btree | Каталог по категориям |
| products | (attributes) | gin | Поиск по атрибутам |
