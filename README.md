<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Afiyet Et Oyunu 🥘</title>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
            background-color: #f8f1e4;
        }
        h1 {
            color: #d9534f;
        }
        #gameCanvas {
            border: 2px solid #d9534f;
            background-color: #fff3e0;
        }
        #score {
            font-size: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Afiyet Et Oyunu 🥘</h1>
    <p>Etleri tavaya düşür, en yüksek skoru yap!</p>
    <canvas id="gameCanvas" width="400" height="500"></canvas>
    <p id="score">Skor: 0</p>
    
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        let score = 0;
        let meats = [];
        
        // Tava nesnesi (ızgara yerine)
        let pan = { x: 150, y: 450, width: 120, height: 30 };
        
        function createMeat() {
            return {
                x: Math.random() * (canvas.width - 30),
                y: 0,
                width: 30,
                height: 30,
                speed: Math.random() * 2 + 2,
                emoji: "🥩"  // Et emojisi
            };
        }

        function drawPan() {
            ctx.fillStyle = "#555";  // Tava rengi (gri ton)
            ctx.fillRect(pan.x, pan.y, pan.width, pan.height);
            ctx.fillStyle = "#000";
            ctx.fillText("🥘", pan.x + pan.width / 3, pan.y + 20); // Tavanın görseli
        }

        function drawMeats() {
            ctx.font = "30px Arial";
            meats.forEach(meat => ctx.fillText(meat.emoji, meat.x, meat.y));
        }

        function moveMeats() {
            meats.forEach(meat => {
                meat.y += meat.speed;
                if (meat.y + meat.height >= pan.y && meat.x > pan.x && meat.x < pan.x + pan.width) {
                    score++;
                    meats.splice(meats.indexOf(meat), 1);
                } else if (meat.y > canvas.height) {
                    meats.splice(meats.indexOf(meat), 1);
                }
            });
        }

        function updateGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPan();
            drawMeats();
            moveMeats();
            document.getElementById("score").textContent = "Skor: " + score;
        }

        document.addEventListener("keydown", event => {
            if (event.key === "ArrowLeft" && pan.x > 0) pan.x -= 20;
            if (event.key === "ArrowRight" && pan.x < canvas.width - pan.width) pan.x += 20;
        });

        setInterval(() => meats.push(createMeat()), 1000);
        setInterval(updateGame, 30);
    </script>
</body>
</html>
