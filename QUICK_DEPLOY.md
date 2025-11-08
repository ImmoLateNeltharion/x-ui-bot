# Быстрое развертывание на сервере

## Шаг 1: Подключитесь к серверу

```bash
ssh root@103.113.71.160
```

## Шаг 2: Выполните команды развертывания

```bash
cd ~/x-ui-bot && \
git pull origin main && \
find . -type d -name __pycache__ -exec rm -r {} + 2>/dev/null || true && \
find . -name "*.pyc" -delete 2>/dev/null || true && \
find . -name "*.pyo" -delete 2>/dev/null || true && \
pip3 install -r requirements.txt --upgrade && \
systemctl stop xui-bot && \
sleep 1 && \
systemctl start xui-bot && \
sleep 2 && \
systemctl status xui-bot
```

## Шаг 3: Проверьте логи

```bash
journalctl -u xui-bot -f
```

## Шаг 4: Протестируйте в боте

1. Откройте Telegram
2. Найдите вашего бота
3. **Выполните команду `/create`** (не `/start`!)
4. Должны появиться кнопки с серверами (по 2 в ряд)

## Важно!

- **Кнопки появляются при `/create`**, а не при `/start`
- **Кнопки появляются при `/list`**, чтобы выбрать сервер для просмотра клиентов
- Если кнопок нет, проверьте логи на ошибки

## Проверка работы

После выполнения `/create` в логах должно быть:
```
Отправляю сообщение с X строками кнопок, всего Y кнопок
```

Если этого нет, значит:
1. Список inbounds пустой - проверьте подключение к x-ui
2. Есть ошибка - проверьте логи

