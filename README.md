<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Симулятор Заказа Еды</title>
    
    <style>
        /* --- CSS: Стилизация, имитирующая мобильное приложение --- */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f8f8f8;
            padding: 10px;
            color: #333;
        }

        .app-container {
            max-width: 450px;
            margin: 0 auto;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        header {
            background-color: #ff5722; /* Цвет доставки */
            color: white;
            padding: 15px;
            text-align: center;
            font-size: 1.2em;
            font-weight: bold;
        }

        .menu-list, .cart-summary {
            padding: 15px;
        }

        .menu-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }

        .item-info h4 {
            margin: 0;
            font-size: 1em;
        }

        .item-info p {
            margin: 2px 0 0;
            color: #777;
            font-size: 0.9em;
        }

        .add-btn {
            background-color: #4CAF50; /* Зеленый цвет для "Добавить" */
            color: white;
            border: none;
            padding: 5px 12px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.2s;
        }

        .add-btn:hover {
            background-color: #45a049;
        }

        /* --- Корзина (Cart) --- */
        .cart-title {
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 15px;
            color: #ff5722;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
            font-size: 0.95em;
        }

        .cart-item .controls button {
            background: none;
            border: 1px solid #ccc;
            padding: 2px 8px;
            cursor: pointer;
            margin: 0 5px;
            border-radius: 3px;
        }

        .checkout-btn {
            width: 100%;
            background-color: #1a73e
