# Микросеврс для анализа трендов криптоактивов

Вам необходимо разработать микросервис для отслеживания и анализа транзакций с криптовалютой, предоставляющий информацию о рыночных тенденциях и активности пользователей. Микросервис должен быть написан на Golang и использовать ClickHouse для хранения данных.

## Требуемый функционал

1. **Добавление транзакции**: Логирование транзакций криптовалюты с указанием ID транзакции, ID пользователя, типа криптовалюты, суммы и времени.
2. **Получение рыночных тенденций**: Извлечение общего объема транзакций, уникального числа пользователей и самых торгуемых криптовалют за указанный период.
3. **Получение портфеля пользователя**: Получение информации о портфеле пользователя, включая общие активы и историю транзакций.

## Эндпоинты

1. **Добавление транзакции**

    - **Эндпоинт**: `/v1/add_transaction`
    - **Метод**: `POST`
    - **Тело запроса (JSON)**:
      ```json
      {
        "transaction_id": "string",
        "user_id": "string",
        "crypto_type": "string",
        "amount": "float",
        "timestamp": "string (ISO8601)"
      }
      ```
    - **Ответ**:
      - `200 OK`: Транзакция успешно добавлена.
      - `400 Bad Request`: Неверное тело запроса.
      - `500 Internal Server Error`: Ошибка сохранения транзакции.

2. **Получение рыночных тенденций**

    - **Эндпоинт**: `/v1/get_market_trends`
    - **Метод**: `POST`
    - **Параметры запроса**:
      - `from (обязательно)`: Начало периода (в формате ISO8601).
      - `to (обязательно)`: Конец периода (в формате ISO8601).
    - **Ответ (JSON)**:
      ```json
      {
        "total_volume": "float",
        "unique_users": "int",
        "top_cryptocurrencies": [
          {
            "crypto_type": "string",
            "total_volume": "float"
          }
        ]
      }
      ```
    - **Ошибки**:
      - `400 Bad Request`: Отсутствуют или некорректные параметры запроса.
      - `500 Internal Server Error`: Ошибка получения рыночных тенденций.

3. **Получение портфеля пользователя**

    - **Эндпоинт**: `/v1/get_user_portfolio`
    - **Метод**: `GET`
    - **Параметры запроса**:
      - `user_id (обязательно)`: ID пользователя.
    - **Ответ (JSON)**:
      ```json
      {
        "user_id": "string",
        "total_holdings": "float",
        "transaction_history": [
          {
            "transaction_id": "string",
            "crypto_type": "string",
            "amount": "float",
            "timestamp": "string (ISO8601)"
          }
        ]
      }
      ```
    - **Ошибки**:
      - `400 Bad Request`: Отсутствуют или некорректные параметры запроса.
      - `500 Internal Server Error`: Ошибка получения портфеля пользователя.

## Используемые технологии

Для выполнения задания необходимо использовать следующие технологии:

- **Golang**: Основной язык программирования для разработки микросервиса.
- **ClickHouse**: База данных для быстрого и эффективного хранения данных и аналитики.
- **Docker**: Опционально, для контейнеризации и упрощения развертывания ClickHouse.

## Пример использования

**Добавление транзакции**
```bash
curl -X POST http://localhost:8080/v1/add_transaction \
-H "Content-Type: application/json" \
-d '{
  "transaction_id": "txn123",
  "user_id": "user123",
  "crypto_type": "BTC",
  "amount": 0.05,
  "timestamp": "2025-01-19T10:00:00Z"
}'
```

**Получение рыночных тенденций**
```bash
curl -X POST "http://localhost:8080/v1/get_market_trends?from=2025-01-01T00:00:00Z&to=2025-01-18T23:59:59Z"
```

**Получение портфеля пользователя**
```bash
curl -X GET "http://localhost:8080/v1/get_user_portfolio?user_id=user123"
```

## Заключение

Это задание направлено на разработку микросервиса, который позволит эффективно анализировать данные о транзакциях с криптовалютой. Успешное выполнение задания продемонстрирует ваши навыки в области разработки микросервисов и работы с базами данных.