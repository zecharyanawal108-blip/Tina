<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Симулятор Заказа Такси</title>
    
    <style>
        /* --- CSS: Стиль интерфейса --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            padding: 20px;
            color: #333;
        }

        .app-container {
            max-width: 400px;
            margin: 0 auto;
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }

        h2 {
            color: #1a73e8;
            text-align: center;
            margin-bottom: 20px;
        }

        .input-group {
            margin-bottom: 15px;
        }

        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        .input-group input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
        }

        .result-box {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #1a73e8;
            border-radius: 8px;
            background-color: #e8f0fe;
        }

        .result-box p {
            margin: 5px 0;
        }

        .price {
            font-size: 1.5em;
            font-weight: bold;
            color: #4CAF50; /* Зеленый цвет для цены */
        }
    </style>
</head>
<body>

    <div class="app-container">
        <h2>🚕 Симулятор Заказа Такси</h2>

        <div class="input-group">
            <label for="startPoint">📍 Точка отправления (Latitude):</label>
            <input type="number" id="startPoint" value="48.46">
        </div>

        <div class="input-group">
            <label for="endPoint">🏁 Точка назначения (Latitude):</label>
            <input type="number" id="endPoint" value="48.56">
        </div>
        
        <div class="input-group">
            <label for="multiplier">Коэффициент спроса (1.0 - 2.0):</label>
            <input type="number" id="multiplier" value="1.2" step="0.1" min="1.0" max="2.0">
        </div>

        <div class="result-box" id="resultBox">
            <p>Расстояние: <span id="distance">0.0</span> км</p>
            <p>Базовый тариф: <span id="basePrice">0</span></p>
            <p>Коэффициент: <span id="currentMultiplier">1.0</span></p>
            <p>Итоговая стоимость:</p>
            <p class="price"><span id="finalPrice">0</span> у.е.</p>
        </div>
    </div>

    <script>
        // --- JAVASCRIPT: Логика расчета тарифа ---
        
        // 1. КОНСТАНТЫ И НАСТРОЙКИ ТАРИФА
        const BASE_RATE_PER_KM = 5; // Базовая стоимость за километр
        const MINIMUM_FARE = 50;    // Минимальная стоимость поездки
        const RESULT_BOX = document.getElementById('resultBox');

        // 2. Вспомогательная функция для имитации расчета дистанции (Гипотетический расчет)
        function calculateDistance(startLat, endLat) {
            // В реальном приложении здесь был бы вызов Google Maps API или другого сервиса
            // Мы используем очень упрощенную формулу, имитирующую изменение расстояния
            const EARTH_RADIUS_KM = 6371; 
            const latDiff = Math.abs(endLat - startLat);
            
            // Имитируем расстояние по упрощенной формуле (просто для генерации числа)
            const simulatedDistance = latDiff * EARTH_RADIUS_KM / 111; 
            
            return Math.max(0.1, simulatedDistance); // Минимум 100 метров
        }

        // 3. Основная функция расчета тарифа
        function calculateFare() {
            const startLat = parseFloat(document.getElementById('startPoint').value) || 0;
            const endLat = parseFloat(document.getElementById('endPoint').value) || 0;
            const multiplier = parseFloat(document.getElementById('multiplier').value) || 1.0;

            // Вычисляем дистанцию
            const distance = calculateDistance(startLat, endLat);
            
            // Вычисляем базовую стоимость
            let baseFare = distance * BASE_RATE_PER_KM;
            
            // Применяем минимальный тариф
            baseFare = Math.max(baseFare, MINIMUM_FARE);
            
            // Применяем коэффициент спроса (Surge Pricing)
            const finalFare = baseFare * multiplier;

            // 4. Обновление интерфейса (имитация рендеринга)
            document.getElementById('distance').textContent = distance.toFixed(2);
            document.getElementById('basePrice').textContent = baseFare.toFixed(2);
            document.getElementById('currentMultiplier').textContent = multiplier.toFixed(1);
            document.getElementById('finalPrice').textContent = finalFare.toFixed(2);
        }

        // 5. Инициализация и слушатели событий
        
        // Запускаем расчет при загрузке страницы
        calculateFare(); 

        // Привязываем функцию расчета к изменению любого поля
        document.getElementById('startPoint').addEventListener('input', calculateFare);
        document.getElementById('endPoint').addEventListener('input', calculateFare);
        document.getElementById('multiplier').addEventListener('input', calculateFare);

    </script>

</body>
</html>
