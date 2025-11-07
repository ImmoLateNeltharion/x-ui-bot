# Инструкция по развертыванию на сервере

## Способ 1: Использование переменных окружения напрямую

### 1. Установите зависимости

```bash
pip3 install -r requirements.txt
```

### 2. Установите переменные окружения

Вы можете установить переменные окружения несколькими способами:

#### Вариант A: В текущей сессии (временный)

```bash
export TELEGRAM_BOT_TOKEN="7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA"
export XUI_BASE_URL="http://103.113.71.160:41280"
export XUI_USERNAME="dd"
export XUI_PASSWORD="1I6MC7qv"
```

#### Вариант B: В файле ~/.bashrc или ~/.profile (постоянный для пользователя)

```bash
echo 'export TELEGRAM_BOT_TOKEN="7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA"' >> ~/.bashrc
echo 'export XUI_BASE_URL="http://103.113.71.160:41280"' >> ~/.bashrc
echo 'export XUI_USERNAME="dd"' >> ~/.bashrc
echo 'export XUI_PASSWORD="1I6MC7qv"' >> ~/.bashrc
source ~/.bashrc
```

#### Вариант C: Использование скрипта запуска

Используйте файл `start_bot.sh`:

```bash
chmod +x start_bot.sh
./start_bot.sh
```

### 3. Запуск бота

```bash
python3 bot.py
```

## Способ 2: Использование systemd (рекомендуется для продакшена)

### 1. Скопируйте service файл

```bash
sudo cp xui-bot.service /etc/systemd/system/
```

### 2. Обновите пути в service файле

Отредактируйте `/etc/systemd/system/xui-bot.service` и измените:
- `WorkingDirectory` - путь к директории с ботом
- `ExecStart` - путь к python3 и bot.py
- `User` - пользователь для запуска (обычно root или ваш пользователь)

### 3. Перезагрузите systemd и запустите сервис

```bash
sudo systemctl daemon-reload
sudo systemctl enable xui-bot
sudo systemctl start xui-bot
```

### 4. Проверьте статус

```bash
sudo systemctl status xui-bot
```

### 5. Просмотр логов

```bash
sudo journalctl -u xui-bot -f
```

## Способ 3: Использование screen или tmux

### Запуск в screen:

```bash
# Установите переменные окружения
export TELEGRAM_BOT_TOKEN="7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA"
export XUI_BASE_URL="http://103.113.71.160:41280"
export XUI_USERNAME="dd"
export XUI_PASSWORD="1I6MC7qv"

# Создайте screen сессию
screen -S xui-bot

# Запустите бота
python3 bot.py

# Отключитесь: Ctrl+A, затем D
# Вернуться: screen -r xui-bot
```

### Запуск в tmux:

```bash
# Установите переменные окружения
export TELEGRAM_BOT_TOKEN="7970667333:AAG7fg-ZeW2Z-KOKVLH6EFwEJBBFHGEZtaA"
export XUI_BASE_URL="http://103.113.71.160:41280"
export XUI_USERNAME="dd"
export XUI_PASSWORD="1I6MC7qv"

# Создайте tmux сессию
tmux new -s xui-bot

# Запустите бота
python3 bot.py

# Отключитесь: Ctrl+B, затем D
# Вернуться: tmux attach -t xui-bot
```

## Важные замечания

1. **Безопасность**: Убедитесь, что файлы с переменными окружения имеют правильные права доступа:
   ```bash
   chmod 600 .env  # если используете .env файл
   ```

2. **Адрес x-ui панели**: Обратите внимание, что адрес панели `http://103.113.71.160:41280/panel/` должен быть указан как `http://103.113.71.160:41280` (без `/panel/`), так как API работает на корневом пути.

3. **Проверка подключения**: Убедитесь, что сервер может подключиться к x-ui панели:
   ```bash
   curl http://103.113.71.160:41280/login
   ```

4. **Логи**: Логи бота будут выводиться в консоль. При использовании systemd логи доступны через `journalctl`.

## Обновление переменных окружения

После изменения переменных окружения:
- При использовании systemd: `sudo systemctl restart xui-bot`
- При использовании screen/tmux: перезапустите бота в сессии
- При использовании скрипта: просто перезапустите скрипт

