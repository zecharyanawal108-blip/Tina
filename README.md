import json
import os
from datetime import datetime
from pathlib import Path

# --- Настройки ---
CONFIG_FILENAME = "service_config.json"
CONFIG_PATH = Path(CONFIG_FILENAME)

def generate_default_config(service_name="api-gateway-prod"):
    """
    Создает сложную структуру JSON, имитирующую конфигурацию микросервиса.
    """
    default_config = {
        "service": {
            "name": service_name,
            "version": "1.5.0",
            "environment": "production"
        },
        "database": {
            "host": "rds-master.us-east-1.aws",
            "port": 5432,
            "timeout_ms": 2000,
            "connection_string_secret": f"{service_name}-db-secret-key"
        },
        "logging": {
            "level": "INFO",
            "output": "stdout",
            "format": "json"
        },
        "features": {
            "enable_telemetry": True,
            "rate_limiting_enabled": False
        },
        "metadata": {
            "created_at": datetime.now().isoformat()
        }
    }
    return default_config

def save_config(config_data, filename=CONFIG_PATH):
    """
    Сохраняет словарь Python в файл JSON с красивым форматированием (indent=4).
    """
    try:
        with open(filename, 'w') as f:
            # Использование indent=4 для читаемого форматирования (pretty print)
            json.dump(config_data, f, indent=4)
        print(f"✅ Конфигурация успешно сохранена в '{filename}'")
    except IOError as e:
        print(f"❌ Ошибка при сохранении файла: {e}")

def load_config(filename=CONFIG_PATH):
    """
    Загружает и парсит данные из файла JSON.
    """
    if not filename.exists():
        print(f"❌ Файл конфигурации '{filename}' не найден.")
        return None
    
    try:
        with open(filename, 'r') as f:
            # Преобразование JSON-строки в словарь Python
            config = json.load(f)
        return config
    except json.JSONDecodeError as e:
        print(f"❌ Ошибка парсинга JSON в файле '{filename}': {e}")
        return None

def modify_config_value(config, key_path, new_value):
    """
    Модифицирует значение по заданному пути (например, "database.port").
    """
    keys = key_path.split('.')
    current = config
    
    # Проходим по вложенным ключам до предпоследнего
    for key in keys[:-1]:
        if key in current and isinstance(current[key], dict):
            current = current[key]
        else:
            print(f"⚠️ Ошибка: Ключ '{key_path}' не найден или структура неверна.")
            return False
    
    # Устанавливаем новое значение для последнего ключа
    last_key = keys[-1]
    if last_key in current:
        print(f"⚙️ Изменение: '{key_path}' изменено с '{current[last_key]}' на '{new_value}'")
        current[last_key] = new_value
        return True
    else:
        print(f"⚠️ Ошибка: Конечный ключ '{last_key}' не найден.")
        return False

# --- Главный запуск программы ---

print("--- 💻 ГЕНЕРАТОР И РЕДАКТОР JSON-КОНФИГУРАЦИИ ---")

# 1. Генерация и сохранение дефолтной конфигурации
initial_config = generate_default_config()
save_config(initial_config)

# 2. Загрузка конфигурации
current_config = load_config()

if current_config:
    print("\n--- Модификация ---")
    
    # 3. Модификация значений
    modify_config_value(current_config, "database.host", "db-replica.
