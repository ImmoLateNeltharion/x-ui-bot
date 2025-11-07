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
export TELEGRAM_BOT_TOKEN="your_bot_token_here"
export XUI_BASE_URL="http://your-server:port"
export XUI_USERNAME="your_username"
export XUI_PASSWORD="your_password"
```

#### Вариант B: В файле ~/.bashrc или ~/.profile (постоянный для пользователя)

```bash
echo 'export TELEGRAM_BOT_TOKEN="your_bot_token_here"' >> ~/.bashrc
echo 'export XUI_BASE_URL="http://your-server:port"' >> ~/.bashrc
echo 'export XUI_USERNAME="your_username"' >> ~/.bashrc
echo 'export XUI_PASSWORD="your_password"' >> ~/.bashrc
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
export TELEGRAM_BOT_TOKEN="your_bot_token_here"
export XUI_BASE_URL="http://your-server:port"
export XUI_USERNAME="your_username"
export XUI_PASSWORD="your_password"

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
export TELEGRAM_BOT_TOKEN="your_bot_token_here"
export XUI_BASE_URL="http://your-server:port"
export XUI_USERNAME="your_username"
export XUI_PASSWORD="your_password"

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

2. **Адрес x-ui панели**: Обратите внимание, что адрес панели должен быть указан без `/panel/` в конце, так как API работает на корневом пути.

3. **Проверка подключения**: Убедитесь, что сервер может подключиться к x-ui панели:
   ```bash
   curl http://your-server:port/login
   ```

4. **Логи**: Логи бота будут выводиться в консоль. При использовании systemd логи доступны через `journalctl`.

## Обновление переменных окружения

После изменения переменных окружения:
- При использовании systemd: `sudo systemctl restart xui-bot`
- При использовании screen/tmux: перезапустите бота в сессии
- При использовании скрипта: просто перезапустите скрипт

