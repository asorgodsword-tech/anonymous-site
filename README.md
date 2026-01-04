# anonymous-site
anonymous-site/
│
├── index.html   ✅ (THIS IS REQUIRED)
├── style.css    (if you have CSS)
├── script.js    (if you have JS)
└── assets/      (images, icons, etc.)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Mystery Whispers</title>

<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700;900&display=swap" rel="stylesheet">

<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: 'Montserrat', sans-serif;
  color: white;
  background: linear-gradient(-45deg,#ff0066,#ff9900,#7f00ff,#00ff99,#ff3333);
  background-size: 400% 400%;
  animation: bgMove 14s infinite alternate;
}

@keyframes bgMove {
  0% { background-position: 0% 50%; }
  100% { background-position: 100% 50%; }
}

.page {
  display: none;
  min-height: 100vh;
  padding: 30px;
}

.page.active {
  display: flex;
  align-items: center;
  justify-content: center;
  animation: popIn 0.7s ease;
}

@keyframes popIn {
  0% { transform: scale(0.85); opacity: 0; }
  80% { transform: scale(1.05); }
  100% { transform: scale(1); opacity: 1; }
}

.center {
  width: 100%;
  max-width: 520px;
  text-align: center;
}

.logo {
  font-size: 3.2rem;
  font-weight: 900;
  color: #ff4fa3;
  margin-bottom: 10px;
}

.subtitle {
  opacity: 0.9;
  margin-bottom: 25px;
}

.card {
  background: rgba(0,0,0,0.45);
  padding: 25px;
  border-radius: 18px;
  box-shadow: 0 0 30px rgba(0,0,0,0.4);
}

textarea, input {
  width: 100%;
  padding: 16px;
  border-radius: 14px;
  border: none;
  font-size: 1rem;
  margin-top: 12px;
}

textarea {
  height: 120px;
}

button {
  width: 100%;
  padding: 16px;
  margin-top: 18px;
  border-radius: 30px;
  border: none;
  background: #ff4fa3;
  color: white;
  font-size: 1.05rem;
  font-weight: 900;
  cursor: pointer;
}

.nav {
  position: fixed;
  bottom: 12px;
  left: 50%;
  transform: translateX(-50%);
}

.nav button {
  width: auto;
  padding: 10px 18px;
  margin: 5px;
  font-size: 0.85rem;
}

/* FIRE */
.fire {
  font-size: 80px;
  animation: flame 1.3s infinite alternate;
}

@keyframes flame {
  from { transform: translateY(0) scale(1); filter: brightness(1); }
  to { transform: translateY(-12px) scale(1.15); filter: brightness(1.6); }
}

/* POPUP */
.popup {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.7);
  display: none;
  align-items: center;
  justify-content: center;
}

.popup.active {
  display: flex;
}

.popup .card {
  max-width: 420px;
}
</style>
</head>

<body>

<!-- WELCOME -->
<div class="page active" id="welcome">
  <div class="center">
    <div class="logo">Mystery Whispers</div>
    <div class="subtitle">Say what you’ve never said. Stay anonymous.</div>
    <button onclick="go('send')">ENTER</button>
  </div>
</div>

<!-- SEND -->
<div class="page" id="send">
  <div class="center">
    <div class="card">
      <textarea id="msg" placeholder="Write your anonymous message..."></textarea>
      <input type="file" id="file">
      <button onclick="sendMessage()">SEND ANONYMOUSLY</button>
    </div>
  </div>
</div>

<!-- INBOX -->
<div class="page" id="inbox">
  <div class="center">
    <h2>Inbox</h2>
    <div id="messages"></div>
  </div>
</div>

<!-- SCORE -->
<div class="page" id="score">
  <div class="center">
    <h2>Anonymous Score</h2>
    <div class="fire">🔥</div>
    <h1 id="count">0</h1>
  </div>
</div>

<!-- POPUP -->
<div class="popup" id="popup">
  <div class="card">
    <h3>Share Your Link</h3>
    <p id="linkText"></p>
    <button onclick="closePopup()">DONE</button>
  </div>
</div>

<!-- NAV -->
<div class="nav">
  <button onclick="go('welcome')">HOME</button>
  <button onclick="go('inbox')">INBOX</button>
  <button onclick="go('score')">SCORE</button>
</div>

<script>
let messages = [];
let score = 0;

function go(page) {
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(page).classList.add('active');
}

function sendMessage() {
  if (!msg.value && !file.files[0]) return;

  messages.push({
    text: msg.value,
    file: file.files[0]
  });

  score++;
  updateInbox();
  updateScore();

  const link = "https://mysterywhispers.app/u/" + Math.random().toString(36).slice(2);
  document.getElementById('linkText').innerText = link;

  document.getElementById('popup').classList.add('active');

  msg.value = "";
  file.value = "";
}

function closePopup() {
  document.getElementById('popup').classList.remove('active');
  go('welcome');
}

function updateInbox() {
  const box = document.getElementById('messages');
  box.innerHTML = "";
  messages.forEach(m=>{
    const d = document.createElement('div');
    d.className = "card";
    d.innerHTML = `<p>${m.text||""}</p>`;
    if(m.file){
      const img=document.createElement('img');
      img.src=URL.createObjectURL(m.file);
      img.style.width="100%";
      img.style.marginTop="10px";
      d.appendChild(img);
    }
    box.appendChild(d);
  });
}

function updateScore() {
  document.getElementById('count').innerText = score;
}
</script>

</body>
</html>
