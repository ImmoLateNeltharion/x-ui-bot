# Решение проблемы: ModuleNotFoundError: No module named 'config'

## Проблема
Файл `config.py` отсутствует, потому что он в `.gitignore` и не был скопирован из репозитория.

## Решение

На сервере выполните команду:

```bash
cp config.py.example config.py
```

Это создаст файл `config.py` из примера. Поскольку в `config.py.example` используются переменные окружения через `os.getenv()`, все значения будут автоматически браться из переменных окружения, которые вы уже установили.

## Проверка

После создания файла проверьте:

```bash
# Убедитесь, что файл создан
ls -la config.py

# Запустите бота
python3 bot.py
```

## Альтернативный способ (если не помогло)

Если по какой-то причине файл `config.py.example` отсутствует, создайте файл `config.py` вручную:

```bash
nano config.py
```

И вставьте следующее содержимое:

```python
"""
Конфигурационный файл для Telegram бота x-ui
"""
import os
from typing import Optional

# Telegram Bot Token (получить у @BotFather)
TELEGRAM_BOT_TOKEN: Optional[str] = os.getenv("TELEGRAM_BOT_TOKEN", "")

# x-ui API настройки
XUI_BASE_URL: str = os.getenv("XUI_BASE_URL", "http://your-server:54321")
XUI_USERNAME: str = os.getenv("XUI_USERNAME", "your_username")
XUI_PASSWORD: str = os.getenv("XUI_PASSWORD", "your_password")

# Список разрешенных Telegram username пользователей (опционально)
ALLOWED_USERNAMES: list[str] = []

# ID inbound по умолчанию
DEFAULT_INBOUND_ID: Optional[int] = None

# Администраторы бота
ADMIN_USERNAMES: list[str] = []

# Интервал проверки напоминаний (в секундах)
REMINDER_CHECK_INTERVAL: int = 3600  # 1 час

# Напоминания за N дней до истечения
REMINDER_DAYS: list[int] = [10, 3]
```

Сохраните файл (Ctrl+X, затем Y, затем Enter) и запустите бота снова.

