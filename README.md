<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Улучшенный Симулятор Заказа Такси</title>
    
    <style>
        /* --- CSS: Стиль интерфейса --- */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            padding: 20px;
            color: #333;
        }
        .app-container {
            max-width: 450px;
            margin: 0 auto;
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }
        h2 { color: #1a73e8; text-align: center; margin-bottom: 20px; }
        .input-group { margin-bottom: 15px; }
        .input-group label { display: block; margin-bottom: 5px; font-weight: bold; }
        .input-group input, .input-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
            background-color: #fff;
        }
        .result-box {
            margin-top: 25px;
            padding: 15px;
            border: 2px solid #1a73e8;
            border-radius: 10px;
            background-color: #e8f0fe;
        }
        .price { font-size: 1.8em; font-weight: bold; color: #4CAF50; }
        .address-suggestion {
            background-color: #f9f9f9;
            padding: 5px;
            border-bottom: 1px solid #eee;
            cursor: pointer;
        }
        .address-suggestion:hover { background-color: #e8f0fe; }
    </style>
</head>
<body>

    <div class="app-container">
        <h2>✨ Улучшенный Заказ Такси</h2>

        <div class="input-group">
            <label for="startAddress">📍 Откуда:</label>
            <input type="text" id="startAddress" value="ул. Шевченко, 10" placeholder="Введите адрес отправления">
            <div id="startSuggestions" class="suggestions"></div>
        </div>

        <div class="input-group">
            <label for="endAddress">🏁 Куда:</label>
            <input type="text" id="endAddress
