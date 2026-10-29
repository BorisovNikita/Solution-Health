## Спецификация OpenAPI (YAML)

```yaml
openapi: 3.2.0
info:
  title: API "Здоровье+"
  version: 1.0.0
servers:
  - url: https://api.zdorovie.plus/api/v1 # Версионирование в URL
paths:
  /appointments:
    get:
      summary: Получение списка предстоящих записей
      security:
        - BearerAuth: [] # Требуется аутентификация
      parameters:
        - name: status
          in: query
          schema:
            type: string
            enum: [upcoming, past]
            default: upcoming
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
        - name: offset
          in: query
          schema:
            type: integer
            default: 0
      responses:
        '200':
          description: Успешный ответ
          content:
            application/json:
              example:
                data:
                  - id: "appt-1"
                    doctor_name: "Иванов И.И."
                    date_time: "2024-06-15T10:30:00Z"
                pagination: { total: 1, limit: 10, offset: 0 }
        '401':
          description: Неавторизован (нет или просрочен токен)
          content:
            application/json:
              example: { error: "Unauthorized", message: "Token is missing or invalid" }
        '500':
          description: Внутренняя ошибка сервера
          content:
            application/json:
              example: { error: "Internal Server Error" }
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

Пагинация (limit/offset): Выбрана для простоты реализации бесконечного скролла в мобильном приложении. Клиенту легко запрашивать следующую порцию данных при прокрутке списка.

Версионирование (/api/v1/): Версия указана в URL. Это позволяет безболезненно выпускать обновления (v2), не ломая работу старых версий мобильного приложения, которые уже установлены у пациентов.