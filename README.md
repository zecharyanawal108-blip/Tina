from cryptography.fernet import Fernet
import os
import base64

# --- 1. Основные функции безопасности ---

def generate_encryption_key():
    """
    Генерирует новый 32-байтный ключ Fernet, закодированный в Base64.
    Этот ключ должен храниться в безопасности!
    """
    # 

[Image of symmetric encryption diagram]

    return Fernet.generate_key()

def save_key(key, filename="secret.key"):
    """Сохраняет ключ в файл."""
    with open(filename, 'wb') as key_file:
        key_file.write(key)
    print(f"✅ Ключ сохранен в файл: {filename}")

def load_key(filename="secret.key"):
    """Загружает ключ из файла."""
    try:
        with open(filename, 'rb') as key_file:
            return key_file.read()
    except FileNotFoundError:
        print(f"❌ ОШИБКА: Файл ключа '{filename}' не найден. Сгенерируйте его сначала.")
        return None

def encrypt_data(data, key):
    """Шифрует данные (строку) с использованием ключа Fernet."""
    f = Fernet(key)
    # Данные должны быть в формате bytes
    encoded_data = data.encode()
    encrypted_token = f.encrypt(encoded_data)
    # Возвращаем зашифрованный результат в виде строки для удобства
    return encrypted_token.decode()

def decrypt_data(encrypted_data, key):
    """Дешифрует данные (строку) с использованием ключа Fernet."""
    f = Fernet(key)
    # Данные должны быть в формате bytes (снова кодируем)
    encrypted_token = encrypted_data.encode()
    decrypted_data = f.decrypt(encrypted_token)
    # Возвращаем дешифрованный результат в виде строки
    return decrypted_data.decode()

# --- 2. Демонстрация работы ---

KEY_FILE = "security_demo.key"
ORIGINAL_DATA = "Это очень секретная строка, которую нужно зашифровать перед сохранением."

print("--- 🔐 ИНСТРУМЕНТ СИММЕТРИЧНОГО ШИФРОВАНИЯ (Fernet) ---")
print(f"Исходные данные: '{ORIGINAL_DATA}'")
print("-" * 60)

# 1. Генерируем ключ и сохраняем его
encryption_key = generate_encryption_key()
save_key(encryption_key, KEY_FILE)

# 2. Шифруем данные
encrypted_text = encrypt_data(ORIGINAL_DATA, encryption_key)

print("🔒 Зашифрованные данные (Token):")
print(f"{encrypted_text}")
print("-" * 60)

# 3. Дешифруем данные (используя ТОТ ЖЕ ключ)
# Имитируем загрузку ключа из файла
loaded_key = load_key(KEY_FILE)

if loaded_key:
    decrypted_text = decrypt_data(encrypted_text, loaded_key)
    
    print("🔓 Дешифрованные данные:")
    print(f"'{decrypted_text}'")
    
    # 4. Проверка
    is_match = decrypted_text == ORIGINAL_DATA
    print("-" * 60)
    print(f"Статус проверки: {'✅ Успех' if is_match else '❌ Ошибка'}")

# 5. Очистка (удаление файла ключа)
os.remove(KEY_FILE)
print(f"\nУдален временный файл ключа: {KEY_FILE}")
