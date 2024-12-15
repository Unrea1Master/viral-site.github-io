---
layout: default
---

<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Вирусный сайт</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }
        .container {
            padding: 50px;
        }
        h1 {
            color: #333;
        }
        p {
            color: #666;
        }
        .button {
            display: inline-block;
            padding: 10px 20px;
            margin-top: 20px;
            background-color: #007bff;
            color: #fff;
            text-decoration: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .visitor-counter {
            margin-top: 20px;
            color: #333;
        }
        .name-input {
            margin-top: 20px;
        }
        .name-display {
            margin-top: 20px;
            color: #333;
        }
        .language-selector {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 id="title">Добро пожаловать на вирусный сайт!</h1>
        <p id="description">Этот сайт создан для того, чтобы стать вирусным. Поделитесь им с друзьями!</p>
        <button class="button" onclick="shareSite()">Поделиться</button>
        <div class="visitor-counter">
            Посетителей: <span id="visitorCount">0</span>
        </div>
        <div class="name-input">
            <label for="name">Введите ваше имя:</label>
            <input type="text" id="name">
            <button class="button" onclick="saveName()">Сохранить имя</button>
        </div>
        <div id="nameDisplay" class="name-display"></div>
        <div class="language-selector">
            <label for="language">Выберите язык:</label>
            <select id="language" onchange="changeLanguage()">
                <option value="ru">Русский</option>
                <option value="en">English</option>
                <option value="hi">हिन्दी</option>
                <option value="es">Español</option>
                <option value="ar">العربية</option>
            </select>
        </div>
    </div>

    <script>
        const translations = {
            ru: {
                title: "Добро пожаловать на вирусный сайт!",
                description: "Этот сайт создан для того, чтобы стать вирусным. Поделитесь им с друзьями!",
                button: "Поделиться",
                alert: "Ссылка скопирована в буфер обмена! Поделитесь ею с друзьями.",
                nameLabel: "Введите ваше имя:",
                saveButton: "Сохранить имя",
                visitorCount: "Посетителей:"
            },
            en: {
                title: "Welcome to the Viral Website!",
                description: "This website is designed to go viral. Share it with your friends!",
                button: "Share",
                alert: "Link copied to clipboard! Share it with your friends.",
                nameLabel: "Enter your name:",
                saveButton: "Save Name",
                visitorCount: "Visitors:"
            },
            hi: {
                title: "वायरल वेबसाइट में आपका स्वागत है!",
                description: "यह वेबसाइट वायरल होने के लिए डिज़ाइन की गई है। इसे अपने दोस्तों के साथ साझा करें!",
                button: "साझा करें",
                alert: "लिंक क्लिपबोर्ड में कॉपी हो गया है! इसे अपने दोस्तों के साथ साझा करें।",
                nameLabel: "अपना नाम दर्ज करें:",
                saveButton: "नाम सहेजें",
                visitorCount: "आगंतुक:"
            },
            es: {
                title: "¡Bienvenido al Sitio Web Viral!",
                description: "Este sitio está diseñado para volverse viral. ¡Compártelo con tus amigos!",
                button: "Compartir",
                alert: "¡Enlace copiado al portapapeles! Compártelo con tus amigos.",
                nameLabel: "Ingrese su nombre:",
                saveButton: "Guardar nombre",
                visitorCount: "Visitantes:"
            },
            ar: {
                title: "مرحبًا بك في الموقع الفيروسي!",
                description: "تم تصميم هذا الموقع ليصبح فيروسيًا. شاركه مع أصدقائك!",
                button: "شارك",
                alert: "تم نسخ الرابط إلى الحافظة! شاركه مع أصدقائك.",
                nameLabel: "أدخل اسمك:",
                saveButton: "احفظ الاسم",
                visitorCount: "الزوار:"
            }
        };

        function shareSite() {
            const language = document.getElementById("language").value;
            const url = window.location.href;
            navigator.clipboard.writeText(url).then(() => {
                alert(translations[language].alert);
            }).catch(err => {
                alert('Не удалось скопировать ссылку: ' + err);
            });
        }

        // Функция для обновления счетчика посетителей
        function updateVisitorCount() {
            const countElement = document.getElementById('visitorCount');
            let count = localStorage.getItem('visitorCount') || 0;
            count++;
            localStorage.setItem('visitorCount', count);
            countElement.innerText = count;
        }

        // Обновляем счетчик при загрузке страницы
        window.onload = function() {
            updateVisitorCount();
            displaySavedNames();
            changeLanguage();
        };

        // Функция для сохранения имени
        function saveName() {
            const nameInput = document.getElementById('name').value;
            const forbiddenWords = ["badword1", "badword2"]; // Добавьте сюда нецензурные слова

            let isClean = true;
            forbiddenWords.forEach(word => {
                if (nameInput.toLowerCase().includes(word)) {
                    isClean = false;
                }
            });

            if (isClean) {
                let savedNames = JSON.parse(localStorage.getItem('savedNames')) || [];
                const deviceId = localStorage.getItem('deviceId') || generateDeviceId();

                if (!savedNames.some(name => name.deviceId === deviceId)) {
                    savedNames.push({ name: nameInput, deviceId: deviceId, timestamp: Date.now() });
                    localStorage.setItem('savedNames', JSON.stringify(savedNames));
                    displaySavedNames();
                } else {
                    alert("Вы уже ввели имя с этого устройства.");
                }
            } else {
                alert("Некорректное имя.");
            }
        }

        // Функция для отображения сохраненных имен
        function displaySavedNames() {
            const savedNames = JSON.parse(localStorage.getItem('savedNames')) || [];
            const nameDisplay = document.getElementById('nameDisplay');
            nameDisplay.innerHTML = '';

            const oneWeek = 604800000; // 7 дней в миллисекундах
            const now = Date.now();

            const filteredNames = savedNames.filter(name => now - name.timestamp < oneWeek);
            localStorage.setItem('savedNames', JSON.stringify(filteredNames));

            filteredNames.forEach(name => {
                const nameElement = document.createElement('div');
                nameElement.innerText = name.name;
                nameDisplay.appendChild(nameElement);
            });
        }

        // Функция для генерации уникального идентификатора устройства
        function generateDeviceId() {
            const deviceId = 'device-' + Math.random().toString(36).substr(2, 9);
            localStorage.setItem('deviceId', deviceId);
            return deviceId;
        }

        // Функция для смены языка
        function changeLanguage() {
            const language = document.getElementById("language").value;
            document.getElementById("title").innerText = translations[language].title;
            document.getElementById("description").innerText = translations[language].description;
            document.querySelector(".button").innerText = translations[language].button;
            document.querySelector("label[for='name']").innerText = translations[language].nameLabel;
            document.querySelector("button[onclick='saveName()']").innerText = translations[language].saveButton;
            document.querySelector(".visitor-counter span").previousSibling.textContent = translations[language].visitorCount;
        }
    </script>
</body>
</html>
