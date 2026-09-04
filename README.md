# Стукачёв Павел Сергеевич

📍 Могилёв  
📧 [Pavel91104@mail.ru](mailto:Pavel91104@mail.ru)  
📱 +375 33 393-60-91  
💻 [GitHub: Candymanzz](https://github.com/Candymanzz)  
tg: @htodino

---

## 🎯 Цель

.NET-разработчик с опытом промышленных сервисов и компьютерного зрения. Ищу команду, где можно развивать backend на C# / ASP.NET и доводить сложные интеграции до стабильной работы.

---

## 🛠️ Навыки

- **Основной стек:** C#, .NET / .NET Core, ASP.NET Core, Kestrel, Swagger, YAML-конфигурация, юнит-тесты
- **Интеграции и железо:** Hikrobot MVS SDK (`MvCameraControl.Net`), P/Invoke нативных DLL, GenICam, COM / Ethernet, DI/DO, UDP
- **Компьютерное зрение:** OpenCV, NumPy, FastAPI — пайплайн инспекции (выравнивание, diff, аномалии, FP-фильтры)
- **Архитектура:** микросервисы, HTTP/REST, клиент-сервер, Clean Architecture, DI, CQRS, Repository, Unit of Work
- **Базы данных:** SQL Server, PostgreSQL, MySQL, MongoDB; Entity Framework Core
- **Другие языки:** Python, TypeScript, JavaScript, C++
- **Инструменты:** Git, GitHub (ветвление, merge, code review в команде из 4 человек)
- **Принципы:** ООП, SOLID, паттерны проектирования
- **Английский:** B1

---

## 💼 Опыт

### Defect Detector — промышленная система оптического контроля

Промышленный комплекс инспекции изделий на линии: камеры, вспышки, дискретное IO, PLC, оркестратор и детектор поверхности. Работал в команде из 4 человек: фичи в отдельных ветках, merge, разбор конфликтов, согласование контрактов между сервисами.

**Роль:** .NET-сервисы оборудования, алгоритм анализа поверхности, работа с вендорским SDK и документацией.

**Стек:** C#, ASP.NET Core, .NET 10, WinForms, Python, OpenCV, FastAPI, Hikrobot MVS SDK, YAML, Swagger, UDP, Git.

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

#### Алгоритм анализа поверхности (analisSurface)

Пайплайн «кадр vs эталон» → вердикт ГОДЕН / БРАК для HTTP API, который вызывает оркестратор.

- Выравнивание кадра (ORB + гомография / ECC, опционально матрица из сервиса геометрии)
- Карта отличий, сегментация аномалий, ROI и FP-зоны, heatmap / diff / mask
- Настраиваемые пороги по типу изделия (simple / pro), пересчёт результата при смене настроек
- Сессионное дообучение: оператор помечает ложные срабатывания, сервис запоминает допустимые фрагменты и не считает их браком на следующих кадрах
- Связка с продакшеном через shared memory и REST

#### Документация и SDK

Читал и применял вендорские гайды Hikrobot (камеры, MV-LE, IO Box). Писал внутренние GUIDE по LightServer, IoInputMonitor и детектору: запуск, конфиг, API, типовые сбои железа.

---

## 💻 Учебные и личные проекты

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

- Опыт командной разработки в Git: ветки, merge, согласование API между сервисами
- Разбираюсь в технической документации вендоров и пишу внутренние гайды
- Готов развиваться как .NET-разработчик в продуктовой или промышленной команде
