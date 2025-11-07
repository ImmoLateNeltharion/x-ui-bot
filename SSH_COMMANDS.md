# Команды для работы с сервером через SSH

## Подключение к серверу

```bash
ssh root@103.113.71.160
# Пароль: j926kPIY2M2A
```

## Основные команды для развертывания

### 1. Получить обновления из git

```bash
cd ~/x-ui-bot
git pull origin main
```

### 2. Установить/обновить зависимости

```bash
pip3 install -r requirements.txt --upgrade
```

### 3. Создать config.py (если отсутствует)

```bash
cp config.py.example config.py
```

### 4. Перезапустить бота (systemd)

```bash
systemctl restart xui-bot
systemctl status xui-bot
```

### 5. Проверить логи

```bash
journalctl -u xui-bot -n 50 -f
```

## Команды для диагностики

### Проверить статус сервиса

```bash
systemctl status xui-bot
```

### Проверить процессы

```bash
ps aux | grep bot.py
```

### Проверить переменные окружения

```bash
systemctl show xui-bot --property=Environment
```

### Проверить подключение к x-ui API

```bash
curl http://103.113.71.160:41280/login
```

### Проверить файлы

```bash
cd ~/x-ui-bot
ls -la config.py bot.py
```

## Полная последовательность развертывания

```bash
# Подключиться к серверу
ssh root@103.113.71.160

# Перейти в директорию
cd ~/x-ui-bot

# Получить обновления
git pull origin main

# Установить зависимости
pip3 install -r requirements.txt --upgrade

# Создать config.py если нужно
[ ! -f config.py ] && cp config.py.example config.py

# Перезапустить сервис
systemctl restart xui-bot

# Проверить статус
systemctl status xui-bot
```

## Выполнение команд одной строкой через SSH

```bash
# Получить обновления и перезапустить
ssh root@103.113.71.160 "cd ~/x-ui-bot && git pull origin main && pip3 install -r requirements.txt --upgrade && systemctl restart xui-bot"

# Проверить статус
ssh root@103.113.71.160 "systemctl status xui-bot"

# Посмотреть логи
ssh root@103.113.71.160 "journalctl -u xui-bot -n 50"
```

## Важно

- Все секреты хранятся в переменных окружения на сервере
- Не коммитьте файлы с паролями и токенами в git
- Используйте переменные окружения для всех чувствительных данных

