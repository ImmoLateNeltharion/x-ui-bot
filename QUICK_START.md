# Быстрый старт на сервере

## Ваши данные для подключения:

- **Токен бота**: `7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA`
- **Адрес панели x-ui**: `http://103.113.71.160:41280` (без /panel/)
- **Логин**: `dd`
- **Пароль**: `1I6MC7qv`

## Способ 1: Быстрый запуск (для тестирования)

На сервере выполните:

```bash
# 1. Создайте файл config.py из примера
cp config.py.example config.py

# 2. Установите зависимости
pip3 install -r requirements.txt

# 3. Установите переменные окружения (если еще не установлены)
export TELEGRAM_BOT_TOKEN="7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA"
export XUI_BASE_URL="http://103.113.71.160:41280"
export XUI_USERNAME="dd"
export XUI_PASSWORD="1I6MC7qv"

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
Environment="TELEGRAM_BOT_TOKEN=7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA"
Environment="XUI_BASE_URL=http://103.113.71.160:41280"
Environment="XUI_USERNAME=dd"
Environment="XUI_PASSWORD=1I6MC7qv"
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

1. **Адрес панели**: Используется `http://103.113.71.160:41280` (без `/panel/`), так как API работает на корневом пути.

2. **Проверка подключения**: Перед запуском убедитесь, что сервер может подключиться к x-ui:
   ```bash
   curl http://103.113.71.160:41280/login
   ```

3. **Безопасность**: Файлы с секретами (`.service`, `start_bot.sh`) не должны попадать в git (уже в `.gitignore`).

4. **Логи**: При проблемах проверьте логи через `journalctl` или в консоли.

