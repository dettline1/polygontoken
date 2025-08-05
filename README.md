# 🚀 Polygon Token Analyzer

Полнофункциональное приложение для анализа ERC20 токенов в сети Polygon. Поддерживает получение балансов, анализ топ-холдеров, информацию о транзакциях и HTTP API.

## 📋 Функциональность

### ✅ Уровень A - Баланс адреса
Получение баланса конкретного адреса для указанного токена.

**Пример использования:**
```python
balance_tokens, balance_wei = analyzer.get_balance("0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d")
print(f"Баланс: {balance_tokens} TBY")
```

### ✅ Уровень B - Пакетные балансы
Получение балансов нескольких адресов одновременно.

**Пример использования:**
```python
addresses = ["0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d", "0x4830AF4aB9cd9E381602aE50f71AE481a7727f7C"]
balances = analyzer.get_balance_batch(addresses)
```

### ✅ Уровень C - Топ холдеры
Получение списка адресов с наибольшими балансами токена.

**Пример использования:**
```python
top_holders = analyzer.get_top(10)  # Топ-10 холдеров
for address, balance in top_holders:
    print(f"{address}: {balance} TBY")
```

### ✅ Уровень D - Топ холдеры с транзакциями
Расширенная информация о топ-холдерах с датами последних транзакций.

**Пример использования:**
```python
top_with_dates = analyzer.get_top_with_transactions(5)
for address, balance, last_tx_date in top_with_dates:
    print(f"{address}: {balance} TBY (последняя транзакция: {last_tx_date})")
```

### ✅ Уровень E - Информация о токене
Получение метаданных токена: название, символ, decimals, total supply.

**Пример использования:**
```python
token_info = analyzer.get_token_info("0x1a9b54a3075119f1546c52ca0940551a6ce5d2d0")
print(f"Токен: {token_info['name']} ({token_info['symbol']})")
```

### ✅ Уровень F - HTTP API сервер
REST API сервер для всех функций с JSON ответами.

## 🛠 Установка и настройка

### Требования
- Python 3.8+
- Интернет соединение для доступа к Polygon RPC

### Установка зависимостей
```bash
pip install -r requirements.txt
```

### Структура проекта
```
polygon-token-analyzer/
├── main.py              # Основной файл приложения
├── requirements.txt     # Зависимости Python
└── README.md           # Документация
```

## 🚀 Запуск

### 1. Тестирование функций (CLI)
```bash
python main.py
```

Выведет результаты тестирования всех уровней функциональности.

### 2. Запуск HTTP сервера
```bash
python main.py --server
```

Сервер будет доступен по адресу: `http://localhost:8080`

## 🌐 HTTP API

### Endpoints

#### GET /get_balance
Получение баланса одного адреса.

**Параметры:**
- `address` (string) - адрес кошелька

**Пример запроса:**
```bash
curl "http://localhost:8080/get_balance?address=0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d"
```

**Ответ:**
```json
{
  "address": "0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d",
  "balance": 0.01,
  "balanceWei": "10000000000000000",
  "symbol": "TBY"
}
```

#### POST /get_balance_batch
Получение балансов нескольких адресов.

**Body (JSON):**
```json
{
  "addresses": [
    "0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d",
    "0x4830AF4aB9cd9E381602aE50f71AE481a7727f7C"
  ]
}
```

**Пример запроса:**
```bash
curl -X POST http://localhost:8080/get_balance_batch \
  -H "Content-Type: application/json" \
  -d '{"addresses": ["0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d", "0x4830AF4aB9cd9E381602aE50f71AE481a7727f7C"]}'
```

**Ответ:**
```json
{
  "addresses": ["0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d", "0x4830AF4aB9cd9E381602aE50f71AE481a7727f7C"],
  "balances": [0.01, 0.01],
  "symbol": "TBY"
}
```

#### GET /get_top
Получение топ холдеров токена.

**Параметры:**
- `n` (int, default=10) - количество адресов

**Пример запроса:**
```bash
curl "http://localhost:8080/get_top?n=5"
```

**Ответ:**
```json
{
  "top": [
    ["0x1a9b54a3075119f1546c52ca0940551a6ce5d2d0", 1000000.0],
    ["0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d", 0.01]
  ],
  "count": 2,
  "symbol": "TBY"
}
```

#### GET /get_top_with_transactions
Топ холдеры с информацией о последних транзакциях.

**Параметры:**
- `n` (int, default=10) - количество адресов

**Пример запроса:**
```bash
curl "http://localhost:8080/get_top_with_transactions?n=3"
```

**Ответ:**
```json
{
  "top": [
    ["0x1a9b54a3075119f1546c52ca0940551a6ce5d2d0", 1000000.0, "2024-01-15 14:30:25"],
    ["0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d", 0.01, null]
  ],
  "count": 2,
  "symbol": "TBY"
}
```

#### GET /get_token_info
Получение информации о токене.

**Параметры:**
- `address` (string, optional) - адрес токена (по умолчанию TBY)

**Пример запроса:**
```bash
curl "http://localhost:8080/get_token_info"
```

**Ответ:**
```json
{
  "address": "0x1a9b54a3075119f1546c52ca0940551a6ce5d2d0",
  "symbol": "TBY",
  "name": "Storage GasToken",
  "decimals": 18,
  "totalSupply": 1000000.0,
  "totalSupplyWei": "1000000000000000000000000"
}
```

#### GET /health
Проверка работоспособности сервера.

**Пример запроса:**
```bash
curl "http://localhost:8080/health"
```

**Ответ:**
```json
{
  "status": "healthy",
  "connected": true,
  "token_address": "0x1a9b54a3075119f1546c52ca0940551a6ce5d2d0",
  "symbol": "TBY"
}
```

## ⚙️ Конфигурация

### Токен по умолчанию
Приложение настроено для работы с токеном TBY:
- **Адрес:** `0x1a9b54a3075119f1546c52ca0940551a6ce5d2d0`
- **Сеть:** Polygon Mainnet

### RPC Endpoints
Используются бесплатные Polygon RPC endpoints:
- `https://polygon-rpc.com`
- `https://rpc-mainnet.maticvigil.com`
- `https://poly-rpc.gateway.pokt.network`

## 🔧 Архитектура

### Основные компоненты

1. **PolygonTokenAnalyzer** - основной класс для работы с блокчейном
2. **Web3 Integration** - подключение к Polygon через web3.py
3. **ERC20 Contract** - взаимодействие с токеном через стандартный ABI
4. **Flask API** - HTTP сервер для внешних запросов
5. **Async Data Fetching** - асинхронное получение данных о транзакциях

### Особенности реализации

- **Отказоустойчивость**: несколько RPC endpoints для надежности
- **Производительность**: асинхронные запросы для получения данных
- **Экономичность**: минимальное использование внешних API
- **Безопасность**: валидация адресов и обработка ошибок
- **Логирование**: подробные логи для отладки

## 📊 Производительность

- **Получение баланса**: ~1-2 секунды
- **Пакетные балансы**: ~N секунд для N адресов
- **Топ холдеры**: ~10-30 секунд (зависит от количества)
- **Информация о транзакциях**: +5-10 секунд на каждый адрес

## 🚨 Ограничения

1. **Rate Limits**: API PolygonScan может ограничивать запросы
2. **Топ холдеры**: сложность получения всех холдеров без индексатора
3. **Исторические данные**: ограниченная глубина истории транзакций
4. **Сетевые задержки**: зависимость от скорости RPC endpoints

## 🛡️ Обработка ошибок

- Автоматическое переключение между RPC endpoints
- Graceful обработка недоступных адресов
- Логирование всех ошибок
- Возврат пустых результатов вместо падения приложения

## 🔍 Примеры использования

### Мониторинг крупных холдеров
```python
# Получить топ-20 холдеров с датами активности
top_holders = analyzer.get_top_with_transactions(20)
for addr, balance, last_tx in top_holders:
    if balance > 1000:  # Только крупные холдеры
        print(f"🐋 {addr}: {balance:,.2f} TBY (активность: {last_tx})")
```

### Анализ распределения токенов
```python
# Получить общую информацию о токене
token_info = analyzer.get_token_info()
top_10 = analyzer.get_top(10)

total_supply = token_info['totalSupply']
top_10_sum = sum(balance for _, balance in top_10)
concentration = (top_10_sum / total_supply) * 100

print(f"Концентрация топ-10: {concentration:.2f}%")
```

### Мониторинг конкретных адресов
```python
# Отслеживание балансов важных адресов
important_addresses = [
    "0x51f1774249Fc2B0C2603542Ac6184Ae1d048351d",  # Адрес 1
    "0x4830AF4aB9cd9E381602aE50f71AE481a7727f7C"   # Адрес 2
]

balances = analyzer.get_balance_batch(important_addresses)
for addr, balance in zip(important_addresses, balances):
    print(f"📊 {addr}: {balance} TBY")
```
