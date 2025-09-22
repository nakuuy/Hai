</html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QRISku - Pembayaran QRIS</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f5f5f5;
            color: #333;
            line-height: 1.6;
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            padding-bottom: 100px; /* Space for bottom navigation and inventory button */
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: linear-gradient(135deg, #333, #555);
            color: white;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
            position: relative;
        }

        header h1 {
            margin-bottom: 10px;
            font-size: 1.8rem;
        }

        header p {
            font-size: 1rem;
            opacity: 0.9;
        }

        .profile-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .profile-btn:hover {
            transform: scale(1.1);
        }

        .input-section {
            margin-bottom: 20px;
            background-color: #fff;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.06);
            border: 1px solid #ddd;
        }

        .input-section h3 {
            margin-bottom: 15px;
            color: #333;
            font-size: 1.3rem;
            text-align: center;
        }

        .amount-display {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 20px;
            padding: 15px;
            background-color: #f8f9fc;
            border-radius: 10px;
            border: 2px solid #e0e0e0;
            font-weight: bold;
            color: #333;
            min-height: 90px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .numeric-keypad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .key {
            padding: 18px;
            background-color: #fff;
            color: #333;
            border: 1px solid #e0e0e0;
            border-radius: 10px;
            font-size: 1.5rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 2px 5px rgba(0,0,0,0.04);
        }

        .key:hover {
            background-color: #f0f0f0;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .key:active {
            transform: translateY(0);
        }

        .action-buttons {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 12px;
        }

        .convert-button {
            background-color: #333;
            color: white;
            border: none;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .convert-button:hover {
            background-color: #555;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .clear-button {
            background-color: #f8f9fc;
            color: #333;
            border: 1px solid #e0e0e0;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .clear-button:hover {
            background-color: #e0e0e0;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .loading {
            display: none;
            text-align: center;
            margin: 20px 0;
        }

        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #333;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .settings-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .settings-content {
            background-color: white;
            padding: 30px;
            border-radius: 12px;
            width: 90%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }

        .settings-content h2 {
            margin-bottom: 20px;
            color: #333;
            text-align: center;
        }

        .close-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #8a93a6;
        }

        .close-btn:hover {
            color: #333;
        }

        .drop-area {
            border: 2px dashed #333;
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            margin-bottom: 20px;
            transition: all 0.3s ease;
            background-color: #f8f9fc;
            cursor: pointer;
        }

        .drop-area:hover, .drop-area.dragover {
            background-color: #e0e0e0;
            border-color: #555;
        }

        .drop-area p {
            color: #666;
            margin-bottom: 15px;
            font-size: 0.9rem;
        }

        .drop-area i {
            font-size: 2rem;
            color: #333;
            margin-bottom: 15px;
            display: block;
        }

        .text-input {
            width: 100%;
            padding: 15px;
            border: 1px solid #d5d9e4;
            border-radius: 8px;
            font-size: 1rem;
            margin-bottom: 20px;
            resize: vertical;
            transition: border-color 0.3s;
        }

        .text-input:focus {
            border-color: #333;
            outline: none;
        }

        .save-button {
            background-color: #333;
            color: white;
            border: none;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
        }

        .save-button:hover {
            background-color: #555;
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .payment-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 1001;
            justify-content: center;
            align-items: center;
        }

        .payment-content {
            background-color: white;
            padding: 25px;
            border-radius: 15px;
            width: 90%;
            max-width: 350px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            text-align: center;
        }

        .payment-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid #e0e0e0;
        }

        .payment-time {
            font-size: 1.2rem;
            font-weight: bold;
            color: #333;
            display: none; /* Sembunyikan jam */
        }

        .payment-title {
            font-size: 1.5rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 5px;
        }

        .payment-subtitle {
            color: #666;
            margin-bottom: 15px;
        }

        .payment-amount {
            font-size: 1.8rem;
            font-weight: bold;
            color: #000;
            margin: 15px 0;
        }

        .payment-qr {
            margin: 15px 0;
        }

        .payment-qr img {
            width: 220px;
            height: 220px;
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 10px;
            background: white;
        }

        .payment-info {
            background-color: #f8f9fc;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            text-align: left;
        }

        .info-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
        }

        .info-label {
            color: #666;
        }

        .info-value {
            font-weight: 600;
            color: #333;
        }

        .close-payment {
            background-color: #333;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            width: 100%;
            margin-top: 15px;
        }

        .close-payment:hover {
            background-color: #555;
        }

        footer {
            text-align: center;
            margin-top: 50px;
            padding: 20px;
            color: #666;
            font-size: 0.9rem;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: #333;
        }

        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 12px;
            border: 1px solid #d5d9e4;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            border-color: #333;
            outline: none;
        }

        .password-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .password-content {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 400px;
            text-align: center;
        }

        .password-content h2 {
            margin-bottom: 20px;
            color: #333;
        }

        .password-input {
            width: 100%;
            padding: 15px;
            border: 2px solid #333;
            border-radius: 8px;
            font-size: 1.2rem;
            text-align: center;
            letter-spacing: 5px;
            margin-bottom: 20px;
        }

        .password-error {
            color: #d32f2f;
            margin-bottom: 15px;
            display: none;
        }

        .settings-tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
            overflow-x: auto;
        }

        .tab-btn {
            padding: 10px 15px;
            background: none;
            border: none;
            cursor: pointer;
            font-weight: 600;
            color: #666;
            border-bottom: 3px solid transparent;
            white-space: nowrap;
        }

        .tab-btn.active {
            color: #333;
            border-bottom: 3px solid #333;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background-color: white;
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
            z-index: 999;
            border-top: 1px solid #e0e0e0;
        }

        .nav-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            background: none;
            border: none;
            font-size: 0.8rem;
            color: #666;
            cursor: pointer;
            transition: color 0.3s;
        }

        .nav-btn.active {
            color: #333;
        }

        .nav-btn i {
            font-size: 1.2rem;
            margin-bottom: 5px;
        }

        /* New styles for additional pages */
        .page {
            display: none;
            animation: fadeIn 0.3s ease;
        }

        .page.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .transaction-list {
            margin-top: 20px;
        }

        .transaction-item {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            border: 1px solid #e0e0e0;
        }

        .transaction-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }

        .transaction-id {
            font-weight: 600;
            color: #333;
        }

        .transaction-date {
            color: #666;
            font-size: 0.9rem;
        }

        .transaction-details {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .transaction-amount {
            font-weight: bold;
            color: #333;
            font-size: 1.2rem;
        }

        .transaction-status {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            display: none; /* Sembunyikan status pembayaran */
        }

        .status-success {
            background-color: #e8f5e9;
            color: #2e7d32;
        }

        .qrcode-display {
            text-align: center;
            margin: 20px 0;
            padding: 20px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            border: 1px solid #e0e0e0;
        }

        .qrcode-display img {
            max-width: 100%;
            height: auto;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
        }

        .phone-input-section {
            margin-bottom: 20px;
        }

        .phone-display {
            text-align: center;
            font-size: 1.8rem;
            margin-bottom: 20px;
            padding: 15px;
            background-color: #f8f9fc;
            border-radius: 8px;
            border: 2px solid #e0e0e0;
            font-weight: bold;
            color: #333;
            min-height: 70px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .phone-history {
            margin-top: 20px;
        }

        .history-item {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            cursor: pointer;
            transition: all 0.2s;
            border: 1px solid #e0e0e0;
        }

        .history-item:hover {
            background-color: #f0f0f0;
        }

        .history-phone {
            font-weight: 600;
            margin-bottom: 5px;
            color: #333;
        }

        .history-name {
            color: #666;
            font-size: 0.9rem;
        }

        .pulsa-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .pulsa-content {
            background-color: white;
            padding: 25px;
            border-radius: 15px;
            width: 90%;
            max-width: 350px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .pulsa-title {
            font-size: 1.3rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 15px;
            text-align: center;
        }

        .pulsa-options {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .pulsa-option {
            padding: 15px;
            background-color: #f8f9fc;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s;
        }

        .pulsa-option:hover {
            background-color: #e0e0e0;
        }

        .pulsa-option.active {
            border-color: #333;
            background-color: #e0e0e0;
        }

        .pulsa-amount {
            font-weight: bold;
            color: #333;
        }

        .pulsa-price {
            color: #666;
            font-size: 0.9rem;
        }

        /* Debt styles */
        .debt-section {
            margin-top: 20px;
        }

        .debt-form {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            margin-bottom: 20px;
            border: 1px solid #e0e0e0;
        }

        .debt-list {
            margin-top: 20px;
        }

        .debt-item {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            position: relative;
            border: 1px solid #e0e0e0;
        }

        .debt-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            align-items: flex-start;
        }

        .debt-name {
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
        }

        .debt-phone {
            color: #666;
            font-size: 0.9rem;
        }

        .debt-details {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .debt-amount {
            font-weight: bold;
            color: #333;
            font-size: 1.2rem;
        }

        .debt-status {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            cursor: pointer;
        }

        .status-pending {
            background-color: #ffecb3;
            color: #f57c00;
        }

        .status-paid {
            background-color: #e8f5e9;
            color: #2e7d32;
        }

        .debt-item-menu {
            position: absolute;
            top: 10px;
            right: 10px;
            background: none;
            border: none;
            color: #666;
            cursor: pointer;
        }

        .debt-item-menu:hover {
            color: #333;
        }

        .debt-item-menu-content {
            display: none;
            position: absolute;
            top: 25px;
            right: 0;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            z-index: 10;
            width: 120px;
            border: 1px solid #e0e0e0;
        }

        .debt-item-menu-content button {
            display: block;
            width: 100%;
            padding: 10px;
            background: none;
            border: none;
            text-align: left;
            cursor: pointer;
        }

        .debt-item-menu-content button:hover {
            background-color: #f0f0f0;
        }

        .debt-edit-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .debt-edit-content {
            background-color: white;
            padding: 25px;
            border-radius: 15px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        /* QRIS Image Preview */
        .qris-preview {
            text-align: center;
            margin-top: 15px;
            display: none;
        }

        .qris-preview img {
            max-width: 100%;
            max-height: 200px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }

        /* Finance Tabs */
        .finance-tabs {
            display: flex;
            margin-bottom: 15px;
            border-bottom: 1px solid #ddd;
        }

        .finance-tab {
            padding: 10px 15px;
            background: none;
            border: none;
            cursor: pointer;
            font-weight: 600;
            color: #666;
            border-bottom: 3px solid transparent;
        }

        .finance-tab.active {
            color: #333;
            border-bottom: 3px solid #333;
        }

        .debt-item-name {
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
        }

        /* Inventory Button */
        .inventory-btn {
            position: fixed;
            bottom: 80px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #333, #555);
            color: white;
            border: none;
            border-radius: 50%;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            z-index: 998;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }

        .inventory-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
        }

        /* Inventory Page */
        #inventoryPage {
            display: none;
        }

        #inventoryPage.active {
            display: block;
        }

        .inventory-form {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            margin-bottom: 20px;
            border: 1px solid #e0e0e0;
        }

        .inventory-list {
            margin-top: 20px;
        }

        .inventory-item {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            position: relative;
            border: 1px solid #e0e0e0;
        }

        .inventory-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            align-items: flex-start;
        }

        .inventory-name {
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
        }

        .inventory-category {
            color: #666;
            font-size: 0.9rem;
        }

        .inventory-details {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .inventory-stock {
            font-weight: bold;
            color: #333;
            font-size: 1.2rem;
        }

        .inventory-action {
            display: flex;
            gap: 10px;
        }

        .inventory-btn-small {
            padding: 5px 10px;
            background-color: #333;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.8rem;
        }

        .inventory-btn-small:hover {
            background-color: #555;
        }

        .inventory-item-menu {
            position: absolute;
            top: 10px;
            right: 10px;
            background: none;
            border: none;
            color: #666;
            cursor: pointer;
        }

        .inventory-item-menu:hover {
            color: #333;
        }

        .inventory-item-menu-content {
            display: none;
            position: absolute;
            top: 25px;
            right: 0;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            z-index: 10;
            width: 120px;
            border: 1px solid #e0e0e0;
        }

        .inventory-item-menu-content button {
            display: block;
            width: 100%;
            padding: 10px;
            background: none;
            border: none;
            text-align: left;
            cursor: pointer;
        }

        .inventory-item-menu-content button:hover {
            background-color: #f0f0f0;
        }

        .inventory-edit-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .inventory-edit-content {
            background-color: white;
            padding: 25px;
            border-radius: 15px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .stock-warning {
            color: #d32f2f;
            font-weight: 600;
        }

        .stock-adequate {
            color: #2e7d32;
            font-weight: 600;
        }

        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }
            
            header h1 {
                font-size: 1.5rem;
            }
            
            .amount-display {
                font-size: 2rem;
            }
            
            .key {
                padding: 15px;
                font-size: 1.3rem;
            }
            
            .payment-content {
                padding: 20px;
                max-width: 320px;
            }
            
            .payment-amount {
                font-size: 1.6rem;
            }
            
            .payment-qr img {
                width: 200px;
                height: 200px;
            }
            
            .phone-display {
                font-size: 1.5rem;
            }
            
            .pulsa-options {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .inventory-btn {
                bottom: 70px;
                right: 15px;
                width: 50px;
                height: 50px;
                font-size: 1.3rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Beranda Page -->
        <div class="page active" id="homePage">
            <header>
                <h1 id="headerTitle">QRISku</h1>
                <p id="headerSubtitle">Pembayaran QRIS Mudah dan Cepat</p>
                <button class="profile-btn" id="profileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="input-section">
                <h3>Tambah nominal</h3>
                <div class="amount-display" id="amountDisplay">0</div>
                
                <div class="numeric-keypad">
                    <button class="key" data-value="1">1</button>
                    <button class="key" data-value="2">2</button>
                    <button class="key" data-value="3">3</button>
                    <button class="key" data-value="4">4</button>
                    <button class="key" data-value="5">5</button>
                    <button class="key" data-value="6">6</button>
                    <button class="key" data-value="7">7</button>
                    <button class="key" data-value="8">8</button>
                    <button class="key" data-value="9">9</button>
                    <button class="key" data-value="000">000</button>
                    <button class="key" data-value="0">0</button>
                    <button class="key" id="backspaceBtn"><i class="fas fa-backspace"></i></button>
                </div>
                
                <div class="action-buttons">
                    <button class="convert-button" id="convertButton">Terapkan</button>
                    <button class="clear-button" id="clearButton">Clear</button>
                </div>
                
                <div class="loading" id="loading">
                    <div class="loading-spinner"></div>
                    <p>Sedang memproses...</p>
                </div>
            </div>

            <footer>
                <p id="footerText">© 2023 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Keuangan Page -->
        <div class="page" id="financePage">
            <header>
                <h1>Keuangan</h1>
                <p>Kelola transaksi dan hutang piutang</p>
                <button class="profile-btn" id="financeProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="finance-tabs">
                <button class="finance-tab active" data-tab="transactions-tab">Transaksi</button>
                <button class="finance-tab" data-tab="debts-tab">Hutang</button>
            </div>

            <!-- Transactions Tab -->
            <div id="transactions-tab" class="tab-content active">
                <div class="transaction-list" id="transactionList">
                    <!-- Transaction items will be added here dynamically -->
                </div>
            </div>

            <!-- Debts Tab -->
            <div id="debts-tab" class="tab-content">
                <div class="debt-form">
                    <h3>Tambah Hutang Baru</h3>
                    <div class="form-group">
                        <label for="debtName">Nama</label>
                        <input type="text" id="debtName" placeholder="Nama orang yang berhutang" list="debtNameList">
                        <datalist id="debtNameList">
                            <!-- Options will be added dynamically -->
                        </datalist>
                    </div>
                    <div class="form-group">
                        <label for="debtPhone">Nomor Telepon</label>
                        <input type="text" id="debtPhone" placeholder="Nomor telepon">
                    </div>
                    <div class="form-group">
                        <label for="debtItem">Nama Barang</label>
                        <input type="text" id="debtItem" placeholder="Nama barang yang dihutang">
                    </div>
                    <div class="form-group">
                        <label for="debtAmount">Jumlah Hutang</label>
                        <input type="number" id="debtAmount" placeholder="Jumlah hutang">
                    </div>
                    <button class="save-button" id="addDebtButton">Tambah Hutang</button>
                </div>

                <div class="debt-list" id="debtList">
                    <h3>Daftar Hutang</h3>
                    <!-- Debt items will be added here dynamically -->
                </div>
            </div>

            <footer>
                <p>© 2023 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Kode QRIS Page -->
        <div class="page" id="qrcodePage">
            <header>
                <h1>Kode QRIS</h1>
                <p>Scan kode QRIS untuk pembayaran</p>
                <button class="profile-btn" id="qrcodeProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="qrcode-display" id="qrcodeDisplay">
                <!-- QR code will be displayed here -->
                <p>Kode QRIS akan ditampilkan di sini</p>
            </div>

            <footer>
                <p>© 2023 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Pulsa Page -->
        <div class="page" id="pulsaPage">
            <header>
                <h1>Isi Pulsa</h1>
                <p>Isi pulsa handphone dengan mudah</p>
                <button class="profile-btn" id="pulsaProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="phone-input-section">
                <h3>Masukkan nomor handphone</h3>
                <div class="phone-display" id="phoneDisplay">0</div>
                
                <div class="numeric-keypad">
                    <button class="key" data-value="1">1</button>
                    <button class="key" data-value="2">2</button>
                    <button class="key" data-value="3">3</button>
                    <button class="key" data-value="4">4</button>
                    <button class="key" data-value="5">5</button>
                    <button class="key" data-value="6">6</button>
                    <button class="key" data-value="7">7</button>
                    <button class="key" data-value="8">8</button>
                    <button class="key" data-value="9">9</button>
                    <button class="key" data-value="+62">+62</button>
                    <button class="key" data-value="0">0</button>
                    <button class="key" id="pulsaBackspaceBtn"><i class="fas fa-backspace"></i></button>
                </div>
                
                <div class="action-buttons">
                    <button class="convert-button" id="savePhoneButton">Simpan</button>
                    <button class="clear-button" id="clearPhoneButton">Clear</button>
                </div>
            </div>

            <div class="phone-history" id="phoneHistory">
                <h3>Riwayat Nomor</h3>
                <!-- Phone history items will be added here dynamically -->
            </div>

            <footer>
                <p>© 2023 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>

        <!-- Inventory Page -->
        <div class="page" id="inventoryPage">
            <header>
                <h1>Manajemen Inventori</h1>
                <p>Kelola stok barang dan daftar belanja</p>
                <button class="profile-btn" id="inventoryProfileBtn"><i class="fas fa-user"></i></button>
            </header>

            <div class="inventory-form">
                <h3>Tambah Barang Baru</h3>
                <div class="form-group">
                    <label for="itemName">Nama Barang</label>
                    <input type="text" id="itemName" placeholder="Nama barang">
                </div>
                <div class="form-group">
                    <label for="itemCategory">Kategori</label>
                    <select id="itemCategory">
                        <option value="makanan">Makanan</option>
                        <option value="minuman">Minuman</option>
                        <option value="snack">Snack</option>
                        <option value="sembako">Sembako</option>
                        <option value="lainnya">Lainnya</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="itemStock">Stok Awal</label>
                    <input type="number" id="itemStock" placeholder="Jumlah stok">
                </div>
                <div class="form-group">
                    <label for="itemMinStock">Stok Minimum</label>
                    <input type="number" id="itemMinStock" placeholder="Batas stok minimum">
                </div>
                <button class="save-button" id="addItemButton">Tambah Barang</button>
            </div>

            <div class="inventory-list" id="inventoryList">
                <h3>Daftar Inventori</h3>
                <!-- Inventory items will be added here dynamically -->
            </div>

            <div class="shopping-list" id="shoppingList">
                <h3>Daftar Belanja</h3>
                <!-- Shopping list items will be added here dynamically -->
            </div>

            <footer>
                <p>© 2023 QRISku - Pembayaran QRIS</p>
            </footer>
        </div>
    </div>

    <!-- Bottom Navigation -->
    <div class="bottom-nav">
        <button class="nav-btn active" data-page="homePage"><i class="fas fa-home"></i><span>Beranda</span></button>
        <button class="nav-btn" data-page="financePage"><i class="fas fa-chart-line"></i><span>Keuangan</span></button>
        <button class="nav-btn" data-page="qrcodePage"><i class="fas fa-qrcode"></i><span>Kode QRIS</span></button>
        <button class="nav-btn" data-page="pulsaPage"><i class="fas fa-mobile-alt"></i><span>Pulsa</span></button>
    </div>

    <!-- Inventory Button -->
    <button class="inventory-btn" id="inventoryButton" data-page="inventoryPage">
        <i class="fas fa-box"></i>
    </button>

    <!-- Password Modal -->
    <div class="password-modal" id="passwordModal">
        <div class="password-content">
            <h2>Masukkan Sandi</h2>
            <input type="password" class="password-input" id="passwordInput" maxlength="6" placeholder="Masukkan 6 digit sandi">
            <div class="password-error" id="passwordError">Sandi salah! Coba lagi.</div>
            <button class="save-button" id="submitPassword">Submit</button>
            <button class="close-btn" id="closePassword"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <!-- Settings Modal -->
    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <button class="close-btn" id="closeSettings"><i class="fas fa-times"></i></button>
            <h2>Pengaturan QRIS</h2>
            
            <div class="settings-tabs">
                <button class="tab-btn active" data-tab="qris-settings">QRIS</button>
                <button class="tab-btn" data-tab="text-settings">Teks</button>
                <button class="tab-btn" data-tab="security-settings">Keamanan</button>
            </div>
            
            <div class="tab-content active" id="qris-settings">
                <div class="form-group">
                    <label for="merchantName">Nama Merchant</label>
                    <input type="text" id="merchantName" placeholder="Nama Merchant">
                </div>

                <div class="form-group">
                    <label for="qrisStaticCode">Kode QRIS Statis</label>
                    <textarea class="text-input" id="qrisStaticCode" placeholder="Tempel Kode QRIS statis di sini..." rows="4"></textarea>
                </div>
                
                <div class="drop-area" id="qrisDropArea">
                    <i class="fas fa-cloud-upload-alt"></i>
                    <p>Drag & drop gambar QRIS di sini atau klik untuk memilih file</p>
                    <input type="file" id="qrisInput" accept="image/*" style="display: none;">
                </div>
                
                <div class="qris-preview" id="qrisPreview">
                    <p>Preview QRIS:</p>
                    <img id="qrisPreviewImg" src="" alt="QRIS Preview">
                </div>
                
                <button class="clear-button" id="resetQrisButton">Reset QRIS</button>
            </div>
            
            <div class="tab-content" id="text-settings">
                <div class="form-group">
                    <label for="headerTitleInput">Judul Halaman</label>
                    <input type="text" id="headerTitleInput" placeholder="Judul Halaman">
                </div>
                
                <div class="form-group">
                    <label for="headerSubtitleInput">Subjudul Halaman</label>
                    <input type="text" id="headerSubtitleInput" placeholder="Subjudul Halaman">
                </div>
                
                <div class="form-group">
                    <label for="footerTextInput">Teks Footer</label>
                    <textarea id="footerTextInput" placeholder="Teks Footer" rows="2"></textarea>
                </div>
            </div>
            
            <div class="tab-content" id="security-settings">
                <div class="form-group">
                    <label for="currentPassword">Sandi Saat Ini</label>
                    <input type="password" id="currentPassword" placeholder="Masukkan sandi saat ini">
                </div>
                
                <div class="form-group">
                    <label for="newPassword">Sandi Baru</label>
                    <input type="password" id="newPassword" placeholder="Masukkan sandi baru (6 digit)">
                </div>
                
                <div class="form-group">
                    <label for="confirmPassword">Konfirmasi Sandi Baru</label>
                    <input type="password" id="confirmPassword" placeholder="Konfirmasi sandi baru">
                </div>
                
                <button class="save-button" id="changePasswordButton">Ubah Sandi</button>
            </div>
            
            <button class="save-button" id="saveButton">Simpan Pengaturan</button>
        </div>
    </div>

    <!-- Payment Modal -->
    <div class="payment-modal" id="paymentModal">
        <div class="payment-content">
            <div class="payment-header">
                <div class="payment-time" id="paymentTime">17:05</div>
                <div class="payment-close" id="closePaymentModal"><i class="fas fa-times"></i></div>
            </div>
            
            <div class="payment-title" id="paymentTitle">QRISku</div>
            <div class="payment-subtitle">Scan QRIS berikut untuk melakukan pembayaran</div>
            
            <div class="payment-amount" id="paymentAmount">Rp 0</div>
            
            <div class="payment-qr">
                <img id="paymentQr" src="" alt="QR Code">
            </div>
            
            <div class="payment-info">
                <div class="info-item">
                    <span class="info-label">Merchant</span>
                    <span class="info-value" id="paymentMerchant">Merchant</span>
                </div>
                <div class="info-item">
                    <span class="info-label">Transaction ID</span>
                    <span class="info-value" id="transactionId">TRX-789012</span>
                </div>
                <!-- Status item dihapus -->
            </div>
            
            <button class="close-payment" id="closePayment">Tutup</button>
        </div>
    </div>

    <!-- Pulsa Modal -->
    <div class="pulsa-modal" id="pulsaModal">
        <div class="pulsa-content">
            <button class="close-btn" id="closePulsaModal"><i class="fas fa-times"></i></button>
            <div class="pulsa-title">Pilih Nominal Pulsa</div>
            
            <div class="form-group">
                <label for="phoneName">Nama Kontak</label>
                <input type="text" id="phoneName" placeholder="Masukkan nama untuk nomor ini">
            </div>
            
            <div class="pulsa-options">
                <div class="pulsa-option" data-amount="5000">
                    <div class="pulsa-amount">5.000</div>
                    <div class="pulsa-price">Rp 5.000</div>
                </div>
                <div class="pulsa-option" data-amount="10000">
                    <div class="pulsa-amount">10.000</div>
                    <div class="pulsa-price">Rp 10.000</div>
                </div>
                <div class="pulsa-option" data-amount="20000">
                    <div class="pulsa-amount">20.000</div>
                    <div class="pulsa-price">Rp 20.000</div>
                </div>
                <div class="pulsa-option" data-amount="50000">
                    <div class="pulsa-amount">50.000</div>
                    <div class="pulsa-price">Rp 50.000</div>
                </div>
                <div class="pulsa-option" data-amount="100000">
                    <div class="pulsa-amount">100.000</div>
                    <div class="pulsa-price">Rp 100.000</div>
                </div>
                <div class="pulsa-option" data-amount="0">
                    <div class="pulsa-amount">Lainnya</div>
                    <div class="pulsa-price">Custom</div>
                </div>
            </div>
            
            <div class="form-group" id="customAmountGroup" style="display: none;">
                <label for="customAmount">Nominal Custom</label>
                <input type="number" id="customAmount" placeholder="Masukkan nominal pulsa">
            </div>
            
            <button class="save-button" id="confirmPulsaButton">Simpan</button>
        </div>
    </div>

    <!-- Debt Edit Modal -->
    <div class="debt-edit-modal" id="debtEditModal">
        <div class="debt-edit-content">
            <h2>Edit Hutang</h2>
            <div class="form-group">
                <label for="editDebtName">Nama</label>
                <input type="text" id="editDebtName" placeholder="Nama orang yang berhutang">
            </div>
            <div class="form-group">
                <label for="editDebtPhone">Nomor Telepon</label>
                <input type="text" id="editDebtPhone" placeholder="Nomor telepon">
            </div>
            <div class="form-group">
                <label for="editDebtItem">Nama Barang</label>
                <input type="text" id="editDebtItem" placeholder="Nama barang yang dihutang">
            </div>
            <div class="form-group">
                <label for="editDebtAmount">Jumlah Hutang</label>
                <input type="number" id="editDebtAmount" placeholder="Jumlah hutang">
            </div>
            <button class="save-button" id="saveEditDebtButton">Simpan Perubahan</button>
            <button class="close-btn" id="closeEditDebtModal"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <!-- Inventory Edit Modal -->
    <div class="inventory-edit-modal" id="inventoryEditModal">
        <div class="inventory-edit-content">
            <h2>Edit Barang</h2>
            <div class="form-group">
                <label for="editItemName">Nama Barang</label>
                <input type="text" id="editItemName" placeholder="Nama barang">
            </div>
            <div class="form-group">
                <label for="editItemCategory">Kategori</label>
                <select id="editItemCategory">
                    <option value="makanan">Makanan</option>
                    <option value="minuman">Minuman</option>
                    <option value="snack">Snack</option>
                    <option value="sembako">Sembako</option>
                    <option value="lainnya">Lainnya</option>
                </select>
            </div>
            <div class="form-group">
                <label for="editItemStock">Stok Saat Ini</label>
                <input type="number" id="editItemStock" placeholder="Jumlah stok">
            </div>
            <div class="form-group">
                <label for="editItemMinStock">Stok Minimum</label>
                <input type="number" id="editItemMinStock" placeholder="Batas stok minimum">
            </div>
            <button class="save-button" id="saveEditItemButton">Simpan Perubahan</button>
            <button class="close-btn" id="closeEditItemModal"><i class="fas fa-times"></i></button>
        </div>
    </div>

    <script>
        // Elements
        const profileBtn = document.getElementById('profileBtn');
        const financeProfileBtn = document.getElementById('financeProfileBtn');
        const qrcodeProfileBtn = document.getElementById('qrcodeProfileBtn');
        const pulsaProfileBtn = document.getElementById('pulsaProfileBtn');
        const inventoryProfileBtn = document.getElementById('inventoryProfileBtn');
        const inventoryButton = document.getElementById('inventoryButton');
        const passwordModal = document.getElementById('passwordModal');
        const passwordInput = document.getElementById('passwordInput');
        const passwordError = document.getElementById('passwordError');
        const closePassword = document.getElementById('closePassword');
        const submitPassword = document.getElementById('submitPassword');
        const settingsModal = document.getElementById('settingsModal');
        const closeSettings = document.getElementById('closeSettings');
        const amountDisplay = document.getElementById('amountDisplay');
        const convertButton = document.getElementById('convertButton');
        const clearButton = document.getElementById('clearButton');
        const paymentModal = document.getElementById('paymentModal');
        const paymentMerchant = document.getElementById('paymentMerchant');
        const paymentAmount = document.getElementById('paymentAmount');
        const paymentTitle = document.getElementById('paymentTitle');
        const paymentQr = document.getElementById('paymentQr');
        const closePayment = document.getElementById('closePayment');
        const closePaymentModal = document.getElementById('closePaymentModal');
        const loading = document.getElementById('loading');
        const qrisDropArea = document.getElementById('qrisDropArea');
        const qrisInput = document.getElementById('qrisInput');
        const saveButton = document.getElementById('saveButton');
        const backspaceBtn = document.getElementById('backspaceBtn');
        const merchantName = document.getElementById('merchantName');
        const paymentTime = document.getElementById('paymentTime');
        const transactionId = document.getElementById('transactionId');
        const headerTitle = document.getElementById('headerTitle');
        const headerSubtitle = document.getElementById('headerSubtitle');
        const footerText = document.getElementById('footerText');
        const headerTitleInput = document.getElementById('headerTitleInput');
        const headerSubtitleInput = document.getElementById('headerSubtitleInput');
        const footerTextInput = document.getElementById('footerTextInput');
        const tabBtns = document.querySelectorAll('.tab-btn');
        const tabContents = document.querySelectorAll('.tab-content');
        const navBtns = document.querySelectorAll('.nav-btn');
        const pages = document.querySelectorAll('.page');
        const phoneDisplay = document.getElementById('phoneDisplay');
        const savePhoneButton = document.getElementById('savePhoneButton');
        const clearPhoneButton = document.getElementById('clearPhoneButton');
        const pulsaBackspaceBtn = document.getElementById('pulsaBackspaceBtn');
        const phoneHistory = document.getElementById('phoneHistory');
        const transactionList = document.getElementById('transactionList');
        const qrcodeDisplay = document.getElementById('qrcodeDisplay');
        const pulsaModal = document.getElementById('pulsaModal');
        const closePulsaModal = document.getElementById('closePulsaModal');
        const pulsaOptions = document.querySelectorAll('.pulsa-option');
        const customAmountGroup = document.getElementById('customAmountGroup');
        const customAmount = document.getElementById('customAmount');
        const confirmPulsaButton = document.getElementById('confirmPulsaButton');
        const phoneName = document.getElementById('phoneName');
        const financeTabs = document.querySelectorAll('.finance-tab');
        const financeTabContents = document.querySelectorAll('#financePage .tab-content');
        const debtName = document.getElementById('debtName');
        const debtPhone = document.getElementById('debtPhone');
        const debtItem = document.getElementById('debtItem');
        const debtAmount = document.getElementById('debtAmount');
        const addDebtButton = document.getElementById('addDebtButton');
        const debtList = document.getElementById('debtList');
        const debtNameList = document.getElementById('debtNameList');
        const qrisPreview = document.getElementById('qrisPreview');
        const qrisPreviewImg = document.getElementById('qrisPreviewImg');
        const resetQrisButton = document.getElementById('resetQrisButton');
        const currentPassword = document.getElementById('currentPassword');
        const newPassword = document.getElementById('newPassword');
        const confirmPassword = document.getElementById('confirmPassword');
        const changePasswordButton = document.getElementById('changePasswordButton');
        const debtEditModal = document.getElementById('debtEditModal');
        const editDebtName = document.getElementById('editDebtName');
        const editDebtPhone = document.getElementById('editDebtPhone');
        const editDebtItem = document.getElementById('editDebtItem');
        const editDebtAmount = document.getElementById('editDebtAmount');
        const saveEditDebtButton = document.getElementById('saveEditDebtButton');
        const closeEditDebtModal = document.getElementById('closeEditDebtModal');
        const qrisStaticCode = document.getElementById('qrisStaticCode');
        
        // Inventory elements
        const itemName = document.getElementById('itemName');
        const itemCategory = document.getElementById('itemCategory');
        const itemStock = document.getElementById('itemStock');
        const itemMinStock = document.getElementById('itemMinStock');
        const addItemButton = document.getElementById('addItemButton');
        const inventoryList = document.getElementById('inventoryList');
        const shoppingList = document.getElementById('shoppingList');
        const inventoryEditModal = document.getElementById('inventoryEditModal');
        const editItemName = document.getElementById('editItemName');
        const editItemCategory = document.getElementById('editItemCategory');
        const editItemStock = document.getElementById('editItemStock');
        const editItemMinStock = document.getElementById('editItemMinStock');
        const saveEditItemButton = document.getElementById('saveEditItemButton');
        const closeEditItemModal = document.getElementById('closeEditItemModal');
        
        // State
        let currentAmount = '0';
        let currentPhone = '0';
        let savedQrisImage = localStorage.getItem('qrisImage') || '';
        let savedMerchant = localStorage.getItem('merchantName') || 'Merchant';
        let savedHeaderTitle = localStorage.getItem('headerTitle') || 'QRISku';
        let savedHeaderSubtitle = localStorage.getItem('headerSubtitle') || 'Pembayaran QRIS Mudah dan Cepat';
        let savedFooterText = localStorage.getItem('footerText') || '© 2023 QRISku - Pembayaran QRIS';
        let savedQrisStaticCode = localStorage.getItem('qrisStaticCode') || '';
        let currentQrUrl = '';
        let phoneHistoryData = JSON.parse(localStorage.getItem('phoneHistory')) || [];
        let transactions = JSON.parse(localStorage.getItem('transactions')) || [];
        let selectedPulsaAmount = 0;
        let selectedPhoneNumber = '';
        let debts = JSON.parse(localStorage.getItem('debts')) || [];
        let appPassword = localStorage.getItem('appPassword') || '131313';
        let currentDebtId = null;
        let passwordContext = null;
        let inventory = JSON.parse(localStorage.getItem('inventory')) || [];
        let currentItemId = null;
        
        // Initialize
        function initializeApp() {
            merchantName.value = savedMerchant;
            headerTitleInput.value = savedHeaderTitle;
            headerSubtitleInput.value = savedHeaderSubtitle;
            footerTextInput.value = savedFooterText;
            qrisStaticCode.value = savedQrisStaticCode;
            paymentMerchant.textContent = savedMerchant;
            headerTitle.textContent = savedHeaderTitle;
            headerSubtitle.textContent = savedHeaderSubtitle;
            footerText.textContent = savedFooterText;
            paymentTitle.textContent = savedHeaderTitle;
            renderPhoneHistory();
            renderTransactions();
            updateQrCodeDisplay();
            renderDebts();
            updateDebtNameList();
            renderInventory();
            renderShoppingList();
            
            // Show QRIS image preview if exists
            if (savedQrisImage) {
                qrisPreview.style.display = 'block';
                qrisPreviewImg.src = savedQrisImage;
            }
        }
        
        // Finance tabs functionality
        financeTabs.forEach(tab => {
            tab.addEventListener('click', () => {
                const tabId = tab.getAttribute('data-tab');
                
                // Remove active class from all tabs and contents
                financeTabs.forEach(t => t.classList.remove('active'));
                financeTabContents.forEach(content => content.classList.remove('active'));
                
                // Add active class to current tab and content
                tab.classList.add('active');
                document.getElementById(tabId).classList.add('active');
            });
        });
        
        // Settings tabs functionality
        tabBtns.forEach(tabBtn => {
            tabBtn.addEventListener('click', () => {
                const tabId = tabBtn.getAttribute('data-tab');
                
                // Remove active class from all tabs and contents
                tabBtns.forEach(btn => btn.classList.remove('active'));
                tabContents.forEach(content => content.classList.remove('active'));
                
                // Add active class to current tab and content
                tabBtn.classList.add('active');
                document.getElementById(tabId).classList.add('active');
            });
        });
        
        // Navigation functionality
        navBtns.forEach(navBtn => {
            navBtn.addEventListener('click', () => {
                const pageId = navBtn.getAttribute('data-page');
                
                // Remove active class from all nav buttons and pages
                navBtns.forEach(btn => btn.classList.remove('active'));
                pages.forEach(page => page.classList.remove('active'));
                
                // Add active class to current nav button and page
                navBtn.classList.add('active');
                document.getElementById(pageId).classList.add('active');
                
                // Update specific page content if needed
                if (pageId === 'financePage') {
                    renderTransactions();
                    renderDebts();
                } else if (pageId === 'qrcodePage') {
                    updateQrCodeDisplay();
                } else if (pageId === 'inventoryPage') {
                    renderInventory();
                    renderShoppingList();
                }
            });
        });
        
        // Inventory button functionality
        inventoryButton.addEventListener('click', () => {
            // Remove active class from all nav buttons and pages
            navBtns.forEach(btn => btn.classList.remove('active'));
            pages.forEach(page => page.classList.remove('active'));
            
            // Add active class to inventory page
            document.getElementById('inventoryPage').classList.add('active');
            
            // Update inventory content
            renderInventory();
            renderShoppingList();
        });
        
        // Update time
        function updateTime() {
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            paymentTime.textContent = `${hours}:${minutes}`;
        }
        
        // Generate random transaction ID
        function generateTransactionId() {
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let result = 'TRX-';
            for (let i = 0; i < 6; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return result;
        }
        
        // Render phone history
        function renderPhoneHistory() {
            phoneHistory.innerHTML = '';
            
            if (phoneHistoryData.length === 0) {
                phoneHistory.innerHTML = '<p style="text-align: center; color: #666; padding: 10px;">Belum ada riwayat nomor handphone</p>';
                return;
            }
            
            phoneHistoryData.forEach(item => {
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item';
                historyItem.dataset.phone = item.phone;
                
                historyItem.innerHTML = `
                    <div class="history-phone">${item.phone}</div>
                    <div class="history-name">${item.name || 'Tidak ada nama'}</div>
                `;
                
                historyItem.addEventListener('click', () => {
                    currentPhone = item.phone;
                    updatePhoneDisplay();
                });
                
                phoneHistory.appendChild(historyItem);
            });
        }
        
        // Render transactions
        function renderTransactions() {
            transactionList.innerHTML = '';
            
            // Filter only confirmed transactions
            const confirmedTransactions = transactions.filter(t => t.status === 'confirmed');
            
            if (confirmedTransactions.length === 0) {
                transactionList.innerHTML = '<p style="text-align: center; color: #666; padding: 20px;">Belum ada transaksi yang dikonfirmasi</p>';
                return;
            }
            
            confirmedTransactions.forEach(transaction => {
                const transactionItem = document.createElement('div');
                transactionItem.className = 'transaction-item';
                
                // Hapus status dari tampilan transaksi
                transactionItem.innerHTML = `
                    <div class="transaction-header">
                        <div class="transaction-id">${transaction.id}</div>
                        <div class="transaction-date">${transaction.date}</div>
                    </div>
                    <div class="transaction-details">
                        <div class="transaction-amount">Rp ${formatNumber(transaction.amount)}</div>
                    </div>
                `;
                
                transactionList.appendChild(transactionItem);
            });
        }
        
        // Render debts
        function renderDebts() {
            debtList.innerHTML = '';
            
            if (debts.length === 0) {
                debtList.innerHTML = '<p style="text-align: center; color: #666; padding: 20px;">Belum ada catatan hutang</p>';
                return;
            }
            
            debts.forEach(debt => {
                const debtItem = document.createElement('div');
                debtItem.className = 'debt-item';
                debtItem.dataset.id = debt.id;
                
                debtItem.innerHTML = `
                    <button class="debt-item-menu"><i class="fas fa-ellipsis-v"></i></button>
                    <div class="debt-item-menu-content">
                        <button class="edit-debt" data-id="${debt.id}">Edit</button>
                        <button class="delete-debt" data-id="${debt.id}">Hapus</button>
                    </div>
                    <div class="debt-header">
                        <div>
                            <div class="debt-name">${debt.name}</div>
                            <div class="debt-phone">${debt.phone}</div>
                        </div>
                    </div>
                    <div class="debt-details">
                        <div>
                            <div class="debt-item-name">${debt.item}</div>
                            <div class="debt-amount">Rp ${formatNumber(debt.amount)}</div>
                        </div>
                        <div class="debt-status status-${debt.status}" data-id="${debt.id}">${debt.status === 'paid' ? 'Lunas' : 'Belum Lunas'}</div>
                    </div>
                `;
                
                // Toggle menu
                const menuBtn = debtItem.querySelector('.debt-item-menu');
                const menuContent = debtItem.querySelector('.debt-item-menu-content');
                
                menuBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    document.querySelectorAll('.debt-item-menu-content').forEach(menu => {
                        if (menu !== menuContent) menu.style.display = 'none';
                    });
                    menuContent.style.display = menuContent.style.display === 'block' ? 'none' : 'block';
                });
                
                // Edit debt
                debtItem.querySelector('.edit-debt').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const debtId = e.target.getAttribute('data-id');
                    showPasswordPrompt('edit', debtId);
                    menuContent.style.display = 'none';
                });
                
                // Delete debt
                debtItem.querySelector('.delete-debt').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const debtId = e.target.getAttribute('data-id');
                    if (confirm('Apakah Anda yakin ingin menghapus hutang ini?')) {
                        debts = debts.filter(d => d.id !== debtId);
                        localStorage.setItem('debts', JSON.stringify(debts));
                        renderDebts();
                    }
                    menuContent.style.display = 'none';
                });
                
                // Toggle debt status
                const statusElement = debtItem.querySelector('.debt-status');
                statusElement.addEventListener('click', (e) => {
                    e.stopPropagation();
                    const debtId = e.target.getAttribute('data-id');
                    const debt = debts.find(d => d.id === debtId);
                    
                    if (debt.status === 'pending') {
                        showPasswordPrompt('status', debtId);
                    }
                });
                
                debtList.appendChild(debtItem);
            });
            
            // Close menu when clicking outside
            document.addEventListener('click', () => {
                document.querySelectorAll('.debt-item-menu-content').forEach(menu => {
                    menu.style.display = 'none';
                });
            });
        }
        
        // Update debt name list for autocomplete
        function updateDebtNameList() {
            debtNameList.innerHTML = '';
            const uniqueNames = [...new Set(debts.map(debt => debt.name))];
            
            uniqueNames.forEach(name => {
                const option = document.createElement('option');
                option.value = name;
                debtNameList.appendChild(option);
            });
        }
        
        // Auto-fill phone number when selecting a name
        debtName.addEventListener('input', () => {
            const selectedName = debtName.value;
            const existingDebt = debts.find(debt => debt.name === selectedName);
            
            if (existingDebt) {
                debtPhone.value = existingDebt.phone;
            }
        });
        
        // Show password prompt for debt actions
        function showPasswordPrompt(context, debtId) {
            passwordContext = context;
            currentDebtId = debtId;
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        }
        
        // Update QR code display
        function updateQrCodeDisplay() {
            qrcodeDisplay.innerHTML = '';
            
            if (savedQrisImage) {
                qrcodeDisplay.innerHTML = `
                    <h3>${savedMerchant}</h3>
                    <p> </p>
                    <img src="${savedQrisImage}" alt="QRIS Code" style="max-width: 100%; height: auto; border: 1px solid #ddd; border-radius: 8px;">
                    <p>Scan Kode QRIS di atas untuk pembayaran</p>
                `;
            } else {
                qrcodeDisplay.innerHTML = `
                    <h3>${savedMerchant}</h3>
                    <p> </p>
                    <div style="background-color: #f0f0f0; padding: 20px; display: inline-block; margin: 15px 0;">
                        <i class="fas fa-qrcode" style="font-size: 150px; color: #333;"></i>
                    </div>
                    <p>Upload gambar QRIS melalui pengaturan</p>
                `;
            }
        }
        
        // Render inventory
        function renderInventory() {
            inventoryList.innerHTML = '';
            
            if (inventory.length === 0) {
                inventoryList.innerHTML = '<p style="text-align: center; color: #666; padding: 20px;">Belum ada data inventori</p>';
                return;
            }
            
            inventory.forEach(item => {
                const inventoryItem = document.createElement('div');
                inventoryItem.className = 'inventory-item';
                inventoryItem.dataset.id = item.id;
                
                const stockStatus = item.stock < item.minStock ? 'stock-warning' : 'stock-adequate';
                const stockText = item.stock < item.minStock ? 'Stok Rendah!' : 'Stok Adekuat';
                
                inventoryItem.innerHTML = `
                    <button class="inventory-item-menu"><i class="fas fa-ellipsis-v"></i></button>
                    <div class="inventory-item-menu-content">
                        <button class="edit-item" data-id="${item.id}">Edit</button>
                        <button class="delete-item" data-id="${item.id}">Hapus</button>
                    </div>
                    <div class="inventory-header">
                        <div>
                            <div class="inventory-name">${item.name}</div>
                            <div class="inventory-category">${getCategoryName(item.category)}</div>
                        </div>
                    </div>
                    <div class="inventory-details">
                        <div>
                            <div class="inventory-stock">Stok: ${item.stock}</div>
                            <div class="${stockStatus}">${stockText}</div>
                        </div>
                        <div class="inventory-action">
                            <button class="inventory-btn-small decrease-stock" data-id="${item.id}">-</button>
                            <button class="inventory-btn-small increase-stock" data-id="${item.id}">+</button>
                        </div>
                    </div>
                `;
                
                // Toggle menu
                const menuBtn = inventoryItem.querySelector('.inventory-item-menu');
                const menuContent = inventoryItem.querySelector('.inventory-item-menu-content');
                
                menuBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    document.querySelectorAll('.inventory-item-menu-content').forEach(menu => {
                        if (menu !== menuContent) menu.style.display = 'none';
                    });
                    menuContent.style.display = menuContent.style.display === 'block' ? 'none' : 'block';
                });
                
                // Edit item
                inventoryItem.querySelector('.edit-item').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const item = inventory.find(i => i.id === itemId);
                    
                    if (item) {
                        editItemName.value = item.name;
                        editItemCategory.value = item.category;
                        editItemStock.value = item.stock;
                        editItemMinStock.value = item.minStock;
                        currentItemId = itemId;
                        inventoryEditModal.style.display = 'flex';
                    }
                    menuContent.style.display = 'none';
                });
                
                // Delete item
                inventoryItem.querySelector('.delete-item').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    if (confirm('Apakah Anda yakin ingin menghapus barang ini?')) {
                        inventory = inventory.filter(i => i.id !== itemId);
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                    }
                    menuContent.style.display = 'none';
                });
                
                // Decrease stock
                inventoryItem.querySelector('.decrease-stock').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const itemIndex = inventory.findIndex(i => i.id === itemId);
                    
                    if (itemIndex >= 0 && inventory[itemIndex].stock > 0) {
                        inventory[itemIndex].stock--;
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                    }
                });
                
                // Increase stock
                inventoryItem.querySelector('.increase-stock').addEventListener('click', (e) => {
                    e.stopPropagation();
                    const itemId = e.target.getAttribute('data-id');
                    const itemIndex = inventory.findIndex(i => i.id === itemId);
                    
                    if (itemIndex >= 0) {
                        inventory[itemIndex].stock++;
                        localStorage.setItem('inventory', JSON.stringify(inventory));
                        renderInventory();
                        renderShoppingList();
                    }
                });
                
                inventoryList.appendChild(inventoryItem);
            });
            
            // Close menu when clicking outside
            document.addEventListener('click', () => {
                document.querySelectorAll('.inventory-item-menu-content').forEach(menu => {
                    menu.style.display = 'none';
                });
            });
        }
        
        // Render shopping list
        function renderShoppingList() {
            shoppingList.innerHTML = '';
            
            const lowStockItems = inventory.filter(item => item.stock < item.minStock);
            
            if (lowStockItems.length === 0) {
                shoppingList.innerHTML = '<p style="text-align: center; color: #666; padding: 20px;">Tidak ada barang yang perlu dibeli</p>';
                return;
            }
            
            shoppingList.innerHTML = '<h3>Daftar Belanja (Stok Rendah)</h3>';
            
            lowStockItems.forEach(item => {
                const shoppingItem = document.createElement('div');
                shoppingItem.className = 'inventory-item';
                
                shoppingItem.innerHTML = `
                    <div class="inventory-header">
                        <div>
                            <div class="inventory-name">${item.name}</div>
                            <div class="inventory-category">${getCategoryName(item.category)}</div>
                        </div>
                    </div>
                    <div class="inventory-details">
                        <div>
                            <div class="stock-warning">Stok: ${item.stock} (Min: ${item.minStock})</div>
                            <div>Perlu dibeli: ${item.minStock - item.stock}</div>
                        </div>
                    </div>
                `;
                
                shoppingList.appendChild(shoppingItem);
            });
        }
        
        // Get category name
        function getCategoryName(category) {
            const categories = {
                'makanan': 'Makanan',
                'minuman': 'Minuman',
                'snack': 'Snack',
                'sembako': 'Sembako',
                'lainnya': 'Lainnya'
            };
            
            return categories[category] || 'Lainnya';
        }
        
        // Event Listeners
        profileBtn.addEventListener('click', () => {
            passwordContext = 'settings';
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        });
        
        financeProfileBtn.addEventListener('click', () => {
            passwordContext = 'settings';
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        });
        
        qrcodeProfileBtn.addEventListener('click', () => {
            passwordContext = 'settings';
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        });
        
        pulsaProfileBtn.addEventListener('click', () => {
            passwordContext = 'settings';
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        });
        
        inventoryProfileBtn.addEventListener('click', () => {
            passwordContext = 'settings';
            passwordModal.style.display = 'flex';
            passwordInput.value = '';
            passwordError.style.display = 'none';
            passwordInput.focus();
        });
        
        closePassword.addEventListener('click', () => {
            passwordModal.style.display = 'none';
        });
        
        submitPassword.addEventListener('click', () => {
            if (passwordInput.value === appPassword) {
                passwordModal.style.display = 'none';
                
                if (passwordContext === 'status') {
                    // Update debt status
                    const debtIndex = debts.findIndex(d => d.id === currentDebtId);
                    if (debtIndex >= 0) {
                        debts[debtIndex].status = 'paid';
                        localStorage.setItem('debts', JSON.stringify(debts));
                        renderDebts();
                    }
                } else if (passwordContext === 'edit') {
                    // Edit debt
                    const debt = debts.find(d => d.id === currentDebtId);
                    if (debt) {
                        editDebtName.value = debt.name;
                        editDebtPhone.value = debt.phone;
                        editDebtItem.value = debt.item;
                        editDebtAmount.value = debt.amount;
                        debtEditModal.style.display = 'flex';
                    }
                } else {
                    // Open settings
                    settingsModal.style.display = 'flex';
                }
            } else {
                passwordError.style.display = 'block';
                passwordInput.value = '';
                setTimeout(() => {
                    passwordError.style.display = 'none';
                }, 2000);
            }
        });
        
        closeSettings.addEventListener('click', () => {
            settingsModal.style.display = 'none';
        });
        
        closeEditDebtModal.addEventListener('click', () => {
            debtEditModal.style.display = 'none';
        });
        
        closeEditItemModal.addEventListener('click', () => {
            inventoryEditModal.style.display = 'none';
        });
        
        closePaymentModal.addEventListener('click', () => {
            paymentModal.style.display = 'none';
        });
        
        closePulsaModal.addEventListener('click', () => {
            pulsaModal.style.display = 'none';
        });
        
        // Close modal when clicking outside
        passwordModal.addEventListener('click', (e) => {
            if (e.target === passwordModal) {
                passwordModal.style.display = 'none';
            }
        });
        
        settingsModal.addEventListener('click', (e) => {
            if (e.target === settingsModal) {
                settingsModal.style.display = 'none';
            }
        });
        
        debtEditModal.addEventListener('click', (e) => {
            if (e.target === debtEditModal) {
                debtEditModal.style.display = 'none';
            }
        });
        
        inventoryEditModal.addEventListener('click', (e) => {
            if (e.target === inventoryEditModal) {
                inventoryEditModal.style.display = 'none';
            }
        });
        
        paymentModal.addEventListener('click', (e) => {
            if (e.target === paymentModal) {
                paymentModal.style.display = 'none';
            }
        });
        
        pulsaModal.addEventListener('click', (e) => {
            if (e.target === pulsaModal) {
                pulsaModal.style.display = 'none';
            }
        });
        
        // Numeric keypad for amount
        document.querySelectorAll('#homePage .key[data-value]').forEach(key => {
            key.addEventListener('click', () => {
                const value = key.getAttribute('data-value');
                
                if (currentAmount === '0') {
                    currentAmount = value;
                } else {
                    currentAmount += value;
                }
                
                updateAmountDisplay();
            });
        });
        
        // Numeric keypad for phone
        document.querySelectorAll('#pulsaPage .key[data-value]').forEach(key => {
            key.addEventListener('click', () => {
                const value = key.getAttribute('data-value');
                
                if (currentPhone === '0') {
                    currentPhone = value;
                } else {
                    currentPhone += value;
                }
                
                updatePhoneDisplay();
            });
        });
        
        // Backspace button for amount
        backspaceBtn.addEventListener('click', () => {
            if (currentAmount.length > 1) {
                currentAmount = currentAmount.slice(0, -1);
            } else {
                currentAmount = '0';
            }
            
            updateAmountDisplay();
        });
        
        // Backspace button for phone
        pulsaBackspaceBtn.addEventListener('click', () => {
            if (currentPhone.length > 1) {
                currentPhone = currentPhone.slice(0, -1);
            } else {
                currentPhone = '0';
            }
            
            updatePhoneDisplay();
        });
        
        // Clear button for amount
        clearButton.addEventListener('click', () => {
            currentAmount = '0';
            updateAmountDisplay();
        });
        
        // Clear button for phone
        clearPhoneButton.addEventListener('click', () => {
            currentPhone = '0';
            updatePhoneDisplay();
        });
        
        // Save phone button
        savePhoneButton.addEventListener('click', () => {
            if (currentPhone === '0' || currentPhone.length < 10) {
                alert('Harap masukkan nomor handphone yang valid!');
                return;
            }
            
            selectedPhoneNumber = currentPhone;
            phoneName.value = '';
            pulsaModal.style.display = 'flex';
            
            // Reset pulsa options
            pulsaOptions.forEach(option => {
                option.classList.remove('active');
            });
            customAmountGroup.style.display = 'none';
            selectedPulsaAmount = 0;
        });
        
        // Pulsa options
        pulsaOptions.forEach(option => {
            option.addEventListener('click', () => {
                pulsaOptions.forEach(opt => opt.classList.remove('active'));
                option.classList.add('active');
                
                const amount = option.getAttribute('data-amount');
                selectedPulsaAmount = parseInt(amount);
                
                if (amount === '0') {
                    customAmountGroup.style.display = 'block';
                } else {
                    customAmountGroup.style.display = 'none';
                }
            });
        });
        
        // Custom amount input
        customAmount.addEventListener('input', () => {
            selectedPulsaAmount = parseInt(customAmount.value) || 0;
        });
        
        // Confirm pulsa button
        confirmPulsaButton.addEventListener('click', () => {
            if (selectedPulsaAmount <= 0) {
                alert('Harap pilih nominal pulsa!');
                return;
            }
            
            const name = phoneName.value.trim() || 'Tidak ada nama';
            
            // Save to phone history
            const existingIndex = phoneHistoryData.findIndex(item => item.phone === selectedPhoneNumber);
            
            if (existingIndex >= 0) {
                phoneHistoryData[existingIndex].name = name;
            } else {
                phoneHistoryData.push({
                    phone: selectedPhoneNumber,
                    name: name
                });
            }
            
            localStorage.setItem('phoneHistory', JSON.stringify(phoneHistoryData));
            
            // Add transaction (simulate success after 5 seconds)
            const newTransaction = {
                id: generateTransactionId(),
                amount: selectedPulsaAmount,
                date: new Date().toLocaleDateString('id-ID', {
                    day: '2-digit',
                    month: '2-digit',
                    year: 'numeric',
                    hour: '2-digit',
                    minute: '2-digit'
                }),
                status: 'pending',
                type: 'pulsa',
                phone: selectedPhoneNumber
            };
            
            transactions.push(newTransaction);
            localStorage.setItem('transactions', JSON.stringify(transactions));
            
            pulsaModal.style.display = 'none';
            
            // Simulate confirmation after 5 seconds
            setTimeout(() => {
                const transactionIndex = transactions.findIndex(t => t.id === newTransaction.id);
                if (transactionIndex >= 0) {
                    transactions[transactionIndex].status = 'confirmed';
                    localStorage.setItem('transactions', JSON.stringify(transactions));
                    
                    // If we're on the finance page, update it
                    if (document.getElementById('financePage').classList.contains('active')) {
                        renderTransactions();
                    }
                }
            }, 5000);
            
            renderPhoneHistory();
        });
        
        // Add debt button
        addDebtButton.addEventListener('click', () => {
            const name = debtName.value.trim();
            const phone = debtPhone.value.trim();
            const itemName = debtItem.value.trim();
            const amount = parseInt(debtAmount.value);
            
            if (!name || !phone || !itemName || !amount) {
                alert('Harap isi semua field!');
                return;
            }
            
            const newDebt = {
                id: Date.now().toString(),
                name,
                phone,
                item: itemName,
                amount,
                status: 'pending',
                date: new Date().toLocaleDateString('id-ID')
            };
            
            debts.push(newDebt);
            localStorage.setItem('debts', JSON.stringify(debts));
            
            // Clear form
            debtName.value = '';
            debtPhone.value = '';
            debtItem.value = '';
            debtAmount.value = '';
            
            renderDebts();
            updateDebtNameList();
        });
        
        // Save edited debt
        saveEditDebtButton.addEventListener('click', () => {
            const name = editDebtName.value.trim();
            const phone = editDebtPhone.value.trim();
            const itemName = editDebtItem.value.trim();
            const amount = parseInt(editDebtAmount.value);
            
            if (!name || !phone || !itemName || !amount) {
                alert('Harap isi semua field!');
                return;
            }
            
            const debtIndex = debts.findIndex(d => d.id === currentDebtId);
            if (debtIndex >= 0) {
                debts[debtIndex].name = name;
                debts[debtIndex].phone = phone;
                debts[debtIndex].item = itemName;
                debts[debtIndex].amount = amount;
                
                localStorage.setItem('debts', JSON.stringify(debts));
                renderDebts();
                updateDebtNameList();
                
                debtEditModal.style.display = 'none';
            }
        });
        
        // Add item button
        addItemButton.addEventListener('click', () => {
            const name = itemName.value.trim();
            const category = itemCategory.value;
            const stock = parseInt(itemStock.value) || 0;
            const minStock = parseInt(itemMinStock.value) || 0;
            
            if (!name || !category) {
                alert('Harap isi nama dan kategori barang!');
                return;
            }
            
            const newItem = {
                id: Date.now().toString(),
                name,
                category,
                stock,
                minStock
            };
            
            inventory.push(newItem);
            localStorage.setItem('inventory', JSON.stringify(inventory));
            
            // Clear form
            itemName.value = '';
            itemCategory.value = 'makanan';
            itemStock.value = '';
            itemMinStock.value = '';
            
            renderInventory();
            renderShoppingList();
        });
        
        // Save edited item
        saveEditItemButton.addEventListener('click', () => {
            const name = editItemName.value.trim();
            const category = editItemCategory.value;
            const stock = parseInt(editItemStock.value) || 0;
            const minStock = parseInt(editItemMinStock.value) || 0;
            
            if (!name || !category) {
                alert('Harap isi nama dan kategori barang!');
                return;
            }
            
            const itemIndex = inventory.findIndex(i => i.id === currentItemId);
            if (itemIndex >= 0) {
                inventory[itemIndex].name = name;
                inventory[itemIndex].category = category;
                inventory[itemIndex].stock = stock;
                inventory[itemIndex].minStock = minStock;
                
                localStorage.setItem('inventory', JSON.stringify(inventory));
                renderInventory();
                renderShoppingList();
                
                inventoryEditModal.style.display = 'none';
            }
        });
        
        // Convert button - FIXED API QRIS
        convertButton.addEventListener('click', async () => {
            if (currentAmount === '0') {
                alert('Harap masukkan nominal terlebih dahulu!');
                return;
            }
            
            loading.style.display = 'block';
            
            try {
                // Use the saved QRIS static code
                const qrisData = savedQrisStaticCode;
                
                if (!qrisData) {
                    alert('Harap masukkan Kode QRIS statis di pengaturan terlebih dahulu!');
                    loading.style.display = 'none';
                    return;
                }
                
                // Encode QRIS data for URL
                const encodedQris = encodeURIComponent(qrisData);
                
                // Make API request to API 2
                const response = await fetch(`https://cekid-ariepulsa.my.id/api/?qris_data=${encodedQris}&nominal=${currentAmount}`);
                const data = await response.json();
                
                if (data.status === 'success') {
                    // Show payment modal with QR code
                    paymentAmount.textContent = `Rp ${formatNumber(currentAmount)}`;
                    paymentQr.src = data.link_qris;
                    paymentMerchant.textContent = savedMerchant;
                    currentQrUrl = data.link_qris;
                    
                    // Update transaction details
                    updateTime();
                    transactionId.textContent = generateTransactionId();
                    
                    // Add to transactions
                    const newTransaction = {
                        id: transactionId.textContent,
                        amount: parseInt(currentAmount),
                        date: new Date().toLocaleDateString('id-ID', {
                            day: '2-digit',
                            month: '2-digit',
                            year: 'numeric',
                            hour: '2-digit',
                            minute: '2-digit'
                        }),
                        status: 'pending',
                        type: 'qris'
                    };
                    
                    transactions.push(newTransaction);
                    localStorage.setItem('transactions', JSON.stringify(transactions));
                    
                    paymentModal.style.display = 'flex';
                    
                    // Simulate confirmation after 5 seconds
                    setTimeout(() => {
                        const transactionIndex = transactions.findIndex(t => t.id === newTransaction.id);
                        if (transactionIndex >= 0) {
                            transactions[transactionIndex].status = 'confirmed';
                            localStorage.setItem('transactions', JSON.stringify(transactions));
                            
                            // If we're on the finance page, update it
                            if (document.getElementById('financePage').classList.contains('active')) {
                                renderTransactions();
                            }
                        }
                    }, 5000);
                } else {
                    alert('API mengembalikan status error: ' + (data.message || 'Unknown error'));
                }
            } catch (error) {
                alert('Terjadi kesalahan: ' + error.message);
            } finally {
                loading.style.display = 'none';
            }
        });
        
        // Close payment modal
        closePayment.addEventListener('click', () => {
            paymentModal.style.display = 'none';
            // Refresh transaction list when closing payment modal
            if (document.getElementById('financePage').classList.contains('active')) {
                renderTransactions();
            }
        });
        
        // QRIS upload handling
        qrisDropArea.addEventListener('click', () => {
            qrisInput.click();
        });
        
        qrisInput.addEventListener('change', (e) => {
            if (e.target.files.length) {
                const file = e.target.files[0];
                const reader = new FileReader();
                
                reader.onload = function(event) {
                    savedQrisImage = event.target.result;
                    localStorage.setItem('qrisImage', savedQrisImage);
                    
                    // Show preview
                    qrisPreview.style.display = 'block';
                    qrisPreviewImg.src = savedQrisImage;
                    
                    // Update QR code display if on that page
                    if (document.getElementById('qrcodePage').classList.contains('active')) {
                        updateQrCodeDisplay();
                    }
                };
                
                reader.readAsDataURL(file);
            }
        });
        
        // Reset QRIS image
        resetQrisButton.addEventListener('click', () => {
            if (confirm('Apakah Anda yakin ingin menghapus gambar QRIS?')) {
                savedQrisImage = '';
                localStorage.removeItem('qrisImage');
                qrisPreview.style.display = 'none';
                
                if (document.getElementById('qrcodePage').classList.contains('active')) {
                    updateQrCodeDisplay();
                }
            }
        });
        
        // Change password
        changePasswordButton.addEventListener('click', () => {
            const current = currentPassword.value;
            const newPass = newPassword.value;
            const confirmPass = confirmPassword.value;
            
            if (current !== appPassword) {
                alert('Sandi saat ini tidak benar!');
                return;
            }
            
            if (newPass.length !== 6) {
                alert('Sandi baru harus 6 digit!');
                return;
            }
            
            if (newPass !== confirmPass) {
                alert('Konfirmasi sandi tidak sesuai!');
                return;
            }
            
            appPassword = newPass;
            localStorage.setItem('appPassword', appPassword);
            
            alert('Sandi berhasil diubah!');
            
            currentPassword.value = '';
            newPassword.value = '';
            confirmPassword.value = '';
        });
        
        // Save settings
        saveButton.addEventListener('click', () => {
            const merchant = merchantName.value.trim();
            const headerTitleText = headerTitleInput.value.trim();
            const headerSubtitleText = headerSubtitleInput.value.trim();
            const footerTextContent = footerTextInput.value.trim();
            const qrisCode = qrisStaticCode.value.trim();
            
            if (!merchant) {
                alert('Harap masukkan nama merchant!');
                return;
            }
            
            if (!headerTitleText) {
                alert('Harap masukkan judul halaman!');
                return;
            }
            
            if (!headerSubtitleText) {
                alert('Harap masukkan subjudul halaman!');
                return;
            }
            
            if (!footerTextContent) {
                alert('Harap masukkan teks footer!');
                return;
            }
            
            if (!qrisCode) {
                alert('Harap masukkan Kode QRIS statis!');
                return;
            }
            
            savedMerchant = merchant;
            savedHeaderTitle = headerTitleText;
            savedHeaderSubtitle = headerSubtitleText;
            savedFooterText = footerTextContent;
            savedQrisStaticCode = qrisCode;
            
            localStorage.setItem('merchantName', merchant);
            localStorage.setItem('headerTitle', headerTitleText);
            localStorage.setItem('headerSubtitle', headerSubtitleText);
            localStorage.setItem('footerText', footerTextContent);
            localStorage.setItem('qrisStaticCode', qrisCode);
            
            // Update UI with new values
            paymentMerchant.textContent = merchant;
            headerTitle.textContent = headerTitleText;
            headerSubtitle.textContent = headerSubtitleText;
            footerText.textContent = footerTextContent;
            paymentTitle.textContent = headerTitleText;
            
            settingsModal.style.display = 'none';
        });
        
        // Helper functions
        function updateAmountDisplay() {
            // Format the amount with thousand separators
            const formattedAmount = formatNumber(currentAmount);
            amountDisplay.textContent = `Rp ${formattedAmount}`;
        }
        
        function updatePhoneDisplay() {
            phoneDisplay.textContent = currentPhone;
        }
        
        function formatNumber(num) {
            return parseInt(num).toLocaleString('id-ID');
        }
        
        // Initialize the app
        initializeApp();
        
        // Initialize amount display and time
        updateAmountDisplay();
        updatePhoneDisplay();
        updateTime();
        setInterval(updateTime, 60000); // Update time every minute
    </script>
</body>
</html>
