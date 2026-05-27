# Портфолио по тестированию
**Меня зовут Наталья.** Я начинающий тестировщик.  
Представляю свои учебные проекты для демонстрации навыков.

## 📌 Оглавление
1. Проект: SauceDemo (функциональное тестирование)
2. Работа с SQL
3. Тестирование API (Postman)

## 1. 🧪 Проект: SauceDemo — интернет-магазин (учебный)

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

### 🐞 Найденные баги

#### BUG_001: одинаковые изображения у problem_user

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

![standard_user](/screenshots/BUG_001_standard_user.JPG)

![problem_user](/screenshots/BUG_001_problem_user.JPG)

## 2. 🗄️ Работа с SQL (базы данных)

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

## 3. 📬 Тестирование API (Postman)

Тестируемое API: [JSONPlaceholder](https://jsonplaceholder.typicode.com/)

**Что проверено:** статус-код 200 (GET) и 201 (POST), структура ответа соответствует документации.

**GET-запрос** — получение списка постов:

![GET-запрос](./screenshots/Postman_GET.JPG)

**POST-запрос** — создание нового поста:

![POST-запрос](./screenshots/Postman_POST.JPG)

