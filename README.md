<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Taktik Otish</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
  body {
    margin: 0;
    background: #0f172a;
    color: white;
    font-family: Arial, sans-serif;
    text-align: center;
  }
  h2 { margin: 10px; }
  #info {
    display: flex;
    justify-content: space-around;
    margin: 10px;
    font-size: 18px;
  }
  #game {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    padding: 20px;
  }
  .target {
    background: #1e293b;
    border-radius: 14px;
    padding: 30px 0;
    font-size: 28px;
    cursor: pointer;
    transition: 0.15s;
  }
  .target:hover { background: #334155; }
</style>
</head>
<body>

<h2>🎯 Taktik Otish</h2>
<div id="info">
  <div>⏱️ <span id="time">30</span>s</div>
  <div>⭐ <span id="score">0</span></div>
</div>
<p>👉 Eng katta **JUFT** sonni tez bos!</p>

<div id="game"></div>

<script>
const tg = window.Telegram.WebApp;
tg.expand();

let score = 0;
let time = 30;
let correctNumber = 0;

const scoreEl = document.getElementById("score");
const timeEl = document.getElementById("time");
const gameEl = document.getElementById("game");

function randomNumber() {
  return Math.floor(Math.random() * 99) + 1;
}

function generateTargets() {
  gameEl.innerHTML = "";
  let numbers = [];
  for (let i = 0; i < 6; i++) numbers.push(randomNumber());

  correctNumber = Math.max(...numbers.filter(n => n % 2 === 0));

  numbers.forEach(n => {
    const div = document.createElement("div");
    div.className = "target";
    div.innerText = n;
    div.onclick = () => clickTarget(n);
    gameEl.appendChild(div);
  });
}

function clickTarget(n) {
  if (n === correctNumber) {
    score += 10;
  } else {
    score -= 5;
  }
  scoreEl.innerText = score;
  generateTargets();
}

function startTimer() {
  const timer = setInterval(() => {
    time--;
    timeEl.innerText = time;
    if (time <= 0) {
      clearInterval(timer);
      endGame();
    }
  }, 1000);
}

function endGame() {
  tg.showAlert(`O‘yin tugadi!\nSizning ochko: ${score}`);
  tg.sendData(JSON.stringify({ score }));
}

generateTargets();
startTimer();
</script>

</body>
</html>
