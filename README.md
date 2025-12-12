import socket
import time
from concurrent.futures import ThreadPoolExecutor
import argparse
import random

# --- 1. Настройки и константы ---

# Максимальное количество потоков
MAX_WORKERS = 50 
# Таймаут в секундах для ожидания ответа от порта
SCAN_TIMEOUT = 0.5

# --- 2. Основная функция сканирования ---

def scan_port(host, port, timeout):
    """
    Пытается установить TCP-соединение с указанным портом.
    Возвращает статус порта: 'Открыт' или 'Закрыт/Фильтруется'.
    """
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.settimeout(timeout)
    
    try:
        # Попытка подключения
        result = sock.connect_ex((host, port))
        
        # connect_ex возвращает 0, если соединение установлено (порт открыт)
        if result == 0:
            status = "ОТКРЫТ"
        else:
            status = "Закрыт/Фильтр"
            
    except socket.gaierror:
        # Ошибка разрешения имени хоста
        status = "Ошибка: Неизвестный хост"
    except Exception:
        # Другие ошибки (например, таймаут, если connect_ex не сработал корректно)
        status = "Ошибка: Другая проблема"
        
    finally:
        sock.close()
        
    return host, port, status

# --- 3. Главная функция запуска сканера ---

def run_port_scanner(host, port_range, max_workers, timeout):
    """
    Управляет многопоточным сканированием портов.
    """
    
    # Генерация списка портов для сканирования
    start_port, end_port = map(int, port_range.split('-'))
    ports_to_scan = list(range(start_port, end_port + 1))
    
    print(f"\n--- 📡 МНОГОПОТОЧНЫЙ СКАНЕР ПОРТОВ ---")
    print(f"Целевой хост: {host}")
    print(f"Диапазон портов: {start_port}-{end_port} ({len(ports_to_scan)} портов)")
    print(f"Потоков: {max_workers} | Таймаут: {timeout} с")
    print("-" * 60)

    open_ports = []
    
    start_time_total = time.time()
    
    # Использование ThreadPoolExecutor для параллельного выполнения
    # 
    with ThreadPoolExecutor(max_workers=max_workers) as executor:
        
        # Создаем карту аргументов для передачи в scan_port
        futures = {executor.submit(scan_port, host, port, timeout): port for port in ports_to_scan}
        
        # Обработка результатов по мере их готов
