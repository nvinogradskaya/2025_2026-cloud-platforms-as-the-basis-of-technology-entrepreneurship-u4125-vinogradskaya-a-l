University: ITMO University Faculty: FICT Course: [Облачные технологии]  
Year: 2025/2026   
Group: U4125   
Author: Vinogradskaya Anastasia Leonidovna  
Lab: Lab4  
Date of create: 07.05.2026   
Date of finished: 08.05.2026


# Лабораторная работа №4

## Проектирование инфраструктуры AI SaaS-платформы в Google Cloud


# Описание

Приложение: SaaS-платформа для управления задачами и командной работы с AI-помощником.

Функциональность приложения:

* создание задач;
* управление проектами;
* совместная работа команд;
* загрузка файлов;
* уведомления;
* аналитика активности;
* AI-помощник для генерации описаний задач, суммаризации обсуждений и рекомендаций по приоритетам.

AI будем использовать так:

* генерация описаний задач;
* автоматическая суммаризация комментариев;
* рекомендации по приоритетам задач;
* AI-анализ загруженных текстов.

---

# Функциональные модули

| Модуль                   | Frontend | Backend |
| ------------------------ | -------- | ------- |
| Главная страница         | ✓        | ✓       |
| Авторизация              | ✓        | ✓       |
| Личный кабинет           | ✓        | ✓       |
| Управление задачами      | ✓        | ✓       |
| Командная работа         | ✓        | ✓       |
| Загрузка файлов          | ✓        | ✓       |
| AI-помощник              | —        | ✓       |
| Система уведомлений      | —        | ✓       |
| Административная панель  | ✓        | ✓       |
| Мониторинг и логирование | —        | ✓       |
| Аналитика пользователей  | —        | ✓       |

---

# Нагрузки (примерно)

| Стадия                              | Нагрузка               |
| ----------------------------------- | ---------------------- |
| Стадия 1 — MVP                      | до 100 пользователей   |
| Стадия 2 — Партнерское тестирование | до 5к пользователей |
| Стадия 3 — Production               | 50к+ пользователей  |

---

# Стадия 1 — MVP

## Цель 

Максимально быстро и дешево запустить рабочий прототип продукта для проверки гипотезы и получения первой обратной связи

---

## Архитектурный подход

На этапе MVP используется максимально простая serverless-архитектура:

* frontend и backend работают отдельно;
* используется минимальная managed база данных;
* AI-функции вызываются через API;
* отсутствуют сложные механизмы отказоустойчивости.

хотим минимизировать стоимость и ускорить разработку

---

# Инфраструктура 

| Ресурс               | Конфигурация   | Обоснование                                  |
| -------------------- | -------------- | -------------------------------------------- |
| Cloud Run Frontend   | 1 vCPU, 512 MB | Serverless frontend без постоянной нагрузки  |
| Cloud Run Backend    | 1 vCPU, 512 MB | Backend API с autoscaling от 0               |
| Cloud SQL PostgreSQL | db-f1-micro    | Минимальная managed БД                       |
| Cloud Storage        | ~20 GB         | Хранение файлов пользователей                |
| Vertex AI API        | pay-as-you-go  | AI-функции без собственной ML-инфраструктуры |
| Cloud Logging        | basic          | Сбор логов                                   |
| Cloud Monitoring     | free tier      | Базовые метрики                              |
| Cloud DNS            | 1 зона         | Подключение домена                           |

---

# Схема инфраструктуры — Стадия 1

На схеме должны присутствовать:

* User
* Cloud Run Frontend
* Cloud Run Backend
* Cloud SQL
* Cloud Storage
* Vertex AI API
* Cloud Logging / Monitoring

---

# Экономическая модель — Стадия 1

| Ресурс                | Стоимость/мес |
| --------------------- | ------------- |
| Cloud Run Frontend    | ~$3           |
| Cloud Run Backend     | ~$5           |
| Cloud SQL db-f1-micro | ~$15          |
| Cloud Storage         | ~$2           |
| Vertex AI API         | ~$10          |
| Logging / Monitoring  | ~$3           |
| Итого                 | ~$35–40       |

---

# Обоснование решений — Стадия 1

Cloud Run выбран из-за serverless-модели и autoscaling от 0. При низкой нагрузке сервис практически не потребляет ресурсы и позволяет минимизировать расходы.

Cloud SQL используется вместо самостоятельного PostgreSQL на виртуальной машине, так как managed БД упрощает администрирование, резервное копирование и обновления.

Vertex AI API позволяет использовать AI-функциональность без необходимости разворачивать собственные ML-сервисы.

Cloud Storage используется для хранения файлов пользователей и статических данных.

---

# Стадия 2 — Партнерское тестирование

## Цель стадии

Обеспечить стабильную работу системы при росте нагрузки и подключении партнеров.

На этом этапе критичны:

* отказоустойчивость;
* мониторинг;
* отсутствие cold starts;
* стабильность API.

---

## Архитектурный подход

На стадии тестирования приложение начинает разделяться на отдельные сервисы:

* frontend;
* backend API;
* AI-service;
* notification-service.

Добавляются:

* Redis;
* Load Balancer;
* CI/CD;
* alerting.

---

# Инфраструктура — Стадия 2

| Ресурс                         | Конфигурация     | Обоснование                       |
| ------------------------------ | ---------------- | --------------------------------- |
| Cloud Run (несколько сервисов) | 2–4 vCPU         | Разделение сервисов и autoscaling |
| Cloud SQL PostgreSQL           | db-n1-standard-1 | Более высокая производительность  |
| Read Replica                   | 1 replica        | Снижение нагрузки на чтение       |
| Memorystore Redis              | Basic tier, 1 GB | Кэширование и хранение сессий     |
| Cloud Storage                  | ~100 GB          | Рост файлов пользователей         |
| HTTP Load Balancer             | HTTPS LB         | Централизованная маршрутизация    |
| Artifact Registry              | Docker images    | Хранение контейнеров              |
| Cloud Build                    | CI/CD            | Автоматический деплой             |
| Vertex AI API                  | increased usage  | Рост AI-запросов                  |
| Monitoring + Alerting          | standard         | Контроль стабильности             |

---

# Схема инфраструктуры — Стадия 2

На схеме должны присутствовать:

* Load Balancer
* Frontend Service
* Backend API
* AI Service
* Notification Service
* Redis
* Cloud SQL + Read Replica
* Cloud Storage
* Vertex AI
* Monitoring / Alerting
* Artifact Registry
* Cloud Build

---

# Экономическая модель — Стадия 2

| Ресурс                    | Стоимость/мес |
| ------------------------- | ------------- |
| Cloud Run services        | ~$40          |
| Cloud SQL                 | ~$80          |
| Read Replica              | ~$40          |
| Redis                     | ~$35          |
| Load Balancer             | ~$20          |
| Cloud Storage             | ~$5           |
| Vertex AI API             | ~$40          |
| Monitoring / Logging      | ~$15          |
| Artifact Registry + CI/CD | ~$10          |
| Итого                     | ~$280–320     |

---

# Обоснование решений — Стадия 2

Cloud Run продолжает использоваться, так как нагрузка еще недостаточно высокая для полноценного Kubernetes-кластера.

Redis добавляется для:

* кэширования;
* хранения сессий;
* уменьшения нагрузки на БД.

Load Balancer необходим для:

* HTTPS;
* распределения трафика;
* централизованной точки входа.

Read Replica позволяет снизить нагрузку на основную БД при росте количества запросов на чтение.

---

# Стадия 3 — Production

## Цель стадии

Обеспечить:

* высокую доступность;
* масштабируемость;
* безопасность;
* SLA;
* стабильную работу AI-компонентов при высокой нагрузке.

---

## Архитектурный подход

На production-этапе backend переходит в Kubernetes-инфраструктуру.

Frontend остается на Cloud Run, так как:

* stateless;
* хорошо масштабируется;
* дешевле в обслуживании.

Backend переносится в GKE Autopilot:

* микросервисная архитектура;
* гибкое масштабирование;
* более эффективное использование ресурсов.

---

# Инфраструктура — Стадия 3

| Ресурс                       | Конфигурация          | Обоснование                |
| ---------------------------- | --------------------- | -------------------------- |
| Cloud Run Frontend           | autoscaling           | Быстрая отдача frontend    |
| GKE Autopilot                | multi-service cluster | Production backend         |
| Cloud SQL HA                 | High Availability     | Отказоустойчивая БД        |
| Read Replicas                | 2 replicas            | Масштабирование чтения     |
| Memorystore Redis HA         | Standard tier         | Высокодоступный кэш        |
| Cloud Storage Multi-region   | 1 TB                  | Надежное хранение файлов   |
| Cloud CDN                    | Global CDN            | Ускорение статики          |
| HTTPS Load Balancer          | global                | Балансировка трафика       |
| Cloud Armor                  | WAF + DDoS protection | Защита production          |
| Secret Manager               | secrets               | Безопасное хранение ключей |
| Vertex AI Endpoints          | managed inference     | Production AI              |
| Pub/Sub                      | async events          | Асинхронные задачи         |
| Monitoring + Trace + Logging | premium               | Полный observability stack |
| BigQuery                     | analytics             | Аналитика данных           |
| Looker Studio                | dashboards            | BI-аналитика               |

---

# Схема инфраструктуры — Стадия 3

На схеме должны присутствовать:

* Users
* Cloud CDN
* HTTPS Load Balancer
* Cloud Armor
* Cloud Run Frontend
* GKE Cluster
* AI Service
* Pub/Sub
* Redis HA
* Cloud SQL HA + replicas
* Cloud Storage
* Vertex AI Endpoints
* BigQuery
* Monitoring / Logging / Trace
* Secret Manager

---

# Экономическая модель — Стадия 3

| Ресурс               | Стоимость/мес |
| -------------------- | ------------- |
| GKE Autopilot        | ~$300–450     |
| Cloud SQL HA         | ~$300         |
| Read Replicas        | ~$120         |
| Redis HA             | ~$100         |
| Load Balancer + CDN  | ~$50          |
| Cloud Storage        | ~$20          |
| Vertex AI Endpoints  | ~$250         |
| Monitoring / Logging | ~$50          |
| Cloud Armor          | ~$30          |
| BigQuery + Looker    | ~$40          |
| Pub/Sub              | ~$15          |
| Итого                | ~$1300–1500   |

---

# Сравнение стадий

| Параметр     | MVP                | Партнеры            | Production                   |
| ------------ | ------------------ | ------------------- | ---------------------------- |
| Пользователи | до 100             | до 5 000            | 50 000+                      |
| Архитектура  | Простая serverless | Микросервисы        | Kubernetes                   |
| Backend      | Cloud Run          | Cloud Run           | GKE                          |
| База данных  | Cloud SQL micro    | Cloud SQL + replica | Cloud SQL HA                 |
| Кэш          | —                  | Redis               | Redis HA                     |
| AI           | Vertex AI API      | Vertex AI API       | Vertex AI Endpoints          |
| CDN          | —                  | —                   | Cloud CDN                    |
| Безопасность | базовая            | средняя             | Cloud Armor + Secret Manager |
| Мониторинг   | basic              | alerting            | full observability           |
| Стоимость    | ~$40               | ~$300               | ~$1500                       |

---

# выводы

В ходе работы была спроектирована инфраструктура SaaS-платформы с AI-функциональностью для трех стадий развития продукта.

Была показана эволюция инфраструктуры

По мере роста нагрузки менялись требования:

* к масштабируемости;
* отказоустойчивости;
* безопасности;
* мониторингу;
* AI-инфраструктуре
