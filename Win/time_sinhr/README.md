# ⏱️ TimeSyncLogger — Синхронизация времени с логированием для серверов Windows

Простой скрипт, который синхронизирует текущее системное время с сервером (в моём случае это NTP) и записывает информацию о синхронизации в лог-файл.

---

## 🎯 Описание

Этот проект предназначен для регулярной синхронизации системного времени через NTP (Network Time Protocol) с последующей записью результатов в логи системы. Может быть полезен в автоматизированных системах, где важна точность времени и требуется аудит изменений.

---

## 🛠️ Функционал

- Получение точного времени с NTP-сервера  
- Обновление системного времени (опционально)  
- Логирование даты, времени и результата синхронизации  

---

## 🧩 Технологии

### 1) Изменение конфигов для w32time

1.1 - Открываем PowerShell и пишем следующие команды:
-   `net stop w32time` - останавливаем службу
-   `w32tm /config /syncfromflags:manual /manualpeerlist:"IP-NTP_сервера"` - прописываем в реест NTP-сервер
-   `net start w32time` - запускам службу
-   `w32tm /config /update` - применяем и сохроняем конфигурацию

### 2) Создаем файл .bat и сохраняем в удобном нам месте

### 3) Пишем в него скрипт:
-   `net stop w32time && net start w32time`
-   `w32tm /resync`

## 🕒 Автоматизация

Автоматизировать всё это можно через Планировщик заданий, это уже встроенный инструмент ОС

### 1. Нажми Win + S , введи Планировщик заданий  и открой его
 
### 2. В левой панели выбери Библиотека планировщика заданий

### 3. В правой панели нажми Создать задачу...

### 📋 4. Настройка задачи

4.1 Имя задачи : Например, Sync System Time
    Отметь Выполнять с наивысшими правами 
    Выбери Независимо от того, вошёл ли пользователь в систему, я использую "LOCAL SERVICE"

4.2 Триггеры:
    Нажми Создать 
    Начать задачу : При выходе в систему / При запуске компьютера / Ежедневно — зависит от твоих целей
    Тип триггера : Выбери Ежечасно  или Повторяющаяся задача 

    Если повторяющаяся:
        Установи интервал, например, каждые 15 минут 
        Длительность: Бессрочно 
    Нажми ОК 

4.3 Действия:
    Нажми Создать 
    Действие : Запуск программы
    Программа или сценарий : "указываем наш путь к .bat"
    Начать выполнение в : оставь как есть
    Нажми ОК

4.4 Условия  (необязательно)  
    Можно снять галочку Запускать задачу только при подключении к электросети 
    Также можно разрешить запуск при отсутствии интернета, если это не критично для скрипта
     

4.5 Параметры  (необязательно)  
    Отметь Перезапуск каждые 1 минуту , если нужно 

### ✅5. Сохранение и тестирование 
    Нажми ОК , чтобы сохранить задачу
    Перейди в раздел Активные задачи , найди свою
    Кликни правой кнопкой → Выполнить 
     

👉 Теперь ты можешь проверить, появился ли лог, лог появляется в "Просмотр событий"-/Журнал windows/Система 

---

## 💩 Нюансы

1) Если у вас на работе есть службы "Кибер Безопасности" их политики могут блокировать ваш батник, зарание предупредите коллег из этой службы, что его не нужно блокировать.




# 🕋 ИТОГ

-   Я описал самый простой способ синхронизации времени с логированием и с архивацией наших логов, да винда архивирует сами наши логи спустя месяц.

-   Из своего опыта могу сказать, что есть способы через скрипты python/powershall, что влечет за собой гибкость и скорость и кастомную настройку логирования и синхронизации времени, но не везде их можно реализивать, все зависит от компании в которой вы работаете.



## 📫 Связь 

Если есть вопросы или предложения — пишите:
📧 vlad.zhukov.7878@mail.ru 
