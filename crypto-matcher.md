---
layout: default
title: Coin-Match Simulator
---

<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore-compat.js"></script>

<style>
  html, body, .site-header, .site-footer, .page-content, .wrapper { background-color: #1a1b23 !important; color: #e2e8f0 !important; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
  .game-container { text-align: center; max-width: 650px; margin: 20px auto; padding: 20px; background: #242632; border: 1px solid #2f3245; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.3); }
  .game-title { color: #00f0ff; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 5px; }
  .game-stats { display: flex; justify-content: space-around; margin-bottom: 20px; font-size: 16px; color: #ff007a; font-weight: bold; }
  
  .grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 25px; background: #13141c; padding: 15px; border-radius: 8px; border: 2px solid #2f3245; }
  
  .card { height: 100px; background: #2e3142; border: 3px solid #1a1b23; border-radius: 6px; cursor: pointer; display: flex; align-items: center; justify-content: center; position: relative; user-select: none; }
  .card::before { content: ""; position: absolute; width: 70px; height: 70px; background: radial-gradient(circle, #00f0ff 20%, transparent 21%), repeating-conic-gradient(from 0deg, #00f0ff 0deg 30deg, #13141c 30deg 60deg); border-radius: 50%; border: 2px solid #00f0ff; box-shadow: 0 0 8px rgba(0,240,255,0.4); opacity: 1; }
  
  .card.flipped { background: #13141c; border-color: #00f0ff; }
  .card.flipped::before { opacity: 0; }
  .card.flipped::after { content: attr(data-crypto); font-size: 28px; font-weight: 900; color: #00f0ff; text-shadow: 0 0 10px rgba(0,240,255,0.6); }
  
  .card.matched { background: #0f1015 !important; border: 2px dashed #2f3245 !important; cursor: default; box-shadow: inset 0 0 15px rgba(0,0,0,0.6); }
  .card.matched::before, .card.matched::after { display: none !important; opacity: 0 !important; content: "" !important; }

  .action-btn { background: #00e5ff; color: #000; border: none; padding: 10px 30px; font-size: 14px; border-radius: 6px; cursor: pointer; font-weight: bold; text-transform: uppercase; box-shadow: 0 4px 0 #00a8bc; }
  .action-btn:active { transform: translateY(3px); box-shadow: 0 1px 0 #00a8bc; }
  #winMessage { display: none; margin-top: 20px; padding: 15px; border: 1px solid #ff007a; background: #1a1b23; border-radius: 6px; }
</style>

<div class="game-container">
  <h2 class="game-title">⚡ COIN-MATCH SIMULATOR</h2>
  <p style="color: #94a3b8; font-size: 13px; margin-bottom: 20px;" id="authWarning">Skor kaydı için önce ana sayfada Google Girişi yapmalısın!</p>

  <div class="game-stats">
    <div>TIME: <span id="timer">45</span>s</div>
    <div>SCORE: <span id="score">0</span></div>
  </div>

  <div class="grid" id="gameGrid"></div>
  <button class="action-btn" id="startBtn" onclick="startGame()">Start Operation</button>

  <div id="winMessage">
    <h3 style="color: #ff007a; margin-top:0;">🚀 RACK CLEARED!</h3>
    <p style="font-size: 14px;" id="rewardText">Saving data to Firebase...</p>
    <a href="/" class="action-btn" style="text-decoration:none; display:inline-block; background:#ff007a; box-shadow:0 4px 0 #b00052; color:white;">Return to Station</a>
  </div>
</div>

<script>
  const firebaseConfig = {
    apiKey: "AIzaSyDi7xosmyNGJELn4KOpe8QEg5tNewkIsEc",
    authDomain: "studioers-arcade.firebaseapp.com",
    projectId: "studioers-arcade",
    storageBucket: "studioers-arcade.firebasestorage.app",
    messagingSenderId: "1096473829075",
    appId: "1:1096473829075:web:a0f0e3023ab7ac02847e26",
    measurementId: "G-0HYW1H5FV2"
  };

  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();
  const db = firebase.firestore();
  let activeUser = null;

  auth.onAuthStateChanged(user => {
    if (user) {
      activeUser = user;
      document.getElementById('authWarning').innerText = "Logged in as: " + user.displayName + " | Puanlar buluta aktarılacak.";
      document.getElementById('authWarning').style.color = "#00ff88";
    } else {
      activeUser = null;
    }
  });

  const cryptoIcons = ['BTC', 'ETH', 'LTC', 'SOL', 'TRX', 'BNB', 'DOGE', 'XRP', 'BTC', 'ETH', 'LTC', 'SOL', 'TRX', 'BNB', 'DOGE', 'XRP'];
  let cardsChosen = []; let cardsChosenId = []; let cardsWon = [];
  let timer; let timeLeft = 45; let gameActive = false;
  const grid = document.getElementById('gameGrid');
  const startBtn = document.getElementById('startBtn');
  const winMessage = document.getElementById('winMessage');

  function shuffle(array) { return array.sort(() => 0.5 - Math.random()); }

  function createBoard() {
    grid.innerHTML = '';
    const shuffledIcons = shuffle([...cryptoIcons]);
    for (let i = 0; i < shuffledIcons.length; i++) {
      const card = document.createElement('div');
      card.setAttribute('class', 'card');
      card.setAttribute('data-id', i);
      card.setAttribute('data-crypto', shuffledIcons[i]);
      card.addEventListener('click', flipCard);
      grid.appendChild(card);
    }
  }

  function startGame() {
    if (gameActive) return;
    gameActive = true; timeLeft = 45; cardsWon = [];
    document.getElementById('timer').innerText = timeLeft;
    document.getElementById('score').innerText = 0;
    winMessage.style.display = 'none'; startBtn.style.display = 'none';
    createBoard();
    timer = setInterval(() => {
      timeLeft--; document.getElementById('timer').innerText = timeLeft;
      if (timeLeft <= 0) { clearInterval(timer); gameOver(false); }
    }, 1000);
  }

  function flipCard() {
    if (!gameActive) return;
    let cardId = this.getAttribute('data-id');
    if (cardsChosenId.includes(cardId) || this.classList.contains('matched') || this.classList.contains('flipped')) return;
    cardsChosen.push(this.getAttribute('data-crypto'));
    cardsChosenId.push(cardId);
    this.classList.add('flipped');
    if (cardsChosen.length === 2) { setTimeout(checkForMatch, 600); }
  }

  function checkForMatch() {
    const cards = grid.children;
    const oOneId = cardsChosenId[0]; const oTwoId = cardsChosenId[1];
    if (cardsChosen[0] === cardsChosen[1]) {
      cards[oOneId].className = 'card matched'; cards[oTwoId].className = 'card matched';
      cardsWon.push(cardsChosen);
      document.getElementById('score').innerText = cardsWon.length * 10;
    } else {
      cards[oOneId].classList.remove('flipped'); cards[oTwoId].classList.remove('flipped');
    }
    cardsChosen = []; cardsChosenId = [];
    if (cardsWon.length === cryptoIcons.length / 2) { clearInterval(timer); gameOver(true); }
  }

  function gameOver(isWin) {
    gameActive = false; startBtn.style.display = 'inline-block';
    if (isWin) {
      winMessage.style.display = 'block';
      if (activeUser) {
        const userRef = db.collection("users").doc(activeUser.uid);
        db.runTransaction(transaction => {
          return transaction.get(userRef).then(doc => {
            let currentPower = doc.exists ? (doc.data().power || 0) : 0;
            transaction.update(userRef, { power: currentPower + 30.000 });
          });
        }).then(() => {
          document.getElementById('rewardText').innerText = "SUCCESS! +30.000 Th/s virtual mining power safely synced to your Firebase account.";
        });
      } else {
        document.getElementById('rewardText').innerText = "SUCCESS! Ancak giriş yapmadığın için +30.000 Th/s gücü buluta kaydedemedik kral.";
      }
    } else { alert('💥 Zaman bitti! Madenciler aşırı ısındı.'); }
  }
</script>
