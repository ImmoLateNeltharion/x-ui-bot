# Что делать дальше после клонирования репозитория

## Шаг 1: Перейдите в директорию проекта

```bash
cd x-ui-bot
# или cd путь/к/вашему/проекту
```

## Шаг 2: Создайте файл config.py

Файл `config.py` не находится в git (он в `.gitignore`), поэтому нужно создать его из примера:

```bash
cp config.py.example config.py
```

Этот файл будет использовать переменные окружения, которые вы уже установили.

## Шаг 3: Установите зависимости

```bash
pip3 install -r requirements.txt
```

Если у вас проблемы с правами, используйте:
```bash
pip3 install --user -r requirements.txt
```

Или установите в виртуальное окружение (рекомендуется):
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Шаг 4: Проверьте переменные окружения

Убедитесь, что переменные окружения установлены:

```bash
echo $TELEGRAM_BOT_TOKEN
echo $XUI_BASE_URL
echo $XUI_USERNAME
echo $XUI_PASSWORD
```

Если переменные не отображаются, установите их заново:

```bash
export TELEGRAM_BOT_TOKEN="your_bot_token_here"
export XUI_BASE_URL="http://your-server:port"
export XUI_USERNAME="your_username"
export XUI_PASSWORD="your_password"
```

## Шаг 5: Проверьте подключение к x-ui панели (опционально)

```bash
curl http://your-server:port/login
```

Должен вернуться ответ от API (может быть ошибка, но это нормально - главное, что сервер отвечает).

## Шаг 6: Запустите бота

### Вариант A: Запуск в текущей сессии (для тестирования)

```bash
python3 bot.py
```

Вы должны увидеть сообщение: `Бот запущен...`

### Вариант B: Запуск в фоне через screen

```bash
# Установите screen (если не установлен)
# sudo apt install screen  # для Debian/Ubuntu
# sudo yum install screen  # для CentOS/RHEL

# Создайте сессию
screen -S xui-bot

# Запустите бота
python3 bot.py

# Отключитесь: нажмите Ctrl+A, затем D
# Вернуться: screen -r xui-bot
```

### Вариант C: Запуск через systemd (для постоянной работы)

1. Создайте service файл:
```bash
sudo nano /etc/systemd/system/xui-bot.service
```

2. Вставьте (измените пути при необходимости):
```ini
[Unit]
Description=Telegram Bot for x-ui VPN Configurations
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/x-ui-bot
Environment="TELEGRAM_BOT_TOKEN=your_bot_token_here"
Environment="XUI_BASE_URL=http://your-server:port"
Environment="XUI_USERNAME=your_username"
Environment="XUI_PASSWORD=your_password"
ExecStart=/usr/bin/python3 /root/x-ui-bot/bot.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

3. Запустите сервис:
```bash
sudo systemctl daemon-reload
sudo systemctl enable xui-bot
sudo systemctl start xui-bot
sudo systemctl status xui-bot
```

4. Просмотр логов:
```bash
sudo journalctl -u xui-bot -f
```

## Шаг 7: Протестируйте бота

1. Откройте Telegram и найдите вашего бота по токену
2. Отправьте команду `/start`
3. Проверьте, что бот отвечает

## Возможные проблемы и решения

### Ошибка: "TELEGRAM_BOT_TOKEN не установлен"
- Проверьте, что переменные окружения установлены: `echo $TELEGRAM_BOT_TOKEN`
- Установите их заново в текущей сессии

### Ошибка: "Не удалось авторизоваться в x-ui"
- Проверьте правильность URL, логина и пароля
- Убедитесь, что сервер может подключиться к x-ui панели
- Проверьте, что x-ui панель работает: `curl http://your-server:port/login`

### Ошибка: "ModuleNotFoundError: No module named 'config'"
- Создайте файл `config.py`: `cp config.py.example config.py`
- Убедитесь, что файл существует: `ls -la config.py`

### Ошибка: "ModuleNotFoundError" (другие модули)
- Установите зависимости: `pip3 install -r requirements.txt`
- Убедитесь, что используете правильную версию Python (3.8+)

### Бот не отвечает в Telegram
- Проверьте, что токен правильный
- Убедитесь, что бот запущен (проверьте логи)
- Проверьте интернет-соединение сервера

## Полезные команды

### Проверка статуса бота (если используете systemd):
```bash
sudo systemctl status xui-bot
```

### Перезапуск бота (systemd):
```bash
sudo systemctl restart xui-bot
```

### Остановка бота (systemd):
```bash
sudo systemctl stop xui-bot
```

### Просмотр последних логов:
```bash
sudo journalctl -u xui-bot -n 50
```

### Если бот запущен в screen:
```bash
screen -r xui-bot  # подключиться к сессии
# Внутри screen: Ctrl+C для остановки
```

## Следующие шаги

После того, как бот запущен и работает:

1. **Добавьте администраторов**: Отредактируйте `config.py` или установите через переменные окружения (если поддерживается)
2. **Настройте лимиты**: Используйте админские команды `/setlimit` для установки лимитов пользователям
3. **Проверьте работу**: Протестируйте получение конфигураций через команду `/list` и `/get`

