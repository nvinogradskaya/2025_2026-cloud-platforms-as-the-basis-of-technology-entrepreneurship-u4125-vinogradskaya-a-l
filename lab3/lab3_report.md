University: ITMO University Faculty: FICT Course: [Облачные технологии] Year: 2025/2026 Group: U4125 Author: Vinogradskaya Anastasia Leonidovna Lab: Lab1 Date of create: 05.05.2026 Date of finished: 05.05.2026



# Лабораторная работа №3

**Исследование Cloud Storage**

---

## 1. Проект

Работа выполнялась в существующем проекте Google Cloud Console

---

## 2. Создание Cloud Storage bucket

В разделе Cloud Storage был создан новый bucket.
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/d1fc4706-b099-4dbc-a777-d588a0894b00" />
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/f0880c88-c32c-44c5-b781-87b711ed30b1" />

### Параметры:

* имя бакета
* регион: europe-west1
* тип хранилища: Standard
* модель доступа: Uniform access

---

## 3. Загрузка файлов в бакет

В созданный бакет были загружены 3 изображения:

* image1.jpg
* image2.jpg
* image3.jpg
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/a298b90c-4866-4171-be52-0d2f20972b8d" />

---

## 4. Создание папки

Внутри бакета была создана папка:

```
images/
```
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/03db3a74-e828-4bb1-ab8e-0780809e4c37" />

Все загруженные файлы были перемещены в данную директорию:

* image1.jpg → images/image1.jpg
* image2.jpg → images/image2.jpg
* image3.jpg → images/image3.jpg

---

## 5. Настройка доступа к объектам

Была предпринята попытка настройки публичного доступа к объектам через IAM.

### Результат:

В бакете включена политика Public Access Prevention, которая запрещает использование принципала allUsers
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/58e5815a-93cc-43c3-ad1e-22e5733269c0" />


---

## 6. Получение ссылок на объекты

Для файлов были получены ссылки через контекстное меню (Copy URL / Copy public URL)
<img width="2498" height="1280" alt="image" src="https://github.com/user-attachments/assets/cd03d2c3-87e8-4a91-a580-38d57c52569c" />

Это скрин который я загружала (скрин с отчета прошлых лаб) - все успешно
<img width="2766" height="1592" alt="image" src="https://github.com/user-attachments/assets/cc0f4ad4-1c8d-4f5f-8629-efc4f660fc6b" />


---

## 7. Удаление ресурсов
все ресурсы удалены

