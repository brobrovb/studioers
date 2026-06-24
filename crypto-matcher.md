---
layout: default
title: Crypto Matcher Game
---

<!-- Oyuna Özel Gelişmiş RollerCoin Teması -->
<style>
  html, body, .site-header, .site-footer, .page-content, .wrapper {
    background-color: #1a1b23 !important;
    color: #e2e8f0 !important;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  .game-container {
    text-align: center;
    max-width: 650px;
    margin: 20px auto;
    padding: 20px;
    background: #242632;
    border: 1px solid #2f3245;
    border-radius: 8px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }

  .game-title {
    color: #00f0ff;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 5px;
  }

  .game-stats {
    display: flex;
    justify-content: space-around;
    margin-bottom: 20px;
    font-size: 16px;
    color: #ff007a;
    font-weight: bold;
  }

  /* Oyun Matrisi (4x4 Düzen - Screenshot_44.png Esintisi) */
  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
    margin-bottom: 25px;
    background: #13141c;
    padding: 15px;
    border-radius: 8px;
    border: 2px solid #2f3245;
  }

  /* KARTIN ANA HALİ: Birebir Madenci Fanı Tasarımı (CSS Pixel Art) */
  .card {
    height: 100px;
    background: #2e3142;
    border: 3px solid #1a1b23;
    border-radius: 6px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    user-select: none;
    transition: all 0.15s ease;
  }

  /* Kartın İçindeki Dönen Turkuaz Fan Katmanı */
  .card::before {
    content: "";
    position: absolute;
    width: 70px;
    height: 70px;
    background: radial-gradient(circle, #00f0ff 20%, transparent 21%),
                repeating-conic-gradient(from 0deg, #00f0ff 0deg 30deg, #13141c 30deg 60deg);
    border-radius: 50%;
    border: 2px solid #00f0ff;
    box-shadow: 0 0 8px rgba(0,240,255,0.4);
    transition: opacity 0.2s ease;
    opacity: 1; /* Varsayılan olarak fanlar görünüyor */
  }

  /* KART TIKLANIP AÇILDIĞINDA (FLIPPED) */
  .card.flipped {
    background: #13141c;
    border-color: #00f0ff;
  }
  /* Kart açılınca fan görseli arkaya gizleniyor */
  .card.flipped::before {
    opacity: 0;
  }
  /* Kart açılınca içindeki Gerçek Kripto Karakteri Devasa ve Net Çıkıyor */
  .card.flipped::after {
    content: attr(data-crypto);
    font-size: 28px;
    font-weight: 900;
    color: #00f0ff;
    font-family: Arial, Helvetica, sans-serif;
    text-shadow: 0 0 10px rgba(0,240,255,0.6);
  }

  /* KARTLAR EŞLEŞTİĞİNDE (MATCHED) -> Görüntü Tamamen Yok Oluyor, Boş Kasa Kalıyor */
  .card.matched {
    background: #0f1015 !important;
    border: 2px dashed #2f3245 !important;
    cursor: default;
    box-shadow: inset 0 0 15px rgba(0,0,0,0.6);
  }
  /* Eşleşen kartın fanı tamamen siliniyor */
  .card.matched::before {
    display: none !important;
    opacity: 0 !important;
  }
  /* Eşleşen kartın içindeki yazı da tamamen uçuyor, boş kalıyor */
  .card.matched::after {
    display: none !important;
    content: "" !important;
  }

  .action-btn {
    background: #00e5ff;
    color: #000000;
    border: none;
    padding: 10px 30px;
    font-size: 14px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: bold;
    text-transform: uppercase;
    box-shadow: 0 4px 0 #00a8bc;
    transition: all 0.1s;
  }

  .action-btn:active {
    transform: translateY(3px);
    box-shadow: 0 1px 0 #00a8bc;
  }

  #winMessage {
    display: none;
    margin-top: 20px;
    padding: 15px;
    border: 1px solid #ff007a;
    background: #1a1b23;
    border-radius: 6px;
  }
</style>

<div class="game-container">
  <h2 class="game-title">⚡ COIN-MATCH SIMULATOR</h2>
  <p style="color: #94a3b8; font-size: 13px; margin-bottom: 20px;">Clear the miner racks before time runs out!</p>

  <div class="game-stats">
    <div>TIME: <span id="timer">45</span>s</div>
    <div>SCORE: <span id="score">0</span></div>
  </div>

  <div class="grid" id="gameGrid"></div>

  <button class="action-btn" id="startBtn" onclick="startGame()">Start Operation</button>

  <div id="winMessage">
    <h3 style="color: #ff007a; margin-top:0;">🚀 RACK CLEARED SUCCESSFULLY!</h3>
    <p style="font-size: 14px;" id="rewardText"></p>
    <a href="/" class="action-btn" style="text-decoration:none; display:inline-block; background:#ff007a; box-shadow:0 4px 0 #b00052; color:white;">Return to Station</a>
  </div>
</div>

<script>
  // 4x4 matris için 16 kartlık havuz (8 çift gerçek kripto adı)
  const cryptoIcons = ['BTC', 'ETH', 'LTC', 'SOL', 'TRX', 'BNB', 'DOGE', 'XRP', 'BTC', 'ETH', 'LTC', 'SOL', 'TRX', 'BNB', 'DOGE', 'XRP'];
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
      // CSS content okuması için veriyi data attribute içine saklıyoruz
      card.setAttribute('data-crypto', shuffledIcons[i]);
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

    cardsChosen.push(this.getAttribute('data-crypto'));
    cardsChosenId.push(cardId);
    this.classList.add('flipped');

    if (cardsChosen.length === 2) {
      setTimeout(checkForMatch, 600);
    }
  }

  function checkForMatch() {
    const cards = grid.children;
    const optionOneId = cardsChosenId[0];
    const optionTwoId = cardsChosenId[1];

    if (cardsChosen[0] === cardsChosen[1]) {
      // Eşleşenleri temizle ve boş kutuya çevir
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
    startBtn.innerText = 'Restart Miners';

    if (isWin) {
      winMessage.style.display = 'block';
      
      let currentPower = localStorage.getItem('power') || 0;
      let newPower = parseFloat(currentPower) + 30.000;
      localStorage.setItem('power', newPower);
      
      document.getElementById('rewardText').innerText = "SUCCESS! +30.000 Th/s virtual power added to your mining operation.";
    } else {
      alert('💥 Connection Timeout! Miners overheated. Try Again!');
    }
  }
</script>
