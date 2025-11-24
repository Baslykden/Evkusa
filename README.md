# EVKusa Presentation Bot

Telegram-бот, который по Excel-файлу с мастер-меню и изображению фона генерирует PowerPoint-презентацию с блюдам и категориями.

## Возможности

- Команда `/evkusa` запускает сценарий подготовки презентации.
- Бот просит прислать:
  1. Excel-файл с мастер-меню (`.xlsx` или `.xlsm`)
  2. Изображение фона (любое изображение, будет использовано как фон слайдов)
- После получения обоих файлов бот:
  - показывает статус: «Идет подготовка презентации...✨»
  - генерирует презентацию `КП <Название_из_B3>.pptx`
  - отправляет сообщение «✨ Презентация готова!»
  - присылает готовый `.pptx` в чат
  - очищает рабочую папку `work/`

Без команды `/evkusa` бот файлы не ожидает и презентацию не формирует.

---

## Требования

- Python 3.8+ (рекомендовано)
- Установленные:
  - `git`
  - `virtualenv` (или модуль `venv`)
  - `supervisor` (для автозапуска бота как сервиса)
- Telegram Bot API токен (от @BotFather)

---

## Установка

### 1. Клонирование репозитория

```
cd /opt
git clone https://github.com/Baslykden/Evkusa.git
cd Evkusa
```

### 2. Виртуальное окружение и зависимости
```
python3 -m venv venv
source venv/bin/activate

pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Настройка config.py

Откройте файл `config.py` и:

1. Вставьте токен бота:

```
BOT_TOKEN = "СЮДА"
```
### 4. Создание папки для временных файлов

```
mkdir -p work
```

### 5. Тестовый запуск бота вручную

Из корня проекта:
```python
source venv/bin/activate
python ev_bot.py
```

Если бот запустился без ошибок, переходим к настройке автозапуска через supervisor

### 6. Запуск под supervisor
## 1. Конфиг supervisor

Создайте файл, например: /etc/supervisor/conf.d/ev_bot.conf:
```
[program:Evkusa_bot]
directory=/opt/Evkusa
command=/opt/Evkusa/venv/bin/python ev_bot.py
autostart=true
autorestart=true
stderr_logfile=/var/log/ev_bot.err.log
stdout_logfile=/var/log/ev_bot.out.log
user=root
stopsignal=TERM
```

## 2. Применение настроек supervisor

После создания конфига:
```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start ev_bot
```

Проверить статус:
```
sudo supervisorctl status ev_bot
```
### 7. Старт Бота
Найдите бота в Telegram по имени, которое вы указали при создании (у BotFather).

Отправьте команду:
```
/evkusa
```

ГОТОВО!

