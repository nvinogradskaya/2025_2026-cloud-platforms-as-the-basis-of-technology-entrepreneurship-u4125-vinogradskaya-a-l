University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Облачные технологии]
Year: 2025/2026
Group: U4125
Author: Vinogradskaya Anastasia Leonidovna
Lab: Lab1
Date of create: 05.05.2026
Date of finished: 05.05.2026


Лабораторная работа №1
Обзор Google Cloud и исследование основных сервисов

---

## Ход работы

### 1. Получение доступа
Получен доступ к проекту в Google Cloud Console

---

### 2. Создание Service Account

Открыт раздел IAM & Admin → Service Accounts
Создан service account с именем nvinogradskaya-sa-lab1
<img width="605" height="94" alt="image" src="https://github.com/user-attachments/assets/9f38b8ce-c33f-4336-8df9-4e3dc060ce26" />

Назначена роль Storage Admin

---

### 3. Создание виртуальной машины

Открыт раздел Compute Engine → VM instances
Создана виртуальная машина с именем nvin-vm-lab1
Выбран тип машины e2-micro
Включен режим Spot
<img width="2690" height="1278" alt="image" src="https://github.com/user-attachments/assets/e131db15-8123-457d-9eb7-fe7d603f3b6a" />
<img width="2690" height="1278" alt="image" src="https://github.com/user-attachments/assets/abb32ca0-3a49-410e-8f30-81afb7e08270" />


---

### 4. Подключение к виртуальной машине

Выполнено подключение к виртуальной машине через SSH из браузера
<img width="2690" height="1278" alt="image" src="https://github.com/user-attachments/assets/ce809625-860e-4622-9f09-1bd9975e9320" />

---

### 5. Работа с Cloud Storage

С использованием gcloud выполнен просмотр доступных бакетов
Обнаружен бакет lab1-bucket-itmo

Создана локальная директория lab1 на виртуальной машине
Выполнено копирование файлов из бакета с помощью команды gcloud storage cp
Скопированы три файла в локальную директорию
<img width="2690" height="1278" alt="image" src="https://github.com/user-attachments/assets/ca602eb4-2f1d-4c28-bce0-cf232521f302" />

---

### 6. Проверка результатов

С помощью команды ls -lah подтверждено наличие файлов в директории виртуальной машины
<img width="2690" height="1278" alt="image" src="https://github.com/user-attachments/assets/0d31615a-7802-40f4-9eda-56641a211fad" />

---

### 7. Изменение прав доступа

В разделе IAM изменена роль service account
Роль Storage Admin заменена на Compute Viewer
<img width="2690" height="1278" alt="image" src="https://github.com/user-attachments/assets/781abbfc-0570-41c8-8cf0-d03a189c89f7" />

---

### 8. Проверка доступа после изменения роли

Повторно выполнена команда копирования файлов из бакета
Получена ошибка доступа 
<img width="2020" height="188" alt="image" src="https://github.com/user-attachments/assets/5a2eba35-2913-4edd-8a52-5ddada9581f3" />

роль Storage Admin предоставляет полный доступ к Cloud Storage, включая возможность чтения и копирования файлов,а роль Compute Viewer не предоставляет доступ к Cloud Storage - используется сситема привелегий

---

### 9. Удаление ресурсов

Удалена виртуальная машина 
Удален service account 


Подготовлен отчет по лабораторной работе с описанием всех этапов выполнения и приложенными скриншотами
