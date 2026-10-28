# Стукачёв Павел Сергеевич

📍 Могилёв  
📧 [Pavel91104@mail.ru](mailto:Pavel91104@mail.ru)  
📱 +375 33 393-60-91  
💻 [GitHub: Candymanzz](https://github.com/Candymanzz)  
tg: @htodino

---

## 🎯 Цель

**.NET-разработчик (C# / ASP.NET Core), 1 год коммерческого опыта.** Разрабатываю backend-сервисы на .NET: REST API, микросервисы, интеграции с оборудованием и нативными SDK. Работал с EF Core, PostgreSQL, Docker, JWT и Clean Architecture. Дополнительно имею опыт в компьютерном зрении (OpenCV, Python). Ищу команду, где смогу развивать backend на .NET.

---

## 🛠️ Навыки

- **Backend / .NET:** C#, .NET / .NET Core (до .NET 10), ASP.NET Core, Web API, REST, Kestrel, Swagger / OpenAPI, Dependency Injection, Problem Details, юнит- и интеграционные тесты
- **Данные:** Entity Framework Core (Code First), PostgreSQL, SQL Server, MySQL, MongoDB; LINQ, AutoMapper, FluentValidation
- **Архитектура:** Clean Architecture, CQRS, Repository, Unit of Work, микросервисы, HTTP/REST, ООП, SOLID, паттерны проектирования
- **Безопасность и сервисы:** JWT Bearer, аутентификация и авторизация, email-уведомления (MailKit)
- **DevOps / инструменты:** Docker, docker-compose, Git, GitHub (ветвление, merge, code review в команде из 4 человек)
- **Интеграции:** Hikrobot MVS SDK (`MvCameraControl.Net`), P/Invoke нативных DLL, GenICam, COM / Ethernet, UDP, YAML-конфигурация
- **Компьютерное зрение (дополнительно):** Python, OpenCV, NumPy, FastAPI — пайплайн инспекции (выравнивание, diff, аномалии, FP-фильтры)
- **Другие языки:** Python, TypeScript, JavaScript, C++
- **Английский:** B1

---

## 💼 Опыт

### Defect Detector — промышленная система оптического контроля

Промышленный комплекс инспекции изделий на линии: камеры, вспышки, дискретное IO, PLC, оркестратор и детектор поверхности. Работал в команде из 4 человек: фичи в отдельных ветках, merge, разбор конфликтов, согласование контрактов между сервисами.

**Роль:** .NET-разработчик — backend-сервисы оборудования (ASP.NET Core, REST API), интеграция с нативными SDK; также реализовал алгоритм анализа поверхности (компьютерное зрение).

**Стек:** C#, .NET 10, ASP.NET Core, REST API, Swagger, P/Invoke, UDP, YAML, WinForms, Git; дополнительно Python, OpenCV, FastAPI.

#### Микросервисная архитектура

Система собрана из независимых сервисов, которые оркестратор связывает по HTTP/UDP и конфигу:

- Java-оркестратор — цикл инспекции, запуск воркеров, health
- LightServer (.NET) — вспышки и подсветка
- IoInputMonitor (.NET) — реле / дискретное IO и триггер съёмки
- analisSurface (Python) — алгоритм контроля поверхности
- camera-worker (C++), сервис геометрии, frontend, связь с PLC (FINS)

Понимаю, как такие сервисы делят ответственность, общаются по контрактам API и живут каждый со своим конфигом и жизненным циклом.

#### LightServer — сервис вспышек (ASP.NET Core)

HTTP-сервис управления вспышками Hikrobot MV-LE (COM и Ethernet). Оркестратор вызывает его перед захватом кадра.

- Интеграция с **MvCameraControl.Net**: GenICam-узлы яркости, селектора канала, software trigger
- Синхронная вспышка нескольких COM: изолированный банк устройств, двухфазный протокол prep → barrier → fire
- Маршрутизация «камера → канал», hot-reload YAML, режимы Direct / Hold / Deferred / Broadcast
- REST API и Swagger для оркестратора и отладки

#### IoInputMonitor — сервис реле и дискретного IO

Консольный .NET-сервис для платы Hikrobot MV IO Box: чтение DI и импульсы DO.

- Обёртка нативного SDK через **P/Invoke** (`MvIOInterfaceBox.dll`): open/close, edge callback, без polling
- Логика затвора съёмки: DI2 — armed, DI3 — fire, DI1 — disarm; debounce и refractory
- UDP-публикация фронтов в оркестратор как внешний триггер инспекции
- Разбор документации SDK: официальный COM-протокол, а не «сырой» GPIO

#### Компьютерное зрение: алгоритм анализа поверхности (analisSurface)

Дополнительная часть проекта на Python. Пайплайн «кадр vs эталон» → вердикт ГОДЕН / БРАК для HTTP API, который вызывает оркестратор.

- Выравнивание кадра (ORB + гомография / ECC, опционально матрица из сервиса геометрии)
- Карта отличий, сегментация аномалий, ROI и FP-зоны, heatmap / diff / mask
- Настраиваемые пороги по типу изделия (simple / pro), пересчёт результата при смене настроек
- Сессионное дообучение: оператор помечает ложные срабатывания, сервис запоминает допустимые фрагменты и не считает их браком на следующих кадрах
- Связка с продакшеном через shared memory и REST

#### Документация и SDK

Читал и применял вендорские гайды Hikrobot (камеры, MV-LE, IO Box). Писал внутренние GUIDE по LightServer, IoInputMonitor и детектору: запуск, конфиг, API, типовые сбои железа.

---

## 💻 Личные .NET-проекты

### InnoShop

Веб-приложение для управления пользователями и продуктами: аутентификация, авторизация, фильтрация товаров.

**Технологии:** ASP.NET Core, Entity Framework Core, PostgreSQL, AutoMapper, FluentValidation, MailKit, Docker, JWT Bearer, Swagger.

**Архитектура:** Clean Architecture (Presentation, Application, Domain, Infrastructure), DI, Code First, Repository, Unit of Work, CQRS.

- JWT, подтверждение аккаунта и восстановление пароля по email
- Два микросервиса: пользователи и продукты; SoftDelete продуктов при деактивации пользователя
- Docker / docker-compose, Problem Details, юнит- и интеграционные тесты

[Репозиторий](https://github.com/Candymanzz/Inno_Shop)

### EventWebApp

Веб-приложение для управления событиями и участниками.

**Технологии:** ASP.NET Core, Entity Framework Core, PostgreSQL, AutoMapper, FluentValidation, MailKit, Docker, JWT Bearer, Swagger.

Clean Architecture, CQRS, JWT, email-уведомления, Swagger, Docker, покрытие ключевых сервисов тестами.

[Репозиторий](https://github.com/Candymanzz/EventWebApp)

### Другие .NET-проекты

| Проект | Описание | Стек |
|--------|----------|------|
| Worker | REST API управления задачами сотрудников | C#, .NET Core, EF |
| juridical-api | CRUD API юридической информации | C#, .NET Core, EF |
| Soil | Мониторинг состояния почвы, интеграция с внешними сервисами | C#, .NET Core |
| car-rental-api | API аренды автомобилей, бронирование | C#, .NET Core, EF |
| SchoolDiningApp | Система школьного питания | JavaScript, Node.js |

---

## 🎓 Образование

**Белорусско-Российский университет**  
Программная инженерия (ПИР)

---

## 🌐 Языки

- Русский — родной  
- Английский — технический (чтение документации и SDK, общение в команде)

---

## 📌 Дополнительная информация

- 1 год опыта в .NET-разработке: ASP.NET Core, REST API, интеграции, микросервисы
- Опыт командной разработки в Git: ветки, merge, согласование API между сервисами
- Разбираюсь в технической документации вендоров и пишу внутренние гайды
- Готов развиваться как .NET-разработчик в продуктовой или промышленной команде
