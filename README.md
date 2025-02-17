# Kode <!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>سایت کد تخفیف</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .form-group {
            margin-bottom: 15px;
        }
        input[type="text"], input[type="number"] {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
            border-radius: 5px;
            border: 1px solid #ddd;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:disabled {
            background-color: #ddd;
        }
        .status {
            text-align: center;
            margin-top: 20px;
        }
        .status span {
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>مدیریت کد تخفیف</h1>
    
    <div class="form-group">
        <label for="discount-code">کد تخفیف:</label>
        <input type="text" id="discount-code" placeholder="کد تخفیف را وارد کنید">
    </div>
    
    <div class="form-group">
        <label for="discount-time">مدت زمان (دقیقه):</label>
        <input type="number" id="discount-time" placeholder="مدت زمان اعتبار کد" min="1">
    </div>
    
    <button id="generate-btn">ایجاد کد تخفیف</button>

    <div class="status">
        <p>کد تخفیف فعال: <span id="active-status">ندارد</span></p>
        <p>زمان اعتبار: <span id="time-status">ندارد</span></p>
    </div>
</div>

<script>
    // ذخیره کد تخفیف و وضعیت آن در localStorage
    const generateBtn = document.getElementById('generate-btn');
    const discountCodeInput = document.getElementById('discount-code');
    const discountTimeInput = document.getElementById('discount-time');
    const activeStatus = document.getElementById('active-status');
    const timeStatus = document.getElementById('time-status');

    let discountCode = '';
    let expirationTime = 0;
    let codeActive = false;

    // بارگذاری وضعیت از localStorage
    window.onload = function() {
        const savedCode = localStorage.getItem('discountCode');
        const savedTime = localStorage.getItem('expirationTime');
        const savedActiveStatus = localStorage.getItem('codeActive');

        if (savedCode && savedTime && savedActiveStatus !== null) {
            discountCode = savedCode;
            expirationTime = savedTime;
            codeActive = savedActiveStatus === 'true';

            // نمایش وضعیت موجود
            discountCodeInput.value = discountCode;
            discountTimeInput.value = expirationTime;
            updateStatus();
        }
    };

    // به‌روزرسانی وضعیت
    function updateStatus() {
        if (codeActive) {
            activeStatus.textContent = 'فعال';
            timeStatus.textContent = `${expirationTime} دقیقه`;
        } else {
            activeStatus.textContent = 'غیرفعال';
            timeStatus.textContent = 'ندارد';
        }
    }

    // ایجاد کد تخفیف
    generateBtn.addEventListener('click', () => {
        discountCode = discountCodeInput.value;
        expirationTime = parseInt(discountTimeInput.value);

        if (discountCode && expirationTime > 0) {
            // ذخیره کد و زمان اعتبار
            localStorage.setItem('discountCode', discountCode);
            localStorage.setItem('expirationTime', expirationTime);
            localStorage.setItem('codeActive', 'true');

            codeActive = true;
            setTimeout(() => {
                codeActive = false;
                localStorage.setItem('codeActive', 'false');
                updateStatus();
            }, expirationTime * 60000); // تبدیل دقیقه به میلی‌ثانیه

            updateStatus();
        } else {
            alert("لطفاً کد تخفیف و مدت زمان را وارد کنید.");
        }
    });
</script>

</body>
</html>
