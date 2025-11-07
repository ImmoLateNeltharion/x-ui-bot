# Быстрый старт на сервере

## Ваши данные для подключения:

Установите переменные окружения на сервере:
- **Токен бота**: `TELEGRAM_BOT_TOKEN`
- **Адрес панели x-ui**: `XUI_BASE_URL` (без /panel/)
- **Логин**: `XUI_USERNAME`
- **Пароль**: `XUI_PASSWORD`

## Способ 1: Быстрый запуск (для тестирования)

На сервере выполните:

```bash
# 1. Создайте файл config.py из примера
cp config.py.example config.py

# 2. Установите зависимости
pip3 install -r requirements.txt

# 3. Установите переменные окружения (если еще не установлены)
export TELEGRAM_BOT_TOKEN="your_bot_token_here"
export XUI_BASE_URL="http://your-server:port"
export XUI_USERNAME="your_username"
export XUI_PASSWORD="your_password"

# 4. Запустите бота
python3 bot.py
```

## Способ 2: Постоянный запуск через systemd

### 1. Создайте service файл

```bash
sudo nano /etc/systemd/system/xui-bot.service
```

### 2. Вставьте следующее содержимое (измените пути при необходимости):

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

### 3. Запустите сервис

```bash
sudo systemctl daemon-reload
sudo systemctl enable xui-bot
sudo systemctl start xui-bot
sudo systemctl status xui-bot
```

### 4. Просмотр логов

```bash
sudo journalctl -u xui-bot -f
```

## Способ 3: Использование скрипта start_bot.sh

На сервере:

```bash
# 1. Сделайте скрипт исполняемым
chmod +x start_bot.sh

# 2. Запустите
./start_bot.sh
```

## Важные замечания

1. **Адрес панели**: Используйте адрес без `/panel/` в конце, так как API работает на корневом пути.

2. **Проверка подключения**: Перед запуском убедитесь, что сервер может подключиться к x-ui:
   ```bash
   curl http://your-server:port/login
   ```

3. **Безопасность**: Файлы с секретами (`.service`, `start_bot.sh`) не должны попадать в git (уже в `.gitignore`).

4. **Логи**: При проблемах проверьте логи через `journalctl` или в консоли.

