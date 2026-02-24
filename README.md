<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <title>BEK_BOLA | Admin Panel</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    <style>
        body { font-family: sans-serif; background: #1a1a1a; color: white; padding: 20px; }
        .alert-card { background: #2a2a2a; border-left: 10px solid #ff3b3b; padding: 20px; border-radius: 10px; margin-bottom: 15px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }
        .btn { background: #0a5cff; color: white; padding: 12px 20px; text-decoration: none; border-radius: 8px; font-weight: bold; }
        .btn-check { background: #2ecc71; border: none; cursor: pointer; margin-left: 10px; }
    </style>
</head>
<body>
    <h1>🆘 BekBola Operativ Boshqaruv</h1>
    <hr>
    <div id="alerts-list">
        </div>

    <script>
        // Foydalanuvchi qismidagi bir xil config ni bu yerga ham qo'ying
        const firebaseConfig = { ... }; 
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        database.ref('chaqiruvlar/').on('child_added', (snapshot) => {
            const data = snapshot.val();
            const alertHtml = `
                <div class="alert-card" id="${data.id}">
                    <div>
                        <h2>${data.turi}</h2>
                        <p>ID: ${data.id} | Vaqt: ${data.vaqt}</p>
                    </div>
                    <div>
                        <a href="https://www.google.com/maps?q=${data.lat},${data.lng}" target="_blank" class="btn">📍 Xaritada ko'rish</a>
                        <button onclick="resolve('${data.id}')" class="btn btn-check">✅ Bajarildi</button>
                    </div>
                </div>
            `;
            document.getElementById('alerts-list').innerHTML = alertHtml + document.getElementById('alerts-list').innerHTML;
            
            // Yangi xabar kelganda ovoz chiqarish
            const audio = new Audio('https://media.geeksforgeeks.org/wp-content/uploads/20190531135120/beep.mp3');
            audio.play();
        });

        function resolve(id) {
            database.ref('chaqiruvlar/' + id).remove();
            document.getElementById(id).remove();
        }
    </script>
</body>
</html>
