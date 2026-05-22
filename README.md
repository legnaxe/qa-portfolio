Чек-лист: авторизация на SauceDemo
№	Проверка	Логин	Пароль	Ожидание	Результат
1	Успешный вход (standard_user)	standard_user	secret_sauce	редирект на /inventory.html	ОК
2	Успешный вход (problem_user)	problem_user	secret_sauce	редирект на /inventory.html	ОК
3	Успешный вход (performance_glitch_user)	performance_glitch_user	secret_sauce	редирект на /inventory.html	ОК
4	Пустой логин	(пусто)	secret_sauce	Epic sadface: Username is required	ОК
5	Пустой пароль	standard_user	(пусто)	Epic sadface: Password is required	ОК
6	Оба поля пустые	(пусто)	(пусто)	Epic sadface: Username is required	ОК
7	Неверный логин	fake_user	secret_sauce	Epic sadface: Username and password do not match any user in this service	ОК
8	Неверный пароль	standard_user	fake_pass	Epic sadface: Username and password do not match any user in this service	ОК

Тест-кейсы: авторизация на SauceDemo

**TC-01 – Успешная авторизация с корректным пользователем**
**Предусловия:** открыта страница https://www.saucedemo.com/
**Шаги:**
1. В поле Username ввести `standard_user`
2. В поле Password ввести `secret_sauce`
3. Нажать кнопку Login
**Ожидаемый результат:** Открывается страница https://www.saucedemo.com/inventory.html

---

**TC-02 – Пустой логин**

**Предусловия:** открыта страница https://www.saucedemo.com/

**Шаги:**
1. Поле Username оставить пустым
2. В поле Password ввести `secret_sauce`
3. Нажать кнопку Login

**Ожидаемый результат:** Появляется сообщение об ошибке `Epic sadface: Username is required`

---

**TC-03 – Неверный пароль**

**Предусловия:** открыта страница https://www.saucedemo.com/

**Шаги:**
1. В поле Username ввести `standard_user`
2. В поле Password ввести `fake_pass`
3. Нажать кнопку Login

**Ожидаемый результат:** Появляется сообщение об ошибке `Epic sadface: Username and password do not match any user in this service`



**Баг-репорт**

**Заголовок:** Главная страница: У пользователя problem_user у всех товаров одинаковые картинки

**Шаги воспроизведения:**
1. Зайти на https://www.saucedemo.com/
2. Войти под логином problem_user / пароль secret_sauce
3. Посмотреть на картинки товаров

**Фактический результат:** У всех товаров картинка с собакой (одинаковая)

**Ожидаемый результат:** У каждого товара своя уникальная картинка

**Скриншоты:** ![standard_user](./screenshots/BAG_001_standard_user.jpg)
![problem_user](./screenshots/BAG_001_problem_user.jpg)


**Логи (Console DevTools):**
- `inventory.html:1 Failed to load resource: the server responded with a status of 404 ()`
