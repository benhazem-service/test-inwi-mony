
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام تسيير الفواتير</title>
    <style>
        :root {
            --bg-color: #f4f6f8;
            --white: #ffffff;
            
            /* ألوان الحالات */
            --orange: #e65100;
            --orange-light: #fff3e0;
            --orange-border: #ffb74d;

            --green: #2e7d32;
            --green-light: #e8f5e9;
            --green-border: #81c784;

            --red: #c62828;
            --red-light: #ffebee;
            --red-border: #ef5350;

            --blue: #1565c0;
            --blue-light: #e3f2fd;
            --blue-border: #64b5f6;

            --gray: #616161;
            --gray-light: #f3f3f3;
            --gray-border: #bdbdbd;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--bg-color);
            color: #333;
        }

        .hidden { display: none !important; }

        .auth-wrapper {
            max-width: 420px;
            margin: 40px auto;
            background: var(--white);
            border-radius: 14px;
            padding: 22px;
            box-shadow: 0 8px 18px rgba(0,0,0,0.06);
        }

        .auth-title {
            font-weight: bold;
            font-size: 1.2rem;
            margin-bottom: 16px;
            text-align: center;
        }

        .auth-tabs {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 14px;
        }

        .auth-tab {
            padding: 10px;
            border-radius: 10px;
            cursor: pointer;
            border: 1px solid #ddd;
            background: #fff;
            font-weight: bold;
        }

        .auth-tab.active {
            border-color: var(--blue);
            color: var(--blue);
            background: var(--blue-light);
        }

        .auth-actions {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 12px;
        }

        .auth-actions button {
            padding: 12px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            background: #333;
            color: white;
        }

        .auth-actions button:hover { background: #000; }

        .auth-note {
            text-align: center;
            font-size: 0.85rem;
            color: #666;
            margin-top: 10px;
            min-height: 20px;
        }

        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
            margin-bottom: 12px;
        }

        .app-header .user-email {
            font-size: 0.9rem;
            color: #666;
            direction: ltr;
        }

        .logout-btn {
            padding: 10px 14px;
            border: 1px solid #ddd;
            background: #fff;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
        }

        .logout-btn:hover { background: #f5f5f5; }

        .settings-btn {
            padding: 10px 14px;
            border: 1px solid #ddd;
            background: #fff;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
        }

        .settings-btn:hover { background: #f5f5f5; }

        #settingsView { display: none; }
        #settingsView.active { display: block; }

        .settings-page {
            background: var(--white);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
        }

        .settings-section {
            border: 1px solid rgba(0,0,0,0.08);
            border-radius: 12px;
            padding: 14px;
            margin-top: 12px;
        }

        .settings-section-title {
            font-weight: bold;
            margin-bottom: 10px;
            color: #444;
        }

        .settings-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 10px;
        }

        .settings-actions button {
            padding: 10px 14px;
            border-radius: 10px;
            border: 1px solid #ddd;
            background: #fff;
            cursor: pointer;
            font-weight: bold;
        }

        .settings-actions button:hover { background: #f5f5f5; }

        .modal-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.45);
            display: none;
            align-items: center;
            justify-content: center;
            padding: 20px;
            z-index: 9999;
        }

        .modal-overlay.active { display: flex; }

        .modal {
            width: 100%;
            max-width: 420px;
            background: #fff;
            border-radius: 14px;
            padding: 18px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.12);
        }

        .modal-title {
            font-weight: bold;
            margin-bottom: 10px;
        }

        .modal-actions {
            display: flex;
            gap: 10px;
            margin-top: 12px;
        }

        .modal-actions button {
            flex: 1;
            padding: 10px 12px;
            border-radius: 10px;
            border: 1px solid #ddd;
            background: #fff;
            cursor: pointer;
            font-weight: bold;
        }

        .modal-actions button.primary {
            background: #333;
            color: #fff;
            border-color: #333;
        }

        .modal-actions button.primary:hover { background: #000; }

        .modal-msg {
            margin-top: 8px;
            min-height: 18px;
            font-size: 0.85rem;
            color: #666;
            text-align: center;
        }

        .container {
            max-width: 950px;
            margin: 0 auto;
        }

        /* --- التاريخ --- */
        .date-control {
            margin-bottom: 25px;
            text-align: center;
        }
        .date-control input {
            padding: 8px 25px;
            border-radius: 20px;
            border: 1px solid #ccc;
            font-family: inherit;
            outline: none;
            font-weight: bold;
            font-size: 1.1rem;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        /* --- شريط الفلترة والأزرار (Filters) --- */
        .filters-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 12px;
            margin-bottom: 25px;
        }

        .filter-card {
            background: var(--white);
            padding: 15px 10px;
            border-radius: 12px;
            cursor: pointer;
            text-align: center;
            border: 2px solid transparent;
            transition: all 0.2s ease;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100px;
        }

        .filter-card:hover { transform: translateY(-3px); }

        .filter-card h3 {
            margin: 0 0 8px 0;
            font-size: 0.85rem;
            font-weight: bold;
            color: #666;
        }

        .filter-card .value {
            font-size: 1.5rem;
            font-weight: bold;
            direction: ltr;
        }

        .filter-card .sub-info {
            font-size: 0.75rem;
            margin-top: 4px;
            opacity: 0.7;
        }

        /* ألوان الفلاتر */
        /* الكل */
        .filter-card.blue { border-bottom: 4px solid var(--blue); }
        .filter-card.blue .value { color: var(--blue); }
        .filter-card.blue.active { background-color: var(--blue); border-color: var(--blue); }
        .filter-card.blue.active h3, .filter-card.blue.active .value, .filter-card.blue.active .sub-info { color: white; }

        /* برتقالي (للمراجعة) */
        .filter-card.orange { border-bottom: 4px solid var(--orange); }
        .filter-card.orange .value { color: var(--orange); }
        .filter-card.orange.active { background-color: var(--orange); border-color: var(--orange); }
        .filter-card.orange.active h3, .filter-card.orange.active .value, .filter-card.orange.active .sub-info { color: white; }

        /* أخضر (للاستخلاص) */
        .filter-card.green { border-bottom: 4px solid var(--green); }
        .filter-card.green .value { color: var(--green); }
        .filter-card.green.active { background-color: var(--green); border-color: var(--green); }
        .filter-card.green.active h3, .filter-card.green.active .value, .filter-card.green.active .sub-info { color: white; }

        /* أحمر (مستخلص) */
        .filter-card.red { border-bottom: 4px solid var(--red); }
        .filter-card.red .value { color: var(--red); }
        .filter-card.red.active { background-color: var(--red); border-color: var(--red); }
        .filter-card.red.active h3, .filter-card.red.active .value, .filter-card.red.active .sub-info { color: white; }

        /* رمادي (الدفع) */
        .filter-card.gray { border-bottom: 4px solid var(--gray); }
        .filter-card.gray .value { color: var(--gray); }
        .filter-card.gray.active { background-color: var(--gray); border-color: var(--gray); }
        .filter-card.gray.active h3, .filter-card.gray.active .value, .filter-card.gray.active .sub-info { color: white; }

        /* صندوق (Caisse) */
        .filter-card.caisse { border-bottom: 4px solid #3949ab; }
        .filter-card.caisse .value { color: #3949ab; }


        #paymentView { display: none; }
        #paymentView.active { display: block; }
        #mainView.hidden { display: none; }

        .payment-page {
            background: var(--white);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
        }

        .payment-page-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 12px;
        }

        .payment-page-header .title {
            font-weight: bold;
            color: var(--gray);
        }

        button.back-btn {
            border: 1px solid #ddd;
            background: #fff;
            padding: 10px 14px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
        }

        button.back-btn:hover { background: #f5f5f5; }

        .payment-form button {
            padding: 12px 18px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-weight: bold;
            background: var(--gray);
            color: white;
        }
        .payment-form button:hover { background: #424242; }

        .type-manager {
            border: 1px solid rgba(0,0,0,0.08);
            border-radius: 12px;
            padding: 12px;
            margin-bottom: 12px;
        }

        .type-manager-title {
            font-weight: bold;
            margin-bottom: 8px;
            color: #444;
        }

        .type-list {
            display: flex;
            flex-direction: column;
            gap: 8px;
            margin-top: 10px;
        }

        .type-item {
            background: #fff;
            border: 1px solid rgba(0,0,0,0.08);
            border-radius: 10px;
            padding: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
        }

        .type-item .name { font-weight: bold; }

        .type-item .actions { display: flex; gap: 8px; }

        .type-item .actions button {
            border: 1px solid #ddd;
            background: #fff;
            border-radius: 8px;
            padding: 6px 10px;
            cursor: pointer;
            font-weight: bold;
            color: #555;
        }

        .type-item .actions button:hover { background: #f5f5f5; }

        .type-item .actions button.danger:hover { color: red; border-color: red; background: #fff; }
        .payment-history {
            margin-top: 10px;
            max-height: 170px;
            overflow: auto;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .payment-history-item {
            background: #fff;
            border: 1px solid rgba(0,0,0,0.08);
            border-radius: 10px;
            padding: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
        }

        .payment-history-item .delete {
            border: 1px solid #ddd;
            background: #fff;
            border-radius: 8px;
            padding: 6px 10px;
            cursor: pointer;
            font-weight: bold;
            color: #777;
        }

        .payment-history-item .delete:hover { color: red; border-color: red; }
        .payment-history-item .left {
            display: flex;
            flex-direction: column;
            gap: 2px;
            text-align: right;
        }
        .payment-history-item .left strong { font-size: 0.95rem; }
        .payment-history-item .left span { font-size: 0.8rem; opacity: 0.75; }
        .payment-history-item .amt { font-weight: bold; direction: ltr; color: var(--gray); }


        /* --- نموذج الإدخال --- */
        .input-section {
            background: var(--white);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            margin-bottom: 25px;
        }

        .form-row {
            display: flex;
            gap: 10px;
            align-items: center;
            flex-wrap: wrap;
        }

        input[type="text"], input[type="number"], input[type="password"], input[type="email"] {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            outline: none;
            flex: 1;
            font-size: 1rem;
        }

        .checkbox-wrapper {
            display: flex;
            align-items: center;
            gap: 8px;
            background: #eee;
            padding: 10px 15px;
            border-radius: 8px;
            cursor: pointer;
            user-select: none;
        }
        .checkbox-wrapper input { transform: scale(1.3); }

        button#addBtn {
            padding: 12px 25px;
            background-color: #333;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.3s;
        }
        button#addBtn:hover { background-color: #000; }

        /* --- القائمة --- */
        .records-list {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .record-item {
            background: white;
            padding: 15px;
            border-radius: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.02);
            border-right: 6px solid #ccc;
            transition: all 0.3s ease;
            animation: fadeIn 0.3s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .record-info strong { display: block; font-size: 1.1rem; }
        .record-info span { font-size: 0.85rem; opacity: 0.7; }
        .record-info .date-tag { font-size: 0.75rem; color: #888; margin-right: 5px; background: #eee; padding: 2px 6px; border-radius: 4px; }

        /* وسم الوقت */
        .record-info .time-tag {
            font-size: 0.75rem;
            color: #666;
            margin-right: 6px;
            opacity: 0.9;
        }

        .record-amount { font-weight: bold; font-size: 1.3rem; margin-left: 15px; direction: ltr; }

        .actions { display: flex; gap: 8px; }
        
        button.action-btn {
            border: none;
            padding: 8px 12px;
            border-radius: 6px;
            cursor: pointer;
            transition: 0.2s;
        }

        /* تنسيق الحالات في القائمة */
        .status-pending { border-right-color: var(--orange); background: var(--orange-light); }
        .status-pending .record-amount { color: var(--orange); }

        .status-checked { border-right-color: var(--green); background: var(--green-light); }
        .status-checked .record-amount { color: var(--green); }

        .status-paid { border-right-color: var(--red); background: var(--red-light); opacity: 0.8; }
        .status-paid .record-amount { color: var(--red); text-decoration: line-through; }

        .status-payment { border-right-color: var(--gray); background: var(--gray-light); }
        .status-payment .record-amount { color: var(--gray); }

        /* الأزرار */
        .check-btn { background: #fff; border: 1px solid #ccc; }
        .check-btn:hover { background: #eee; }
        .check-btn.is-checked { background: var(--green); color: white; border-color: var(--green); }

        .pay-btn { background: #333; color: white; }
        .pay-btn:hover { background: var(--red); }
        .pay-btn:disabled { background: #ccc; cursor: not-allowed; }

        .delete-btn { background: transparent; border: 1px solid #ccc; color: #777; }
        .delete-btn:hover { color: red; border-color: red; }

        @media (max-width: 600px) {
            .filters-container { grid-template-columns: 1fr 1fr; }
        }
    </style>
</head>
<body>

<div id="authView" class="hidden">
    <div class="auth-wrapper">
        <div class="auth-title">تسجيل الدخول / إنشاء حساب</div>
        <div class="auth-tabs">
            <button class="auth-tab active" id="tab-login" type="button" onclick="showAuthMode('login')">تسجيل الدخول</button>
            <button class="auth-tab" id="tab-register" type="button" onclick="showAuthMode('register')">تسجيل حساب</button>
        </div>

        <div class="form-row" style="margin-bottom: 10px;">
            <input type="email" id="authEmail" placeholder="البريد الإلكتروني" autocomplete="email" style="direction:ltr;">
        </div>
        <div class="form-row" style="margin-bottom: 10px;">
            <input type="password" id="authPassword" placeholder="كلمة المرور" autocomplete="current-password" style="direction:ltr;">
        </div>

        <div class="form-row hidden" id="authConfirmRow" style="margin-bottom: 10px;">
            <input type="password" id="authPasswordConfirm" placeholder="تأكيد كلمة المرور" autocomplete="new-password" style="direction:ltr;">
        </div>

        <label class="checkbox-wrapper" style="justify-content:center; margin-bottom: 8px;">
            <input type="checkbox" id="showAuthPassword" onclick="toggleAuthPasswordVisibility()">
            <span>إظهار كلمة المرور</span>
        </label>

        <div style="text-align:center; margin-top: 6px;">
            <button type="button" id="resetPasswordBtn" onclick="resetPassword()" style="border:none; background:transparent; color: var(--blue); cursor:pointer; font-weight:bold;">نسيت كلمة المرور؟</button>
        </div>

        <label class="checkbox-wrapper" style="justify-content:center;">
            <input type="checkbox" id="rememberMe">
            <span>حفظ معلومات الدخول (تذكّرني)</span>
        </label>

        <div class="auth-actions">
            <button id="authSubmitBtn" type="button" onclick="submitAuth()">دخول</button>
        </div>

        <div class="auth-note" id="authMsg"></div>
    </div>
</div>

<div id="appView" class="hidden">

<div class="container">

    <div class="app-header">
        <div class="user-email" id="userEmail"></div>
        <div style="display:flex; gap:10px;">
            <button class="settings-btn" type="button" onclick="openSettings()">إعدادات</button>
        </div>
    </div>

    <div id="mainView">
        <div class="date-control">
            <div style="display: flex; justify-content: center; align-items: center; gap: 10px; margin-bottom: 10px;">
                <button type="button" onclick="changeDate(-1)" style="border: 1px solid #ddd; background: #fff; border-radius: 50%; width: 30px; height: 30px; font-weight: bold; cursor: pointer;">‹</button>
                <input type="date" id="currentDate">
                <button type="button" onclick="changeDate(1)" style="border: 1px solid #ddd; background: #fff; border-radius: 50%; width: 30px; height: 30px; font-weight: bold; cursor: pointer;">›</button>
            </div>
        </div>
        <!--
        <div class="date-control">
            <input type="date" id="currentDate">
        </div>

        <!-- الخزنة والصندوق -->
        <div class="filters-container" style="grid-template-columns: 1fr 1fr; margin-bottom: 15px;">
            <div class="filter-card" onclick="openFundPage()" id="fund-card" style="border-bottom: 4px solid #9c27b0; position: relative; cursor: pointer;">
                <h3>الخزنة</h3>
                <div class="value" id="val-fund" style="color: #9c27b0;">0.00</div>
                <button id="add-to-fund-btn" onclick="event.stopPropagation(); addFundPrompt()" style="position: absolute; top: 5px; left: 5px; background: #9c27b0; color: white; border: none; border-radius: 50%; width: 24px; height: 24px; font-size: 18px; cursor: pointer; display: flex; align-items: center; justify-content: center;">+</button>
                <div class="sub-info" id="count-fund">الرصيد الحالي</div>
            </div>
            <div class="filter-card caisse" onclick="openSafePage()" id="safe-card" style="position: relative; cursor: pointer;">
                <h3>الصندوق</h3>
                <div class="value" id="val-safe">0.00</div>
                <button onclick="event.stopPropagation(); addSafePrompt('addition')" style="position: absolute; top: 5px; left: 5px; background: #3949ab; color: white; border: none; border-radius: 50%; width: 24px; height: 24px; font-size: 18px; cursor: pointer; display: flex; align-items: center; justify-content: center;">+</button>
                <button onclick="event.stopPropagation(); addSafePrompt('deduction')" style="position: absolute; top: 5px; right: 5px; background: #c62828; color: white; border: none; border-radius: 50%; width: 24px; height: 24px; font-size: 18px; cursor: pointer; display: flex; align-items: center; justify-content: center;">-</button>
                <div class="sub-info" id="count-safe">الرصيد الحالي</div>
            </div>
        </div>
        <!-- 2. أزرار الفلترة والتفاصيل (Filters) -->
        <div class="filters-container">
            <div class="filter-card blue active" onclick="setFilter('all')" id="filter-all">
                <h3>المجموع (اليوم)</h3>
                <div class="value" id="val-all">0.00</div>
                <div class="sub-info" id="count-all">0 عملية</div>
            </div>

            <!-- البرتقالي (للمراجعة - كل الأيام) -->
            <div class="filter-card orange" onclick="setFilter('pending')" id="filter-pending">
                <h3>الباقي </h3>
                <div class="value" id="val-pending">0.00</div>
                <div class="sub-info" id="count-pending">0 عملية</div>
            </div>

            <!-- الأخضر (جاهز - كل الأيام) -->
            <div class="filter-card green" onclick="setFilter('ready')" id="filter-ready">
                <h3>خالص</h3>
                <div class="value" id="val-ready">0.00</div>
                <div class="sub-info" id="count-ready">0 عملية</div>
            </div>

            <!-- الأحمر (مستخلص - اليوم) -->
            <div class="filter-card red" onclick="setFilter('paid')" id="filter-paid">
                <h3>مستخلص (اليوم)</h3>
                <div class="value" id="val-paid">0.00</div>
                <div class="sub-info" id="count-paid">0 عملية</div>
            </div>

            <div class="filter-card gray" onclick="openPaymentPage()" id="filter-payment">
                <h3>الدفع</h3>
                <div class="value" id="val-payment">0.00</div>
                <div class="sub-info" id="count-payment">0 عملية</div>
            </div>

        </div>

        <!-- الإدخال -->
        <div class="input-section">
            <div class="form-row">
                <input type="text" id="serviceType" list="servicesList" placeholder="نوع الخدمة (كتابة عربية)" autocomplete="off">
                <datalist id="servicesList"></datalist>
                
                <input type="number" id="amount" placeholder="المبلغ" step="0.01">
                
                <label class="checkbox-wrapper">
                    <input type="checkbox" id="initialCheck">
                    <span>وضع علامة</span>
                </label>
                
                <button id="addBtn">إضافة</button>
            </div>
        </div>

        <!-- القائمة -->
        <div class="records-list" id="recordsList"></div>
    </div>

    <div id="fundView" class="hidden">
        <div class="payment-page">
            <div class="payment-page-header">
                <button class="back-btn" type="button" onclick="closeFundPage()">رجوع</button>
                <div class="title" style="color: #9c27b0;">سجل الخزنة</div>
                <div style="min-width: 1px;"></div>
            </div>

            <div class="date-control" style="margin-bottom: 15px;">
                 <div style="display: flex; justify-content: center; align-items: center; gap: 10px;">
                    <button type="button" onclick="changeDate(-1, 'fund')" style="border: 1px solid #ddd; background: #fff; border-radius: 50%; width: 30px; height: 30px; font-weight: bold; cursor: pointer;">‹</button>
                    <input type="date" id="fundDate">
                    <button type="button" onclick="changeDate(1, 'fund')" style="border: 1px solid #ddd; background: #fff; border-radius: 50%; width: 30px; height: 30px; font-weight: bold; cursor: pointer;">›</button>
                </div>
            </div>

            <div style="text-align:center; margin-bottom: 12px;">
                <div style="font-size: 0.9rem; color:#666;">رصيد الخزنة الحالي</div>
                <div style="font-size: 1.8rem; font-weight:bold; color: #9c27b0; direction:ltr;" id="val-fund-page">0.00</div>
            </div>

            <div class="payment-history" id="fundHistory" style="max-height: 400px;"></div>
        </div>
    </div>

    <div id="safeView" class="hidden">
        <div class="payment-page">
            <div class="payment-page-header">
                <button class="back-btn" type="button" onclick="closeSafePage()">رجوع</button>
                <div class="title" style="color: #3949ab;">سجل الصندوق</div>
                <div style="min-width: 1px;"></div>
            </div>

            <div class="date-control" style="margin-bottom: 15px;">
                 <div style="display: flex; justify-content: center; align-items: center; gap: 10px;">
                    <button type="button" onclick="changeDate(-1, 'safe')" style="border: 1px solid #ddd; background: #fff; border-radius: 50%; width: 30px; height: 30px; font-weight: bold; cursor: pointer;">‹</button>
                    <input type="date" id="safeDate">
                    <button type="button" onclick="changeDate(1, 'safe')" style="border: 1px solid #ddd; background: #fff; border-radius: 50%; width: 30px; height: 30px; font-weight: bold; cursor: pointer;">›</button>
                </div>
            </div>

            <div style="text-align:center; margin-bottom: 12px;">
                <div style="font-size: 0.9rem; color:#666;">رصيد الصندوق الحالي</div>
                <div style="font-size: 1.8rem; font-weight:bold; color: #3949ab; direction:ltr;" id="val-safe-page">0.00</div>
            </div>

            <div class="payment-history" id="safeHistory" style="max-height: 400px;"></div>
        </div>
    </div>


    <div id="paymentView">
        <div class="payment-page">
            <div class="payment-page-header">
                <button class="back-btn" type="button" onclick="closePaymentPage()">رجوع</button>
                <div class="title">الدفع</div>
                <div style="min-width: 1px;"></div>
            </div>

            <div class="date-control" style="margin-bottom: 15px;">
                <input type="date" id="paymentDate">
            </div>

            <div style="text-align:center; margin-bottom: 12px;">
                <div style="font-size: 0.9rem; color:#666;">المجموع (اليوم)</div>
                <div style="font-size: 1.8rem; font-weight:bold; color: var(--gray); direction:ltr;" id="val-payment-page">0.00</div>
            </div>

            <div class="payment-form">
                <div class="form-row">
                    <select id="paymentType" style="flex: 1; padding: 12px; border: 1px solid #ddd; border-radius: 8px; outline: none; font-size: 1rem;">
                        <option value="">اختر نوع العملية</option>
                    </select>
                    <input type="number" id="paymentAmount" placeholder="المبلغ" step="0.01">
                    <button id="paymentAddBtn" type="button">حفظ</button>
                </div>
            </div>

            <div class="payment-history" id="paymentHistory"></div>
        </div>
    </div>

    <div id="settingsView">
        <div class="settings-page">
            <div class="payment-page-header">
                <button class="back-btn" type="button" onclick="closeSettings()">رجوع</button>
                <div class="title">الإعدادات</div>
                <div style="min-width: 1px;"></div>
            </div>

            <div class="settings-section">
                <div class="settings-section-title">الحساب</div>
                <div class="settings-actions">
                    <button class="logout-btn" type="button" onclick="logout()">تسجيل الخروج</button>
                </div>
            </div>

            <div class="settings-section">
                <div class="settings-section-title">تغيير كلمة المرور</div>
                <div class="form-row" style="margin-bottom: 10px;">
                    <input type="password" id="currentPassword" placeholder="كلمة المرور الحالية" style="direction:ltr;">
                </div>
                <div class="form-row" style="margin-bottom: 10px;">
                    <input type="password" id="newPassword" placeholder="كلمة المرور الجديدة" style="direction:ltr;">
                </div>
                <div class="form-row" style="margin-bottom: 10px;">
                    <input type="password" id="newPasswordConfirm" placeholder="تأكيد كلمة المرور الجديدة" style="direction:ltr;">
                </div>
                <label class="checkbox-wrapper" style="justify-content:center;">
                    <input type="checkbox" id="showChangePassword" onclick="toggleChangePasswordVisibility()">
                    <span>إظهار كلمة المرور</span>
                </label>
                <div class="settings-actions">
                    <button type="button" onclick="changePassword()">حفظ</button>
                </div>
                <div class="auth-note" id="changePasswordMsg"></div>
            </div>

            <div class="settings-section">
                <div class="settings-section-title">رقم سري لحذف العمليات</div>
                <div class="form-row" style="margin-bottom: 10px;">
                    <input type="password" id="currentDeletePin" placeholder="PIN الحالي" style="direction:ltr;">
                </div>
                <div class="form-row" style="margin-bottom: 10px;">
                    <input type="password" id="deletePin" placeholder="PIN الجديد" style="direction:ltr;">
                </div>
                <div class="form-row" style="margin-bottom: 10px;">
                    <input type="password" id="deletePinConfirm" placeholder="تأكيد PIN الجديد" style="direction:ltr;">
                </div>
                <label class="checkbox-wrapper" style="justify-content:center;">
                    <input type="checkbox" id="showDeletePin" onclick="toggleDeletePinVisibility()">
                    <span>إظهار PIN</span>
                </label>
                <div class="settings-actions">
                    <button type="button" onclick="saveDeletePin()">حفظ PIN</button>
                </div>
                <div class="auth-note" id="deletePinMsg"></div>
            </div>

            <div class="settings-section">
                <div class="settings-section-title">أنواع عمليات الدفع</div>
                <div class="form-row">
                    <input type="text" id="newPaymentTypeName" placeholder="إضافة نوع جديد" autocomplete="off">
                    <button type="button" onclick="addPaymentType()">إضافة</button>
                </div>
                <div class="type-list" id="paymentTypesList"></div>
            </div>
        </div>
    </div>

</div>

<div class="modal-overlay" id="pinModal">
    <div class="modal">
        <div class="modal-title" id="pinModalTitle">تأكيد الحذف</div>
        <div class="form-row" style="margin-bottom: 10px;">
            <input type="password" id="pinInput" placeholder="PIN" style="direction:ltr;">
        </div>
        <label class="checkbox-wrapper" style="justify-content:center;">
            <input type="checkbox" id="showPinInput" onclick="togglePinPromptVisibility()">
            <span>إظهار PIN</span>
        </label>
        <div class="modal-actions">
            <button type="button" onclick="cancelPinPrompt()">إلغاء</button>
            <button class="primary" type="button" onclick="confirmPinPrompt()">تأكيد</button>
        </div>
        <div class="modal-msg" id="pinModalMsg"></div>
    </div>
</div>

<!-- أضف هنا مكتبات Firebase قبل سكربت التطبيق -->
<script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-firestore-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.2/firebase-auth-compat.js"></script>

<script>
    // --- 1. تحويل الحروف للعربية ---
    const arabicMapping = {
        'q': 'ض', 'w': 'ص', 'e': 'ث', 'r': 'ق', 't': 'ف', 'y': 'غ', 'u': 'ع', 'i': 'ه', 'o': 'خ', 'p': 'ح', '[': 'ج', ']': 'د',
        'a': 'ش', 's': 'س', 'd': 'ي', 'f': 'ب', 'g': 'ل', 'h': 'ا', 'j': 'ت', 'k': 'ن', 'l': 'م', ';': 'ك', '\'': 'ط',
        'z': 'ئ', 'x': 'ء', 'c': 'ؤ', 'v': 'ر', 'b': 'لا', 'n': 'ى', 'm': 'ة', ',': 'و', '.': 'ز', '/': 'ظ',
        '`': 'ذ', 'H': 'أ', 'Y': 'إ', 'T': 'لإ', 'G': 'لأ', 'B': 'لآ'
    };

    document.getElementById('serviceType').addEventListener('input', function(e) {
        let val = this.value;
        if(!val) return;
        let lastChar = val.slice(-1);
        if (/[a-z0-9`\[\];',\.\/]/.test(lastChar.toLowerCase()) && arabicMapping[lastChar.toLowerCase()]) {
            e.preventDefault();
            let mapped = arabicMapping[lastChar.toLowerCase()];
            this.value = val.slice(0, -1) + mapped;
        }
    });

    // --- Firebase configuration & init ---
    const firebaseConfig = {
        apiKey: "AIzaSyD718Ip6IZqKS02feem3p9V9u99_eTCST4",
        authDomain: "inwi-money-29903.firebaseapp.com",
        projectId: "inwi-money-29903",
        storageBucket: "inwi-money-29903.firebasestorage.app",
        messagingSenderId: "602309651736",
        appId: "1:602309651736:web:0c60aa1daab3ca5828026d",
        measurementId: "G-YPHDCXPPK3"
    };

    // تهيئة Firebase
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    const auth = firebase.auth();
    const recordsBaseCol = db.collection('invoiceRecords');
    const fundBaseCol = db.collection('fundTransactions');
    const safeBaseCol = db.collection('safeTransactions');
    const paymentsBaseCol = db.collection('paymentRecords');
    const appOwnerDoc = db.collection('meta').doc('appOwner');
    const legacyServicesDoc = db.collection('meta').doc('savedServices');


    // --- 2. منطق التطبيق (معدل للمزامنة مع Firestore) ---

    // ابدأ بعرض من localStorage كنسخة فورية
    let records = JSON.parse(localStorage.getItem('invoiceRecords')) || [];
    let fundTransactions = JSON.parse(localStorage.getItem('fundTransactions')) || [];
    let safeTransactions = JSON.parse(localStorage.getItem('safeTransactions')) || [];
    let payments = JSON.parse(localStorage.getItem('paymentRecords')) || [];
    let savedServices = JSON.parse(localStorage.getItem('savedServices')) || [];
    let currentFilter = 'all';
    let currentView = 'main';
    let currentUserUid = null;
    let unsubscribeRecords = null;
    let unsubscribeFund = null;
    let unsubscribeSafe = null;
    let unsubscribePayments = null;
    let unsubscribeServices = null;

    let authMode = 'login';

    const authElements = {
        authView: document.getElementById('authView'),
        appView: document.getElementById('appView'),
        tabLogin: document.getElementById('tab-login'),
        tabRegister: document.getElementById('tab-register'),
        email: document.getElementById('authEmail'),
        password: document.getElementById('authPassword'),
        remember: document.getElementById('rememberMe'),
        submitBtn: document.getElementById('authSubmitBtn'),
        msg: document.getElementById('authMsg'),
        userEmail: document.getElementById('userEmail')
    };

    const settingsElements = {
        settingsView: document.getElementById('settingsView'),
        currentPassword: document.getElementById('currentPassword'),
        newPassword: document.getElementById('newPassword'),
        newPasswordConfirm: document.getElementById('newPasswordConfirm'),
        changePasswordMsg: document.getElementById('changePasswordMsg'),
        currentDeletePin: document.getElementById('currentDeletePin'),
        deletePin: document.getElementById('deletePin'),
        deletePinConfirm: document.getElementById('deletePinConfirm'),
        deletePinMsg: document.getElementById('deletePinMsg')
    };

    const pinPromptElements = {
        modal: document.getElementById('pinModal'),
        title: document.getElementById('pinModalTitle'),
        input: document.getElementById('pinInput'),
        msg: document.getElementById('pinModalMsg')
    };

    let deletePinHash = null;
    let pendingPinPromise = null;

    let paymentTypes = [];

    const elements = {
        service: document.getElementById('serviceType'),
        amount: document.getElementById('amount'),
        check: document.getElementById('initialCheck'),
        date: document.getElementById('currentDate'),
        list: document.getElementById('recordsList'),
        fundView: document.getElementById('fundView'),
        fundDate: document.getElementById('fundDate'),
        valFund: document.getElementById('val-fund'),
        valFundPage: document.getElementById('val-fund-page'),
        fundHistory: document.getElementById('fundHistory'),
        safeView: document.getElementById('safeView'),
        safeDate: document.getElementById('safeDate'),
        valSafe: document.getElementById('val-safe'),
        valSafePage: document.getElementById('val-safe-page'),
        safeHistory: document.getElementById('safeHistory'),
        datalist: document.getElementById('servicesList'),
        
        // بطاقات الفلترة
        valAll: document.getElementById('val-all'),
        countAll: document.getElementById('count-all'),

        valPending: document.getElementById('val-pending'),
        countPending: document.getElementById('count-pending'),
        
        valReady: document.getElementById('val-ready'),
        countReady: document.getElementById('count-ready'),
        
        valPaid: document.getElementById('val-paid'),
        countPaid: document.getElementById('count-paid'),

        valPayment: document.getElementById('val-payment'),
        countPayment: document.getElementById('count-payment'),
        paymentType: document.getElementById('paymentType'),
        paymentAmount: document.getElementById('paymentAmount'),
        paymentHistory: document.getElementById('paymentHistory'),
        paymentTypesList: document.getElementById('paymentTypesList'),
        newPaymentTypeName: document.getElementById('newPaymentTypeName'),
        paymentView: document.getElementById('paymentView'),
        mainView: document.getElementById('mainView'),
        paymentDate: document.getElementById('paymentDate'),
        valPaymentPage: document.getElementById('val-payment-page')
    };

    elements.date.valueAsDate = new Date();
    elements.fundDate.value = elements.date.value;
    elements.safeDate.value = elements.date.value;
    elements.paymentDate.value = elements.date.value;

    // تذكّر البريد فقط (بدون كلمة السر)
    const rememberedEmail = localStorage.getItem('rememberedEmail') || '';
    if (rememberedEmail) {
        authElements.email.value = rememberedEmail;
        authElements.remember.checked = true;
    }

    // التحكم في العرض حسب حالة تسجيل الدخول
    auth.onAuthStateChanged(user => {
        if (user) {
            currentUserUid = user.uid;
            authElements.authView.classList.add('hidden');
            authElements.appView.classList.remove('hidden');
            authElements.userEmail.textContent = user.email || '';

            ensureAppOwnerAndMaybeMigrate(user.uid).then(() => {
                attachUserScopedListeners(user.uid);
            }).catch(e => {
                console.warn('ensureAppOwnerAndMaybeMigrate failed:', e);
                attachUserScopedListeners(user.uid);
            });
        } else {
            currentUserUid = null;
            authElements.appView.classList.add('hidden');
            authElements.authView.classList.remove('hidden');

            if (unsubscribeRecords) { unsubscribeRecords(); unsubscribeRecords = null; }
            if (unsubscribeSafe) { unsubscribeSafe(); unsubscribeSafe = null; }
            if (unsubscribeFund) { unsubscribeFund(); unsubscribeFund = null; }
            if (unsubscribePayments) { unsubscribePayments(); unsubscribePayments = null; }
            if (unsubscribeServices) { unsubscribeServices(); unsubscribeServices = null; }

            safeTransactions = [];
            fundTransactions = [];
            records = [];
            payments = [];
            savedServices = [];
        }
    });

    renderServicesList();
    renderRecords();

    window.showAuthMode = function(mode) {
        authMode = mode;
        authElements.msg.textContent = '';

        authElements.tabLogin.classList.toggle('active', mode === 'login');
        authElements.tabRegister.classList.toggle('active', mode === 'register');

        authElements.submitBtn.textContent = mode === 'login' ? 'دخول' : 'تسجيل';
        authElements.password.value = '';

        const confirmRow = document.getElementById('authConfirmRow');
        confirmRow.classList.toggle('hidden', mode !== 'register');
        if (mode !== 'register') {
            document.getElementById('authPasswordConfirm').value = '';
        }

        if (mode === 'register') {
            authElements.password.setAttribute('autocomplete', 'new-password');
        } else {
            authElements.password.setAttribute('autocomplete', 'current-password');
        }
    }

    window.submitAuth = function() {
        const email = authElements.email.value.trim();
        const password = authElements.password.value;
        const remember = authElements.remember.checked;

        const confirmVal = document.getElementById('authPasswordConfirm').value;

        if (!email || !password) {
            authElements.msg.textContent = 'المرجو إدخال البريد وكلمة المرور';
            return;
        }

        if (authMode === 'register') {
            if (!confirmVal) {
                authElements.msg.textContent = 'المرجو تأكيد كلمة المرور';
                return;
            }
            if (password !== confirmVal) {
                authElements.msg.textContent = 'كلمتا المرور غير متطابقتين';
                return;
            }
        }

        authElements.msg.textContent = '...';

        const persistence = remember ? firebase.auth.Auth.Persistence.LOCAL : firebase.auth.Auth.Persistence.SESSION;

        auth.setPersistence(persistence).then(() => {
            if (remember) {
                localStorage.setItem('rememberedEmail', email);
            } else {
                localStorage.removeItem('rememberedEmail');
            }

            if (authMode === 'register') {
                return auth.createUserWithEmailAndPassword(email, password);
            }
            return auth.signInWithEmailAndPassword(email, password);
        }).then(() => {
            authElements.msg.textContent = '';
        }).catch(err => {
            authElements.msg.textContent = err && err.message ? err.message : 'حدث خطأ';
        });
    }

    window.resetPassword = function() {
        const email = authElements.email.value.trim();
        if (!email) {
            authElements.msg.textContent = 'المرجو إدخال البريد الإلكتروني أولاً';
            return;
        }
        authElements.msg.textContent = '...';
        auth.sendPasswordResetEmail(email).then(() => {
            authElements.msg.textContent = 'تم إرسال رابط استعادة كلمة المرور إلى البريد الإلكتروني';
        }).catch(err => {
            authElements.msg.textContent = err && err.message ? err.message : 'حدث خطأ';
        });
    }

    window.logout = function() {
        auth.signOut().catch(err => {
            console.warn('logout failed:', err);
        });
    }

    window.toggleAuthPasswordVisibility = function() {
        const show = document.getElementById('showAuthPassword').checked;
        const type = show ? 'text' : 'password';
        document.getElementById('authPassword').type = type;
        document.getElementById('authPasswordConfirm').type = type;
    }

    window.openSettings = function() {
        currentView = 'settings';
        elements.mainView.classList.add('hidden');
        elements.paymentView.classList.remove('active');
        settingsElements.settingsView.classList.add('active');
        settingsElements.changePasswordMsg.textContent = '';
        settingsElements.deletePinMsg.textContent = '';
    }

    window.closeSettings = function() {
        currentView = 'main';
        settingsElements.settingsView.classList.remove('active');
        elements.paymentView.classList.remove('active');
        elements.mainView.classList.remove('hidden');
        renderRecords();
    }

    window.toggleChangePasswordVisibility = function() {
        const show = document.getElementById('showChangePassword').checked;
        const type = show ? 'text' : 'password';
        settingsElements.currentPassword.type = type;
        settingsElements.newPassword.type = type;
        settingsElements.newPasswordConfirm.type = type;
    }

    window.toggleDeletePinVisibility = function() {
        const show = document.getElementById('showDeletePin').checked;
        const type = show ? 'text' : 'password';
        settingsElements.currentDeletePin.type = type;
        settingsElements.deletePin.type = type;
        settingsElements.deletePinConfirm.type = type;
    }

    window.togglePinPromptVisibility = function() {
        const show = document.getElementById('showPinInput').checked;
        pinPromptElements.input.type = show ? 'text' : 'password';
    }

    function sha256Hex(str) {
        const enc = new TextEncoder();
        return crypto.subtle.digest('SHA-256', enc.encode(str)).then(buf => {
            const arr = Array.from(new Uint8Array(buf));
            return arr.map(b => b.toString(16).padStart(2, '0')).join('');
        });
    }

    function securityDocRef(uid) {
        return db.collection('userMeta').doc(uid).collection('meta').doc('security');
    }

    function loadDeletePinHash(uid) {
        return securityDocRef(uid).get().then(doc => {
            deletePinHash = (doc.exists && doc.data() && doc.data().deletePinHash) ? doc.data().deletePinHash : null;
        });
    }

    window.saveDeletePin = function() {
        if (!currentUserUid) return;
        const currentPin = settingsElements.currentDeletePin.value.trim();
        const pin = settingsElements.deletePin.value.trim();
        const pin2 = settingsElements.deletePinConfirm.value.trim();

        if (!pin || !pin2) {
            settingsElements.deletePinMsg.textContent = 'المرجو إدخال PIN وتأكيده';
            return;
        }
        if (pin !== pin2) {
            settingsElements.deletePinMsg.textContent = 'PIN غير متطابق';
            return;
        }
        if (pin.length < 4) {
            settingsElements.deletePinMsg.textContent = 'PIN يجب أن يكون 4 أرقام أو أكثر';
            return;
        }

        if (deletePinHash && !currentPin) {
            settingsElements.deletePinMsg.textContent = 'المرجو إدخال PIN الحالي';
            return;
        }

        settingsElements.deletePinMsg.textContent = '...';
        const verifyCurrent = deletePinHash
            ? sha256Hex(currentPin).then(h => {
                if (h !== deletePinHash) throw new Error('PIN_INVALID');
            })
            : Promise.resolve();

        verifyCurrent.then(() => sha256Hex(pin)).then(hash => {
            return securityDocRef(currentUserUid).set({ deletePinHash: hash }, { merge: true }).then(() => {
                deletePinHash = hash;
                settingsElements.currentDeletePin.value = '';
                settingsElements.deletePin.value = '';
                settingsElements.deletePinConfirm.value = '';
                settingsElements.deletePinMsg.textContent = 'تم حفظ PIN';
            });
        }).catch(err => {
            console.warn('saveDeletePin failed:', err);
            if (err && err.message === 'PIN_INVALID') {
                settingsElements.deletePinMsg.textContent = 'PIN الحالي غير صحيح';
            } else {
                settingsElements.deletePinMsg.textContent = 'حدث خطأ';
            }
        });
    }

    function requireDeletePin() {
        if (!deletePinHash) {
            return Promise.reject(new Error('PIN_NOT_SET'));
        }
        pinPromptElements.title.textContent = 'تأكيد الحذف';
        pinPromptElements.msg.textContent = '';
        pinPromptElements.input.value = '';
        document.getElementById('showPinInput').checked = false;
        pinPromptElements.input.type = 'password';
        pinPromptElements.modal.classList.add('active');

        return new Promise((resolve, reject) => {
            pendingPinPromise = { resolve, reject };
        });
    }

    window.cancelPinPrompt = function() {
        pinPromptElements.modal.classList.remove('active');
        if (pendingPinPromise) {
            pendingPinPromise.reject(new Error('CANCELLED'));
            pendingPinPromise = null;
        }
    }

    window.confirmPinPrompt = function() {
        const pin = pinPromptElements.input.value.trim();
        if (!pin) {
            pinPromptElements.msg.textContent = 'المرجو إدخال PIN';
            return;
        }
        sha256Hex(pin).then(hash => {
            if (hash !== deletePinHash) {
                pinPromptElements.msg.textContent = 'PIN غير صحيح';
                return;
            }
            pinPromptElements.modal.classList.remove('active');
            if (pendingPinPromise) {
                pendingPinPromise.resolve(true);
                pendingPinPromise = null;
            }
        });
    }

    window.changePassword = function() {
        const cur = settingsElements.currentPassword.value;
        const np = settingsElements.newPassword.value;
        const npc = settingsElements.newPasswordConfirm.value;

        if (!cur || !np || !npc) {
            settingsElements.changePasswordMsg.textContent = 'المرجو ملء جميع الخانات';
            return;
        }
        if (np !== npc) {
            settingsElements.changePasswordMsg.textContent = 'كلمتا المرور غير متطابقتين';
            return;
        }
        if (!auth.currentUser || !auth.currentUser.email) return;

        settingsElements.changePasswordMsg.textContent = '...';
        const cred = firebase.auth.EmailAuthProvider.credential(auth.currentUser.email, cur);
        auth.currentUser.reauthenticateWithCredential(cred).then(() => {
            return auth.currentUser.updatePassword(np);
        }).then(() => {
            settingsElements.currentPassword.value = '';
            settingsElements.newPassword.value = '';
            settingsElements.newPasswordConfirm.value = '';
            settingsElements.changePasswordMsg.textContent = 'تم تغيير كلمة المرور';
        }).catch(err => {
            settingsElements.changePasswordMsg.textContent = err && err.message ? err.message : 'حدث خطأ';
        });
    }

    // عند اختيار اسم مسجل من الـ datalist انتقل تلقائياً إلى خانة المبلغ
    elements.service.addEventListener('input', function () {
        if (savedServices.includes(this.value)) {
            elements.amount.focus();
        }
    });

    document.getElementById('addBtn').addEventListener('click', addRecord);
    document.getElementById('paymentAddBtn').addEventListener('click', addPayment);
    elements.date.addEventListener('change', () => {
        elements.safeDate.value = elements.date.value;
        elements.fundDate.value = elements.date.value;
        elements.paymentDate.value = elements.date.value;
        renderRecords();
    });
    elements.fundDate.addEventListener('change', () => {
        elements.date.value = elements.fundDate.value;
        elements.safeDate.value = elements.date.value;
        renderRecords();
    });
    elements.safeDate.addEventListener('change', () => {
        elements.date.value = elements.safeDate.value;
        elements.fundDate.value = elements.date.value;
        renderRecords();
    });
    elements.paymentDate.addEventListener('change', function() {
        elements.date.value = elements.paymentDate.value;
        renderRecords();
    });

    function attachUserScopedListeners(uid) {
        if (unsubscribeSafe) { unsubscribeSafe(); unsubscribeSafe = null; }
        if (unsubscribeFund) { unsubscribeFund(); unsubscribeFund = null; }
        if (unsubscribeRecords) { unsubscribeRecords(); unsubscribeRecords = null; }
        if (unsubscribePayments) { unsubscribePayments(); unsubscribePayments = null; }
        if (unsubscribeServices) { unsubscribeServices(); unsubscribeServices = null; }

        // --- المزامنة من Firestore (real-time) ---
        unsubscribeRecords = recordsBaseCol.where('ownerUid', '==', uid).onSnapshot(snapshot => {
            records = snapshot.docs.map(d => {
                const data = d.data() || {};
                if (!data.time && data.id) {
                    try {
                        data.time = new Date(data.id).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                    } catch (e) {
                        data.time = '';
                    }
                }
                return { ...data, docId: d.id };
            });
            localStorage.setItem('invoiceRecords', JSON.stringify(records));
            renderRecords();
        }, err => {
            console.warn('Firestore records onSnapshot error:', err);
        });

        unsubscribeFund = fundBaseCol.where('ownerUid', '==', uid).onSnapshot(snapshot => {
            fundTransactions = snapshot.docs.map(d => {
                const data = d.data() || {};
                return { ...data, docId: d.id };
            });
            localStorage.setItem('fundTransactions', JSON.stringify(fundTransactions));
            renderRecords();
        }, err => console.warn('Firestore fund onSnapshot error:', err));

        unsubscribeSafe = safeBaseCol.where('ownerUid', '==', uid).onSnapshot(snapshot => {
            safeTransactions = snapshot.docs.map(d => {
                const data = d.data() || {};
                return { ...data, docId: d.id };
            });
            localStorage.setItem('safeTransactions', JSON.stringify(safeTransactions));
            renderRecords();
        }, err => console.warn('Firestore safe onSnapshot error:', err));

        unsubscribePayments = paymentsBaseCol.where('ownerUid', '==', uid).onSnapshot(snapshot => {
            payments = snapshot.docs.map(d => {
                const data = d.data() || {};
                if (!data.time && data.id) {
                    try {
                        data.time = new Date(data.id).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                    } catch (e) {
                        data.time = '';
                    }
                }
                return { ...data, docId: d.id };
            });
            localStorage.setItem('paymentRecords', JSON.stringify(payments));
            renderRecords();
        }, err => {
            console.warn('Firestore payments onSnapshot error:', err);
        });

        const paymentTypesCol = db.collection('userMeta').doc(uid).collection('paymentTypes');
        paymentTypesCol.onSnapshot(snapshot => {
            paymentTypes = snapshot.docs.map(d => ({ id: d.id, ...(d.data() || {}) }));
            paymentTypes.sort((a, b) => String(a.name || '').localeCompare(String(b.name || '')));
            renderPaymentTypes();
            renderRecords();
        }, err => {
            console.warn('Firestore paymentTypes onSnapshot error:', err);
        });

        const userServicesDoc = db.collection('userMeta').doc(uid).collection('meta').doc('savedServices');
        unsubscribeServices = userServicesDoc.onSnapshot(doc => {
            if (doc.exists) {
                savedServices = doc.data().list || [];
            } else {
                savedServices = [];
            }
            savedServices.sort();
            localStorage.setItem('savedServices', JSON.stringify(savedServices));
            renderServicesList();
        }, err => {
            console.warn('Firestore services onSnapshot error:', err);
        });

        loadDeletePinHash(uid).catch(e => console.warn('loadDeletePinHash failed:', e));
    }

    function getPaymentTypeNameFromPayment(p) {
        if (p && p.typeId) {
            const t = paymentTypes.find(x => x.id === p.typeId);
            if (t && t.name) return t.name;
        }
        return p && (p.typeName || p.type) ? (p.typeName || p.type) : '';
    }

    function renderPaymentTypes() {
        // select options
        if (elements.paymentType) {
            const currentVal = elements.paymentType.value;
            elements.paymentType.innerHTML = '<option value="">اختر نوع العملية</option>';
            paymentTypes.forEach(t => {
                const opt = document.createElement('option');
                opt.value = t.id;
                opt.textContent = t.name || '';
                elements.paymentType.appendChild(opt);
            });
            if (currentVal) elements.paymentType.value = currentVal;
        }

        // list
        if (!elements.paymentTypesList) return;
        elements.paymentTypesList.innerHTML = '';
        paymentTypes.forEach(t => {
            const row = document.createElement('div');
            row.className = 'type-item';
            row.innerHTML = `
                <div class="name">${t.name || ''}</div>
                <div class="actions">
                    <button type="button" onclick="editPaymentTypePrompt('${t.id}')">تعديل</button>
                    <button class="danger" type="button" onclick="deletePaymentType('${t.id}')">حذف</button>
                </div>
            `;
            elements.paymentTypesList.appendChild(row);
        });

        if (paymentTypes.length === 0) {
            elements.paymentTypesList.innerHTML = '<div style="text-align:center; padding:10px; color:#999;">لا توجد أنواع</div>';
        }
    }

    window.addPaymentType = function() {
        if (!currentUserUid) return;
        const name = (elements.newPaymentTypeName.value || '').trim();
        if (!name) return;
        const col = db.collection('userMeta').doc(currentUserUid).collection('paymentTypes');
        col.add({ name: name, createdAt: Date.now() }).then(() => {
            elements.newPaymentTypeName.value = '';
        }).catch(e => console.warn('addPaymentType failed:', e));
    }

    window.editPaymentTypePrompt = function(typeId) {
        if (!currentUserUid) return;
        const t = paymentTypes.find(x => x.id === typeId);
        const currentName = t && t.name ? t.name : '';
        const newName = prompt('تعديل نوع العملية', currentName);
        if (newName === null) return;
        const name = String(newName).trim();
        if (!name) return;
        db.collection('userMeta').doc(currentUserUid).collection('paymentTypes').doc(typeId)
            .set({ name: name }, { merge: true })
            .catch(e => console.warn('editPaymentType failed:', e));
    }

    window.deletePaymentType = function(typeId) {
        if (!currentUserUid) return;
        // require PIN for delete types as well
        requireDeletePin().then(() => {
            return db.collection('userMeta').doc(currentUserUid).collection('paymentTypes').doc(typeId).delete();
        }).catch(err => {
            if (err && err.message === 'PIN_NOT_SET') {
                alert('المرجو تعيين PIN للحذف من الإعدادات');
            }
        });
    }

    function ensureAppOwnerAndMaybeMigrate(uid) {
        return appOwnerDoc.get().then(doc => {
            if (doc.exists && doc.data() && doc.data().ownerUid) {
                return; // owner already set
            }
            // first account: claim ownership and migrate existing data
            return appOwnerDoc.set({ ownerUid: uid }).then(() => migrateLegacyDataToOwner(uid));
        });
    }

    function migrateLegacyDataToOwner(uid) {
        const tasks = [];

        tasks.push(recordsBaseCol.get().then(snapshot => {
            const batch = db.batch();
            let changed = 0;
            snapshot.docs.forEach(d => {
                const data = d.data() || {};
                if (!data.ownerUid) {
                    batch.update(d.ref, { ownerUid: uid });
                    changed++;
                }
            });
            return changed ? batch.commit() : Promise.resolve();
        }));

        tasks.push(paymentsBaseCol.get().then(snapshot => {
            const batch = db.batch();
            let changed = 0;
            snapshot.docs.forEach(d => {
                const data = d.data() || {};
                if (!data.ownerUid) {
                    batch.update(d.ref, { ownerUid: uid });
                    changed++;
                }
            });
            return changed ? batch.commit() : Promise.resolve();
        }));

        // migrate legacy saved services list to this user
        tasks.push(legacyServicesDoc.get().then(doc => {
            if (!doc.exists) return;
            const list = (doc.data() && doc.data().list) ? doc.data().list : [];
            return db.collection('userMeta').doc(uid).collection('meta').doc('savedServices').set({ list: list });
        }));

        return Promise.all(tasks).then(() => undefined);
    }

    window.changeDate = function(days, fromPageId = null) {
        let targetDateElem;
        if (fromPageId === 'fund') {
            targetDateElem = elements.fundDate;
        } else if (fromPageId === 'safe') {
            targetDateElem = elements.safeDate;
        } else if (fromPageId === 'payment') {
            targetDateElem = elements.paymentDate;
        } else {
            targetDateElem = elements.date;
        }
        const d = new Date(targetDateElem.value);
        d.setDate(d.getDate() + days);
        targetDateElem.valueAsDate = d;
        // trigger change event
        const event = new Event('change');
        targetDateElem.dispatchEvent(event);
    }


    // utils
    function findRecordById(id) {
        return records.find(r => r.id === id);
    }
    function findDocIdById(id) {
        const r = findRecordById(id);
        return r ? r.docId : null;
    }

    function findPaymentById(id) {
        return payments.find(p => p.id === id);
    }
    function findPaymentDocIdById(id) {
        const p = findPaymentById(id);
        return p ? p.docId : null;
    }

    // تغيير الفلتر
    window.setFilter = function(filterType) {
        currentFilter = filterType;

        if (currentView === 'payment') {
            closePaymentPage();
            closeSafePage();
            closeFundPage();
        }

        if (currentView === 'settings') {
            closeSettings();
        }
        
        // تحديث التصميم
        document.querySelectorAll('.filter-card').forEach(card => card.classList.remove('active'));
        document.getElementById(`filter-${filterType}`).classList.add('active');

        renderRecords();
    }

    window.openFundPage = function() {
        currentView = 'fund';
        elements.mainView.classList.add('hidden');
        elements.safeView.classList.add('hidden');
        elements.paymentView.classList.remove('active');
        settingsElements.settingsView.classList.remove('active');
        elements.fundView.classList.remove('hidden');
        elements.fundDate.value = elements.date.value;
        renderRecords();
    }

    window.closeFundPage = function() {
        currentView = 'main';
        elements.safeView.classList.add('hidden');
        elements.fundView.classList.add('hidden');
        elements.mainView.classList.remove('hidden');
    }

    window.openPaymentPage = function() {
        currentView = 'payment';
        elements.mainView.classList.add('hidden');
        elements.safeView.classList.add('hidden');
        elements.fundView.classList.add('hidden');
        settingsElements.settingsView.classList.remove('active');
        elements.paymentView.classList.add('active');
        elements.paymentDate.value = elements.date.value;
        renderRecords();
    }

    window.closePaymentPage = function() {
        currentView = 'main';
        elements.paymentView.classList.remove('active');
        elements.safeView.classList.add('hidden');
        elements.fundView.classList.add('hidden');
        settingsElements.settingsView.classList.remove('active');
        elements.mainView.classList.remove('hidden');
        renderRecords();
    }

    window.openSafePage = function() {
        currentView = 'safe';
        elements.mainView.classList.add('hidden');
        elements.fundView.classList.add('hidden');
        elements.paymentView.classList.remove('active');
        settingsElements.settingsView.classList.remove('active');
        elements.safeView.classList.remove('hidden');
        elements.safeDate.value = elements.date.value;
        renderRecords();
    }

    window.closeSafePage = function() {
        currentView = 'main';
        elements.safeView.classList.add('hidden');
        elements.mainView.classList.remove('hidden');
    }

    function addRecord() {
        const service = elements.service.value.trim();
        const amount = parseFloat(elements.amount.value);
        const isChecked = elements.check.checked;
        const date = elements.date.value;

        if (!service || isNaN(amount) || !date) {
            alert('المرجو ملء المعلومات');
            return;
        }

        const newRecord = {
            id: Date.now(),
            service: service,
            amount: amount,
            isChecked: isChecked,
            isPaid: false,
            date: date
            // احفظ وقت التسجيل بصيغة ساعة:دقيقة
            , time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
            , ownerUid: currentUserUid
         };

        const fundTransaction = {
            id: newRecord.id + 1, // avoid same timestamp
            type: 'deduction',
            amount: amount,
            date: date,
            description: `فاتورة: ${service}`,
            time: newRecord.time,
            ownerUid: currentUserUid
        };

        const batch = db.batch();
        batch.set(recordsBaseCol.doc(), newRecord);
        batch.set(fundBaseCol.doc(), fundTransaction);

        batch.commit().then(() => {
             if (!savedServices.includes(service)) {
                savedServices.push(service);
                savedServices.sort();
                if (currentUserUid) {
                    db.collection('userMeta').doc(currentUserUid).collection('meta').doc('savedServices').set({ list: savedServices });
                }
            }
            elements.service.value = '';
            elements.amount.value = '';
            elements.check.checked = false;
        }).catch(err => {
            console.error('Failed to add record and fund transaction:', err);
            alert('خطأ في مزامنة السجل مع السحابة.');
        });
    }

    window.addFundPrompt = function() {
        requireDeletePin().then(() => {
            const amountStr = prompt("أدخل المبلغ المراد إضافته للخزنة:");
            if (amountStr === null) return; // cancelled
            const amount = parseFloat(amountStr);
            if (isNaN(amount) || amount <= 0) {
                alert("المرجو إدخال مبلغ صحيح.");
                return;
            }
            const newFundTx = {
                id: Date.now(),
                type: 'addition',
                amount: amount,
                date: new Date().toISOString().split('T')[0], // استخدم تاريخ اليوم الحالي دائماً
                description: 'إضافة يدوية',
                time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                ownerUid: currentUserUid
            };
            fundBaseCol.add(newFundTx).catch(err => {
                console.error("Fund add error:", err);
                alert('فشلت العملية. تحقق من اتصالك بالإنترنت.');
            });
        }).catch(err => {
            if (err && err.message === 'PIN_NOT_SET') {
                alert('المرجو تعيين PIN للعمليات من الإعدادات');
            } else if (err && err.message !== 'CANCELLED') {
                console.error('Failed to add to fund:', err);
                alert('خطأ في مزامنة العملية مع السحابة. حاول لاحقاً.');
            }
        });
    }

    window.addSafePrompt = function(type) {
        requireDeletePin().then(() => {
            const actionText = type === 'addition' ? 'إضافته' : 'خصمه';
            const amountStr = prompt(`أدخل المبلغ المراد ${actionText} من الصندوق:`);
            if (amountStr === null) return; // cancelled
            const amount = parseFloat(amountStr);
            if (isNaN(amount) || amount <= 0) {
                alert("المرجو إدخال مبلغ صحيح.");
                return;
            }

            const description = type === 'addition' ? 'إضافة يدوية للصندوق' : 'خصم يدوي من الصندوق';

            const newSafeTx = {
                id: Date.now(),
                type: type,
                amount: amount,
                date: new Date().toISOString().split('T')[0], // استخدم تاريخ اليوم الحالي دائماً
                description: description,
                time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                ownerUid: currentUserUid
            };
            safeBaseCol.add(newSafeTx).catch(err => {
                console.error("Safe add error:", err);
                alert('فشلت العملية. تحقق من اتصالك بالإنترنت.');
            });
        }).catch(err => {
            if (err && err.message === 'PIN_NOT_SET') {
                alert('المرجو تعيين PIN للعمليات من الإعدادات');
            } else if (err && err.message !== 'CANCELLED') {
                console.error('Failed to add to safe:', err);
                alert('خطأ في مزامنة العملية مع السحابة. حاول لاحقاً.');
            }
        });
    }

    function addPayment() {
        const typeId = elements.paymentType.value;
        const amount = parseFloat(elements.paymentAmount.value);
        const date = elements.date.value;

        if (!typeId || isNaN(amount) || !date) {
            alert('المرجو ملء المعلومات');
            return;
        }

        const typeObj = paymentTypes.find(t => t.id === typeId);
        const typeName = typeObj && typeObj.name ? typeObj.name : '';

        const newPayment = {
            id: Date.now(),
            typeId: typeId,
            typeName: typeName,
            amount: amount,
            date: date,
            time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
            , ownerUid: currentUserUid
        };

        const fundTransaction = {
            id: newPayment.id + 1,
            type: 'addition',
            amount: amount,
            date: date,
            description: `عملية دفع: ${typeName}`,
            time: newPayment.time,
            ownerUid: currentUserUid
        };

        const safeTransaction = {
            id: newPayment.id + 2,
            type: 'deduction',
            amount: amount,
            date: date,
            description: `عملية دفع: ${typeName}`,
            time: newPayment.time,
            ownerUid: currentUserUid
        };

        const batch = db.batch();
        batch.set(paymentsBaseCol.doc(), newPayment);
        batch.set(fundBaseCol.doc(), fundTransaction);
        batch.set(safeBaseCol.doc(), safeTransaction);

        batch.commit().then(() => {
            elements.paymentType.value = '';
            elements.paymentAmount.value = '';
            renderRecords();
        }).catch(err => {
            console.error('Failed to add payment and related transactions:', err);
            alert('خطأ في مزامنة العملية مع السحابة. حاول لاحقاً.');
        });
    }

    function deletePayment(id) {
        requireDeletePin().then(() => {
            const paymentToDelete = findPaymentById(id);
            if (!paymentToDelete || !paymentToDelete.docId) return;

            const batch = db.batch();

            // حذف سجل الدفع
            batch.delete(paymentsBaseCol.doc(paymentToDelete.docId));

            // إضافة عمليات معاكسة للخزنة والصندوق
            const reverseFundTx = {
                id: Date.now(),
                type: 'deduction', // عكس الإضافة
                amount: paymentToDelete.amount,
                date: paymentToDelete.date,
                description: `إلغاء دفع: ${getPaymentTypeNameFromPayment(paymentToDelete)}`,
                time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                ownerUid: currentUserUid
            };
            batch.set(fundBaseCol.doc(), reverseFundTx);

            const reverseSafeTx = {
                id: Date.now() + 1,
                type: 'addition', // عكس الخصم
                amount: paymentToDelete.amount,
                date: paymentToDelete.date,
                description: `إلغاء دفع: ${getPaymentTypeNameFromPayment(paymentToDelete)}`,
                time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                ownerUid: currentUserUid
            };
            batch.set(safeBaseCol.doc(), reverseSafeTx);

            return batch.commit();
        }).catch(err => {
            if (err && err.message === 'PIN_NOT_SET') {
                alert('المرجو تعيين PIN للحذف من الإعدادات');
            }
        });
    }

    function toggleCheck(id) {
        const record = findRecordById(id);
        if (record && !record.isPaid) {
            const docId = findDocIdById(id);
            if (!docId) return;
            recordsBaseCol.doc(docId).update({ isChecked: !record.isChecked })
                .catch(e => console.warn('toggleCheck update failed:', e));
        }
    }

    function markAsPaid(id) {
        const record = findRecordById(id);
        if (record) {
            if (!record.isChecked) {
                alert('لا يمكن الاستخلاص إلا بعد وضع علامة شيك');
                return;
            }
            const docId = findDocIdById(id);
            if (!docId) return;

            const safeTransaction = {
                id: Date.now(),
                type: 'addition',
                amount: record.amount,
                date: elements.date.value, // use current date for payment
                description: `استخلاص: ${record.service}`,
                time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
                ownerUid: currentUserUid
            };

            const batch = db.batch();
            batch.update(recordsBaseCol.doc(docId), { isPaid: true });
            batch.set(safeBaseCol.doc(), safeTransaction);

            batch.commit().catch(err => {
                console.error('Failed to mark as paid and update safe:', err);
                alert('خطأ في مزامنة عملية الاستخلاص.');
            });
        }
    }

    function deleteRecord(id) {
        const docId = findDocIdById(id);
        if (!docId) return;
        requireDeletePin().then(() => {
            return recordsBaseCol.doc(docId).delete();
        }).catch(err => {
            if (err && err.message === 'PIN_NOT_SET') {
                alert('المرجو تعيين PIN للحذف من الإعدادات');
            }
        });
    }

    function renderServicesList() {
        elements.datalist.innerHTML = '';
        savedServices.forEach(s => {
            const option = document.createElement('option');
            option.value = s;
            elements.datalist.appendChild(option);
        });
    }

    function renderRecords() {
        elements.list.innerHTML = '';
        const selectedDate = elements.date.value;
        if (elements.safeDate.value !== selectedDate) {
            elements.safeDate.value = selectedDate;
        }
        if (elements.fundDate.value !== selectedDate) {
            elements.fundDate.value = selectedDate;
        }
        if (elements.paymentDate.value !== selectedDate) {
            elements.paymentDate.value = selectedDate;
        }
        
        const safeBalance = safeTransactions.reduce((bal, tx) => bal + (tx.type === 'addition' ? tx.amount : -tx.amount), 0);
        const fundBalance = fundTransactions.reduce((bal, tx) => bal + (tx.type === 'addition' ? tx.amount : -tx.amount), 0);

        // 1. تحديد مجموعات البيانات للحساب
        
        // بيانات اليوم فقط (للأزرق والأحمر)
        const todayRecords = records.filter(r => r.date === selectedDate);

        const todayPayments = payments.filter(p => p.date === selectedDate);
        
        // جميع البيانات (للبرتقالي والأخضر)
        const allRecords = records;

        // 2. الحسابات
        
        // الأزرق: مجموع اليوم (كل الحالات المسجلة في هذا اليوم)
        const sumAllToday = todayRecords.reduce((sum, r) => sum + r.amount, 0);

        const sumPaymentToday = todayPayments.reduce((sum, p) => sum + p.amount, 0);
        
        // الأحمر: مستخلص اليوم (فقط المدفوع في هذا التاريخ)
        // ملاحظة: نفترض ان الدفع تم في نفس تاريخ السجل، أو ان المستخدم يرى سجلات مدفوعة بتاريخ اليوم
        const paidToday = todayRecords.filter(r => r.isPaid);
        const sumPaidToday = paidToday.reduce((sum, r) => sum + r.amount, 0);

        // البرتقالي: للمراجعة (الكل - تراكمي) - غير مدفوع وبدون شيك
        const pendingGlobal = allRecords.filter(r => !r.isPaid && !r.isChecked);
        const sumPendingGlobal = pendingGlobal.reduce((sum, r) => sum + r.amount, 0);

        // الأخضر: جاهز (الكل - تراكمي) - غير مدفوع وعليه شيك
        const readyGlobal = allRecords.filter(r => !r.isPaid && r.isChecked);
        const sumReadyGlobal = readyGlobal.reduce((sum, r) => sum + r.amount, 0);

        const paymentsGlobal = payments;
        const sumPaymentsGlobal = paymentsGlobal.reduce((sum, p) => sum + (Number(p.amount) || 0), 0);


        // 3. تحديث واجهة الكروت

        // الصندوق
        elements.valSafe.textContent = safeBalance.toFixed(2);
        elements.valSafePage.textContent = safeBalance.toFixed(2);

        // الخزنة
        elements.valFund.textContent = fundBalance.toFixed(2);
        elements.valFundPage.textContent = fundBalance.toFixed(2);
        
        // الأزرق (اليوم)
        elements.valAll.textContent = sumAllToday.toFixed(2);
        elements.countAll.textContent = todayRecords.length + ' عملية';
        
        // البرتقالي (الكل)
        elements.valPending.textContent = sumPendingGlobal.toFixed(2);
        elements.countPending.textContent = pendingGlobal.length + ' فاتورة';

        // الأخضر (الكل)
        elements.valReady.textContent = sumReadyGlobal.toFixed(2);
        elements.countReady.textContent = readyGlobal.length + ' شيك';

        // الأحمر (اليوم)
        elements.valPaid.textContent = sumPaidToday.toFixed(2);
        elements.countPaid.textContent = paidToday.length + ' عملية';

        elements.valPayment.textContent = sumPaymentToday.toFixed(2);
        elements.countPayment.textContent = todayPayments.length + ' عملية';

        elements.valPaymentPage.textContent = sumPaymentToday.toFixed(2);

        elements.safeHistory.innerHTML = '';
        const safeHistoryToRender = [...safeTransactions.filter(tx => tx.date === selectedDate)].sort((a, b) => b.id - a.id);
        safeHistoryToRender.forEach(tx => {
            const item = document.createElement('div');
            item.className = 'payment-history-item';
            const isAddition = tx.type === 'addition';
            const amountColor = isAddition ? 'var(--green)' : 'var(--red)';
            const amountSign = isAddition ? '+' : '-';
            item.innerHTML = `
                <div class="left">
                    <strong>${tx.description}</strong>
                    <span>${tx.date} <span class="time-tag">${tx.time || ''}</span></span>
                </div>
                <div class="amt" style="color: ${amountColor};">${amountSign}${Number(tx.amount || 0).toFixed(2)}</div>
            `;
            elements.safeHistory.appendChild(item);
        });

        if (safeHistoryToRender.length === 0) {
            elements.safeHistory.innerHTML = '<div style="text-align:center; padding:10px; color:#999;">لا توجد عمليات لهذا اليوم</div>';
        }


        elements.fundHistory.innerHTML = '';
        const fundHistoryToRender = [...fundTransactions.filter(tx => tx.date === selectedDate)].sort((a, b) => b.id - a.id);
        fundHistoryToRender.forEach(tx => {
            const item = document.createElement('div');
            item.className = 'payment-history-item';
            const isAddition = tx.type === 'addition';
            const amountColor = isAddition ? 'var(--green)' : 'var(--red)';
            const amountSign = isAddition ? '+' : '-';
            item.innerHTML = `
                <div class="left">
                    <strong>${tx.description}</strong>
                    <span>${tx.date} <span class="time-tag">${tx.time || ''}</span></span>
                </div>
                <div class="amt" style="color: ${amountColor};">${amountSign}${Number(tx.amount || 0).toFixed(2)}</div>
            `;
            elements.fundHistory.appendChild(item);
        });

        if (fundHistoryToRender.length === 0) {
            elements.fundHistory.innerHTML = '<div style="text-align:center; padding:10px; color:#999;">لا توجد عمليات لهذا اليوم</div>';
        }

        elements.paymentHistory.innerHTML = '';
        const paymentHistoryToRender = [...todayPayments].sort((a, b) => b.id - a.id);
        paymentHistoryToRender.forEach(p => {
            const item = document.createElement('div');
            item.className = 'payment-history-item';
            const displayTypeName = getPaymentTypeNameFromPayment(p);
            item.innerHTML = `
                <div class="left">
                    <strong>${displayTypeName}</strong>
                    <span>${p.date} <span class="time-tag">${p.time || ''}</span></span>
                </div>
                <div style="display:flex; gap:10px; align-items:center;">
                    <div class="amt">${Number(p.amount || 0).toFixed(2)}</div>
                    <button class="delete" type="button" onclick="deletePayment(${p.id})">حذف</button>
                </div>
            `;
            elements.paymentHistory.appendChild(item);
        });

        if (paymentHistoryToRender.length === 0) {
            elements.paymentHistory.innerHTML = '<div style="text-align:center; padding:10px; color:#999;">لا توجد بيانات</div>';
        }


        // 4. تحديد القائمة المعروضة في الأسفل
        let listToRender = [];
        
        if (currentFilter === 'all') {
            const normalizedPayments = todayPayments.map(p => ({
                id: p.id,
                kind: 'payment',
                service: getPaymentTypeNameFromPayment(p),
                amount: p.amount,
                date: p.date,
                time: p.time
            }));
            listToRender = [...todayRecords, ...normalizedPayments];
        } else if (currentFilter === 'paid') {
            listToRender = paidToday; // عرض مدفوعات اليوم
        } else if (currentFilter === 'pending') {
            listToRender = pendingGlobal; // عرض المعلق من كل الأيام
        } else if (currentFilter === 'ready') {
            listToRender = readyGlobal; // عرض الجاهز من كل الأيام
        }

        // الترتيب: من الأحدث إلى الأقدم في الصفحة الرئيسية (all)
        if (currentFilter === 'all') {
            listToRender.sort((a, b) => b.id - a.id); // newest first
        }

        // 5. رسم العناصر
        elements.list.innerHTML = '';
        listToRender.forEach(r => {
            const div = document.createElement('div');
            
            let statusClass = 'status-pending';
            let statusText = 'مراجعة';
            let actionsHtml = '';
            
            // إضافة التاريخ للعرض إذا كنا في قائمة تراكمية (برتقالي/أخضر)
            let dateBadge = '';
            if (currentFilter === 'pending' || currentFilter === 'ready') {
                dateBadge = `<span class="date-tag">${r.date}</span>`;
            }

            // وسم الوقت دائماً إن وجد
            const timeBadge = r.time ? `<span class="time-tag">${r.time}</span>` : '';

            if (r.kind === 'payment') {
                statusClass = 'status-payment';
                statusText = 'الدفع';
                actionsHtml = '';
            } else if (r.isPaid) {
                statusClass = 'status-paid';
                statusText = 'مستخلص';
                actionsHtml = `<button class="action-btn delete-btn" onclick="deleteRecord(${r.id})">حذف</button>`;
            } else {
                if (r.isChecked) {
                    statusClass = 'status-checked';
                    statusText = 'جاهز';
                }

                const payDisabledAttr = r.isChecked ? '' : 'disabled';
                
                actionsHtml = `
                    <button class="action-btn check-btn ${r.isChecked ? 'is-checked' : ''}" 
                            onclick="toggleCheck(${r.id})">
                        ${r.isChecked ? '✓' : '-'}
                    </button>
                    
                    <button class="action-btn pay-btn" 
                            onclick="markAsPaid(${r.id})" 
                            ${payDisabledAttr}>
                        استخلاص
                    </button>
                    
                    <button class="action-btn delete-btn" onclick="deleteRecord(${r.id})">x</button>
                `;
            }

            div.className = `record-item ${statusClass}`;
            
            div.innerHTML = `
                <div class="record-info">
                    <strong>${r.service}</strong>
                    <span>${dateBadge}${statusText} ${timeBadge}</span>
                </div>
                <div class="record-amount">${r.amount.toFixed(2)}</div>
                <div class="actions">
                    ${actionsHtml}
                </div>
            `;
            elements.list.appendChild(div);
        });

        if (listToRender.length === 0) {
            elements.list.innerHTML = '<div style="text-align:center; padding:30px; color:#999;">لا توجد بيانات</div>';
        }
    }
</script>

</body>
</html>
