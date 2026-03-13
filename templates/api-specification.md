# API Specification: [Название API]

> Пример минимальной OpenAPI спецификации. Для реального проекта используйте полноценный `openapi.yaml`.

## OpenAPI 3.1 (фрагмент)

```yaml
openapi: 3.1.0
info:
  title: Order Management API
  version: 1.0.0
  description: API управления заказами

servers:
  - url: https://api.example.com/v1
    description: Production

paths:
  /orders:
    post:
      summary: Создать заказ
      operationId: createOrder
      tags: [orders]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
      responses:
        '201':
          description: Заказ создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          description: Невалидные данные
        '401':
          description: Не аутентифицирован

    get:
      summary: Список заказов
      operationId: listOrders
      tags: [orders]
      parameters:
        - name: status
          in: query
          schema:
            type: string
            enum: [new, confirmed, shipped, delivered, cancelled]
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
        - name: offset
          in: query
          schema:
            type: integer
            default: 0
      responses:
        '200':
          description: Список заказов
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/OrderList'

components:
  schemas:
    CreateOrderRequest:
      type: object
      required: [items]
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
        comment:
          type: string
          maxLength: 500

    OrderItem:
      type: object
      required: [productId, quantity]
      properties:
        productId:
          type: string
          format: uuid
        quantity:
          type: integer
          minimum: 1

    Order:
      type: object
      properties:
        id:
          type: string
          format: uuid
        status:
          type: string
          enum: [new, confirmed, shipped, delivered, cancelled]
        items:
          type: array
          items:
            $ref: '#/components/schemas/OrderItem'
        totalAmount:
          type: number
          format: decimal
        createdAt:
          type: string
          format: date-time

    OrderList:
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/Order'
        total:
          type: integer

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

## Правила линтинга (.spectral.yaml)

```yaml
extends: spectral:oas
rules:
  operation-operationId: error
  operation-description: warn
  oas3-valid-schema-example: error
```

## AsyncAPI (пример события)

```yaml
asyncapi: 2.6.0
info:
  title: Order Events
  version: 1.0.0

channels:
  orders.confirmed:
    publish:
      message:
        payload:
          type: object
          properties:
            orderId:
              type: string
              format: uuid
            confirmedAt:
              type: string
              format: date-time
            totalAmount:
              type: number
```
