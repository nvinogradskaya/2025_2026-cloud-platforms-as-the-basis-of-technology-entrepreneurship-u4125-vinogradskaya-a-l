University: ITMO University Faculty: FICT Course: [Облачные технологии] 
Year: 2025/2026 
Group: U4125 
Author: Vinogradskaya Anastasia Leonidovna 
Lab: Lab2 
Date of create: 05.05.2026 
Date of finished: 05.05.2026


Лабораторная работа №2


---

### 1. Создание Cloud Run сервиса

В сервисе Google Cloud Console создан новый Cloud Run сервис на основе стандартного контейнера Hello.

Использован образ:
us-docker.pkg.dev/cloudrun/container/hello

Имя сервиса:
nvin-cloudrun-lab2

Регион развертывания:
europe-west1
<img width="1178" height="964" alt="image" src="https://github.com/user-attachments/assets/773d2592-7a51-46d6-aed8-0a65d82a1675" />


---

### 2. Проверка работы сервиса

После развертывания выполнен переход по публичному URL сервиса

Сервис корректно возвращал ответ
<img width="2472" height="1442" alt="image" src="https://github.com/user-attachments/assets/77232479-834f-4c2f-a601-f53e82e76040" />

---

### логи

Были изучены логи - они подтверждают корректный запуск контейнера и обработку запросов:
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/6f5d7a4c-c692-40e0-8d83-8bd6552ad424" />

* Контейнер успешно инициализирован
* Сервис начал прослушивание HTTP-запросов
* Startup probe выполнен успешно
* Все HTTP-запросы завершены со статусом 200

Средняя задержка обработки запросов составила 2–7 мс, что указывает на высокую производительность при минимальной нагрузке

---

### метрики

И изучены метрики
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/1ddc2504-ec16-4391-a1cb-c0de8f07f125" />
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/bbd55e3e-1ef1-4c51-80de-301ae171c356" />


* количество запросов стабильно и низкое
* latency находится в диапазоне нескольких миллисекунд
* использование CPU и памяти минимальное
* количество активных инстансов равно 1

---

### 3. Изменение порта на 8090

Создана новая ревизия сервиса с изменением порта контейнера на 8090
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/4a6b644c-fab4-4db8-9602-2dcf8c5b585c" />

После деплоя сервис остался доступен и продолжил обрабатывать запросы
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/bbbe42ce-c812-45cc-8a2f-be27de7efc70" />
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/34ea4d35-0c45-408c-a6e0-b1c87adeb9a4" />

---

### логи на 8090
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/c8accc3f-d418-4ef6-a1df-ccf919b6b29f" />

Логи на 8090

* успешный запуск контейнера на порту 8090
* успешное прохождение startup probe
* автоматическое создание новой ревизии
* обработку HTTP-запросов без ошибок

Дополнительно зафиксирован запуск новых инстансов по причине autoscaling

---

Изменение порта не привело к нарушению работы сервиса
Контейнер корректно обрабатывает заданный порт, переданный через конфигурацию Cloud Run
Сервис автоматически адаптируется к новой ревизии (8090) без остановки работы

---

### 4. Переключение трафика между ревизиями

В Cloud Run создано две ревизии сервиса:

* ревизия 1 с конфигурацией порта 8080
* ревизия 2 с конфигурацией порта 8090
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/3dbd3c33-58f0-4c13-b91a-778c8976cbf9" />
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/db77071e-5428-4bcb-a53d-e9351db58aab" />

Выполнено распределение трафика между ревизиями в пропорции 50/50, а также тестовое переключение 100% трафика между версиями.
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/6e8e842a-0285-4758-98ba-dcdb7dc8bc09" />
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/d5e149d8-051e-45de-92a0-9a026653a1a7" />
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/68242ae6-4c42-4b62-b3fd-6b2728cd9f78" />

---

### логи
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/423f7aa7-df57-47e0-8a45-bc15cde438b9" />

видим 
* одновременную работу обеих ревизий
* успешное прохождение startup probe для обеих версий
* автоматическое масштабирование инстансов
* обработку запросов обеими ревизиями без ошибок

Все HTTP-запросы завершались статусом 200


Cloud Run позволяет направлять трафик на несколько ревизий одновременно и переключение между версиями происходит без изменения URL и без остановки сервиса

---

### 5. Удаление ресурсов

После завершения работы удалены все созданные ресурсы

* автоматическое управление инфраструктурой
* отсутствие простоя при обновлениях
* гибкое управление версиями приложения
* стабильную работу при изменении конфигурации

Сервис демонстрирует подход serverless для контейнерных приложений с высокой степенью автоматизации и отказоустойчивости.
