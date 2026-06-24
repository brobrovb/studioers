---
layout: default
title: Crypto Matcher Game
---

<!-- Oyuna Özel Koyu Tema ve Siber CSS Alanı -->
<style>
  html, body, .site-header, .site-footer, .page-content, .wrapper {
    background-color: #0a0a0a !important;
    color: #e0e0e0 !important;
    font-family: 'Courier New', Courier, monospace;
  }

  .game-container {
    text-align: center;
    max-width: 600px;
    margin: 20px auto;
    padding: 20px;
    background: #121212;
    border: 1px solid #00e5ff;
    border-radius: 8px;
    box-shadow: 0 0 20px rgba(0,229,255,0.2);
  }

  .game-title {
    color: #00e5ff;
    text-shadow: 0 0 10px rgba(0,229,255,0.5);
    text-transform: uppercase;
    letter-spacing: 2px;
  }

  .game-stats {
    display: flex;
    justify-content: space-around;
    margin-bottom: 20px;
    font-size: 16px;
    color: #00ff88;
  }

  /* Oyun Kartları Izgarası */
  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin-bottom: 20px;
    perspective: 1000px;
  }

  /* Her Bir Kartın Yapısı */
  .card {
    height: 85px;
    background-color: #1a1a1a;
    border: 2px solid #333;
    border-radius: 6px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 36px; /* Karakterler iyice büyütüldü */
    font-weight: bold;
    font-family: Arial, sans-serif; /* Gerçek ve net karakterler için standart font */
    color: transparent; /* Kapalıyken karakter gizli */
    user-select: none;
    transition: all 0.2s ease;
  }

  .card:hover {
    border-color: #00e5ff;
    box-shadow: 0 0 8px rgba(0,229,255,0.3);
  }

  /* Kart Tıklanıp Açıldığında */
  .card.flipped {
    background-color: #222;
    border-color: #00e5ff;
    color: #00e5ff !important; /* Açılınca net mavi karakter görünür */
    text-shadow: 0 0 5px rgba(0,229,255,0.5);
  }

  /* Kartlar Eşleştiğinde (Karakter silinir, boş kutu kalır) */
  .card.matched {
    background-color: #051a0e;
    border-color: #00ff88;
    color: transparent !important; /* İçindeki görüntüyü yok ettik, boş kaldı! */
    text-shadow: none !important;
    cursor: default;
    box-shadow: inset 0 0 10px rgba(0,255,136,0.2);
  }

  .action-btn {
    background: transparent;
    color: #00e5ff;
    border: 2px solid #00e5ff;
    padding: 10px 25px;
    font-size: 14px;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
    text-transform: uppercase;
    transition: all 0.2s;
  }

  .action-btn:hover {
    background: #00e5ff;
    color: #000;
    box-shadow: 0 0 10px #00e5ff;
  }

  #winMessage {
    display: none;
    margin-top: 20px;
    padding: 15px;
    border: 1px solid #00ff88;
    background: #051a0e;
    border-radius: 4px;
  }
</style>

<div class="game-container">
  <h2 class="game-title">⚡ Crypto_Matcher_v1.1</h2>
  <p style="color: #aaa; font-size: 13px;">Find all matching crypto assets before the network core overheats!</p>

  <div class="game-stats">
    <div>TIME: <span id="timer">45</span>s</div>
    <div>SCORE: <span id="score">0</span></div>
  </div>

  <div class="grid" id="gameGrid"></div>

  <button class="action-btn" id="startBtn" onclick="startGame()">Initialize Simulation</button>

  <div id="winMessage">
    <h3 style="color: #00ff88; margin-top:0;">🚀 SIMULATION SUCCESSFUL!</h3>
    <p style="font-size: 14px;" id="rewardText"></p>
    <a href="/" class="action-btn" style="text-decoration:none; display:inline-block; border-color:#00ff88; color:#00ff88;">Return to Dashboard</a>
  </div>
</div>

<script>
  // Gerçek ve net büyük karakterler (BTC, ETH, DOGE vb. simgeleri)
  const cryptoIcons = ['BTC', 'ETH', 'LTC', 'SOL', 'TRX', 'BNB', 'BTC', 'ETH', 'LTC', 'SOL', 'TRX', 'BNB'];
  let cardsChosen = [];
  let cardsChosenId = [];
  let cardsWon = [];
  let timer;
  let timeLeft = 45;
  let gameActive = false;

  const grid = document.getElementById('gameGrid');
  const startBtn = document.getElementById('startBtn');
  const winMessage = document.getElementById('winMessage');

  function shuffle(array) {
    return array.sort(() => 0.5 - Math.random());
  }

  function createBoard() {
    grid.innerHTML = '';
    const shuffledIcons = shuffle([...cryptoIcons]);
    
    for (let i = 0; i < shuffledIcons.length; i++) {
      const card = document.createElement('div');
      card.setAttribute('class', 'card');
      card.setAttribute('data-id', i);
      card.innerText = shuffledIcons[i];
      card.addEventListener('click', flipCard);
      grid.appendChild(card);
    }
  }

  function startGame() {
    if (gameActive) return;
    gameActive = true;
    timeLeft = 45;
    cardsWon = [];
    document.getElementById('timer').innerText = timeLeft;
    document.getElementById('score').innerText = 0;
    winMessage.style.display = 'none';
    startBtn.style.display = 'none';
    
    createBoard();
    
    timer = setInterval(function() {
      timeLeft--;
      document.getElementById('timer').innerText = timeLeft;
      
      if (timeLeft <= 0) {
        clearInterval(timer);
        gameOver(false);
      }
    }, 1000);
  }

  function flipCard() {
    if (!gameActive) return;
    let cardId = this.getAttribute('data-id');
    
    if (cardsChosenId.includes(cardId) || this.classList.contains('matched') || this.classList.contains('flipped')) return;

    cardsChosen.push(grid.children[cardId].innerText);
    cardsChosenId.push(cardId);
    this.classList.add('flipped');

    if (cardsChosen.length === 2) {
      setTimeout(checkForMatch, 600); // Oyuncunun karakteri rahat görmesi için süreyi azıcık uzattık
    }
  }

  function checkForMatch() {
    const cards = grid.children;
    const optionOneId = cardsChosenId[0];
    const optionTwoId = cardsChosenId[1];

    if (cardsChosen[0] === cardsChosen[1]) {
      // Eşleşenleri "matched" sınıfına alıyoruz (CSS'te buranın rengini transparent yaptık, yazı silinecek)
      cards[optionOneId].classList.remove('flipped');
      cards[optionTwoId].classList.remove('flipped');
      cards[optionOneId].classList.add('matched');
      cards[optionTwoId].classList.add('matched');
      cardsWon.push(cardsChosen);
      
      document.getElementById('score').innerText = cardsWon.length * 10;
    } else {
      cards[optionOneId].classList.remove('flipped');
      cards[optionTwoId].classList.remove('flipped');
    }

    cardsChosen = [];
    cardsChosenId = [];

    if (cardsWon.length === cryptoIcons.length / 2) {
      clearInterval(timer);
      gameOver(true);
    }
  }

  function gameOver(isWin) {
    gameActive = false;
    startBtn.style.display = 'inline-block';
    startBtn.innerText = 'Run Simulation Again';

    if (isWin) {
      winMessage.style.display = 'block';
      
      let currentPower = localStorage.getItem('power') || 0;
      let newPower = parseFloat(currentPower) + 30.000;
      localStorage.setItem('power', newPower);
      
      document.getElementById('rewardText').innerText = "SUCCESS! +30.000 Th/s virtual mining power has been injected into your terminal.";
    } else {
      alert('💥 Connection Timeout! Core Overheated. Try Again!');
    }
  }
</script>
