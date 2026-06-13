# Портфолио по тестированию
**Меня зовут Наталья.** Я начинающий тестировщик.  
Представляю свои учебные проекты для демонстрации навыков.

## 📌 Оглавление
0. Мой опыт как основа для тестирования
1. Функциональное тестирование (SauceDemo)
2. Работа с SQL (FTB)
3. Тестирование API (JSONPlaceholder)

## 0. 🎯 Мой опыт как основа для тестирования

До перехода в QA я 10+ лет работала с данными и процессами. Это дало мне:

**UAT и аналитика:**
- Участвовала в автоматизации оплат: совместно с коллегами обосновывала ROI (анализ данных, расчет рисков), готовила ТЗ, приоритизировала задачи.
- Проводила UAT: проверяла сценарии, выявляла несоответствия, контролировала исправления.
- Результат: функция внедрена, ручной труд сокращен.

**Работа с данными и багами:**
- Ежемесячно и ежеквартально анализировала оплаты 30+ филиалов, сверяла SAP CRM с первичной документацией.
- Классифицировала ошибки (критичные/некритичные), координировала исправления.
Итог: снижение просрочек на 30%, внедрение регламента работы с филиалами.
- Дополнительно: анализ тендерной документации (выявление условий → выигрыш 3 тендеров на 5+ млн руб.), контроль выплатных дел (сокращение срока выплат с 60 до 20 дней).

**Почему это важно для QA:**  
Я уже умею находить несоответствия, приоритизировать баги, работать с ТЗ и бизнесом — переношу эти навыки на тестирование ПО.

## 1. 🧪 Функциональное тестирование (SauceDemo — интернет-магазин (учебный))

### 📋 Чек-лист авторизации

| № | Проверка | Логин | Пароль | Ожидание | Результат |
|---|----------|-------|--------|----------|-----------|
| 1 | Успешный вход (standard_user) | standard_user | secret_sauce | редирект на /inventory.html | ОК |
| 2 | Успешный вход (problem_user) | problem_user | secret_sauce | редирект на /inventory.html | ОК |
| 3 | Успешный вход (performance_glitch_user) | performance_glitch_user | secret_sauce | редирект на /inventory.html | ОК |
| 4 | Пустой логин | (пусто) | secret_sauce | Epic sadface: Username is required | ОК |
| 5 | Пустой пароль | standard_user | (пусто) | Epic sadface: Password is required | ОК |
| 6 | Оба поля пустые | (пусто) | (пусто) | Epic sadface: Username is required | ОК |
| 7 | Неверный логин | fake_user | secret_sauce | Epic sadface: Username and password do not match any user in this service | ОК |
| 8 | Неверный пароль | standard_user | fake_pass | Epic sadface: Username and password do not match any user in this service | ОК |

---

### 🧾 Тест-кейс

**TC-01 – Успешная авторизация с корректным пользователем**

**Предусловия:** открыта страница https://www.saucedemo.com/

**Шаги:**
1. В поле Username ввести `standard_user`
2. В поле Password ввести `secret_sauce`
3. Нажать кнопку Login

**Ожидаемый результат:** Открывается страница https://www.saucedemo.com/inventory.html

---

### 🐞 Баг-репорт 

#### BUG_001: одинаковые изображения у problem_user

**Окружение:**
- OS: Windows 10
- Браузер: Google Chrome, версия: 148.0.7778.218

**Priority:** Medium  
**Severity:** Major  

**Заголовок:** Главная страница: У пользователя problem_user у всех товаров одинаковые изображения

**Шаги воспроизведения:**
1. Зайти на https://www.saucedemo.com/
2. Войти под логином problem_user / пароль secret_sauce
3. Посмотреть на изображения товаров

**Фактический результат:** У всех товаров изображение с собакой (одинаковое)

**Ожидаемый результат:** У каждого товара свое уникальное изображение

**Логи (Console DevTools):**
- `inventory.html:1 Failed to load resource: the server responded with a status of 404 ()`

**Скриншоты:**

![problem_user](/screenshots/BUG_001_problem_user.JPG)

![standard_user](/screenshots/BUG_001_standard_user.JPG)


## 2. 🗄️ Работа с SQL (FTB)

**Запрос 1 (базовый):** Рейсы дороже 1000

```sql
SELECT 
    f.flight_number, 
    adp.city AS departure_city, 
    ads.city AS destination_city, 
    a.manufacturer, 
    a.model
FROM flight f
JOIN airport adp ON f.departure_airport_airport_id = adp.airport_id
JOIN airport ads ON f.destination_airport_airport_id = ads.airport_id
JOIN aircraft a ON a.aircraft_id = f.aircraft_aircraft_id
WHERE f.flight_charge > 1000
ORDER BY f.flight_charge DESC;
```

**Запрос 2 (агрегация):** Модели самолётов со средней ценой > 1500

```sql
SELECT 
    a.model, 
    AVG(f.flight_charge) AS avg_charge, 
    COUNT(f.flight_id) AS total_flights
FROM flight f 
JOIN aircraft a ON f.aircraft_aircraft_id = a.aircraft_id
GROUP BY a.model
HAVING AVG(f.flight_charge) > 1500
ORDER BY avg_charge DESC;
```

## 3. 📬 Тестирование API (JSONPlaceholder)

Тестируемое API: [JSONPlaceholder](https://jsonplaceholder.typicode.com/)

### GET /posts
- **URL:** https://jsonplaceholder.typicode.com/posts
- **Ожидаемый статус:** 200 OK
- **Фактический статус:** 200 ✅

![GET-запрос](./screenshots/Postman_GET.JPG)

### POST /posts
- **URL:** https://jsonplaceholder.typicode.com/posts
- **Тело запроса:**

  ```json
{
    "title": "Тестирование API: проверка создания задачи",
    "body": "Задача №2: создание новой записи",
    "userId": 7
}
  ```

- **Ожидаемый статус:** 201 Created
- **Фактический статус:** 201 ✅

![POST-запрос](./screenshots/Postman_POST.JPG)
