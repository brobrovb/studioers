---
layout: default
title: Protetris - Crypto Arcade
---

<style>
  html, body {
    background-color: #1a1b23 !important;
    color: #e2e8f0 !important;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    box-sizing: border-box;
    margin: 0 !important;
    padding: 0 !important;
    width: 100% !important;
    height: 100% !important;
    overflow-x: hidden !important;
  }

  /* Ana Düzen Layout */
  .arcade-layout {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    max-width: 1100px;
    margin: 0 auto;
    padding: 15px;
  }

  .arcade-container {
    flex: 1;
    min-width: 300px;
    max-width: 500px;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #1a1b23 !important;
    transition: all 0.2s ease;
  }

  /* Liderlik Tablosu CSS */
  .leaderboard-panel {
    width: 300px;
    background: #242632;
    border: 2px solid #2f3245;
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 0 15px rgba(0, 240, 255, 0.05);
    height: fit-content;
  }

  .leaderboard-title {
    font-size: 16px;
    font-weight: bold;
    color: #00f0ff;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 12px;
    text-align: center;
    border-bottom: 1px solid #2f3245;
    padding-bottom: 8px;
  }

  .leaderboard-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .leaderboard-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 10px;
    border-radius: 4px;
    margin-bottom: 6px;
    background: #13141c;
    font-size: 13px;
  }

  .leaderboard-item:nth-child(1) { border-left: 3px solid #f2a900; background: rgba(242, 169, 0, 0.1); }
  .leaderboard-item:nth-child(2) { border-left: 3px solid #627eea; background: rgba(98, 126, 234, 0.1); }
  .leaderboard-item:nth-child(3) { border-left: 3px solid #006097; background: rgba(0, 96, 151, 0.1); }

  .miner-name { color: #e2e8f0; font-weight: 500; }
  .miner-score { color: #00ff88; font-weight: bold; }

  /* Oyun Arayüz Elemanları */
  .game-ui-top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    max-width: 280px;
    margin-bottom: 10px;
    gap: 8px;
  }

  .game-header {
    background: #242632;
    border: 1px solid #2f3245;
    padding: 8px 15px;
    border-radius: 6px;
    font-weight: bold;
    font-size: 14px;
    text-transform: uppercase;
    flex-grow: 1;
    text-align: center;
  }
  .stat-val { color: #00f0ff; }

  .fullscreen-toggle-btn {
    background: #242632;
    border: 1px solid #2f3245;
    color: #fff;
    padding: 8px 12px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 14px;
    font-weight: bold;
    transition: background 0.2s;
  }
  .fullscreen-toggle-btn:hover { background: #2f3245; }

  .canvas-wrapper {
    position: relative;
    background: #13141c;
    border: 3px solid #2f3245;
    border-radius: 8px;
    box-shadow: 0 0 15px rgba(0, 240, 255, 0.1);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  canvas {
    display: block;
    background: #13141c !important;
  }

  .canvas-overlay-btn {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #00e5ff;
    color: #000;
    border: none;
    padding: 12px 22px;
    font-weight: bold;
    border-radius: 6px;
    cursor: pointer;
    text-transform: uppercase;
    box-shadow: 0 4px 0 #00a8bc;
    font-size: 13px;
    letter-spacing: 1px;
    z-index: 10;
    white-space: nowrap;
  }
  .canvas-overlay-btn:active { transform: translate(-50%, -48%); box-shadow: 0 2px 0 #00a8bc; }
  
  .d-pad {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    width: 100%;
    max-width: 280px;
    margin-top: 10px;
  }
  .pad-btn {
    background: #242632;
    border: 1px solid #2f3245;
    color: #fff;
    padding: 12px 0;
    text-align: center;
    border-radius: 6px;
    font-weight: bold;
    font-size: 18px;
    cursor: pointer;
    user-select: none;
    box-shadow: 0 3px 0 #13141c;
    touch-action: manipulation;
  }
  .pad-btn:active { transform: translateY(2px); box-shadow: 0 1px 0 #13141c; }

  /* --- FULLSCREEN AKTİFKEN HİÇBİR ŞEYİN BOZULMAMASI İÇİN --- */
  .arcade-container.fullscreen-active {
    max-width: 100% !important;
    width: 100vw !important;
    height: 100vh !important;
    position: fixed !important;
    top: 0 !important;
    left: 0 !important;
    z-index: 99999 !important;
    padding: 0 !important;
    justify-content: center;
    background-color: #13141c !important;
  }

  .fullscreen-active h1, .fullscreen-active p { display: none !important; }
  .fullscreen-active .game-ui-top {
    position: absolute; top: 10px; left: 50%; transform: translateX(-50%);
    z-index: 100; width: 90%; max-width: 340px;
    background: rgba(36, 38, 50, 0.85); backdrop-filter: blur(5px); border-radius: 8px;
  }
  .fullscreen-active .canvas-wrapper { border: none; border-radius: 0; width: 100vw !important; height: 100vh !important; box-shadow: none; }
  .fullscreen-active .d-pad {
    position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
    z-index: 100; width: 90%; max-width: 320px;
    background: rgba(26, 27, 35, 0.6); padding: 8px; border-radius: 12px; backdrop-filter: blur(4px);
  }
  .fullscreen-active .pad-btn { background: rgba(36, 38, 50, 0.9); border-color: #3a3f58; }
</style>

<div class="arcade-layout">
  
  <div class="arcade-container" id="arcadeContainer">
    <h1 style="text-align:center; font-size: 18px; color: #ffffff; margin-bottom: 5px; text-transform: uppercase; letter-spacing: 1px;">🧩 PROTETRIS CRYPTO</h1>
    <p style="text-align:center; font-size: 11px; color: #94a3b8; margin: 0 0 10px 0; text-transform: uppercase;">Standard 10x20 Crypto Mining Mode</p>

    <div class="game-ui-top">
      <div class="game-header">
        <div>SCORE: <span id="gameScore" class="stat-val">0</span></div>
      </div>
      <button class="fullscreen-toggle-btn" id="fsToggleBtn">📺 FULL</button>
    </div>

    <div class="canvas-wrapper" id="canvasWrapper">
      <canvas id="tetris"></canvas>
      <button id="startOverlayBtn" class="canvas-overlay-btn">START MINING</button>
    </div>

    <div class="d-pad">
      <div class="pad-btn" id="btnRotate">🔄</div>
      <div class="pad-btn" id="btnUp">🔼</div>
      <div></div>
      <div class="pad-btn" id="btnLeft">◀️</div>
      <div class="pad-btn" id="btnDown">🔽</div>
      <div class="pad-btn" id="btnRight">▶️</div>
    </div>
  </div>

  <div class="leaderboard-panel" id="leaderboardPanel">
    <div class="leaderboard-title">🏆 TOP MINERS</div>
    <ul class="leaderboard-list" id="leaderboardList">
      <li class="leaderboard-item"><span class="miner-name">Loading nodes...</span></li>
    </ul>
  </div>

</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getAuth, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
  import { getFirestore, doc, setDoc, getDoc, collection, query, orderBy, limit, getDocs } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

  const firebaseConfig = {
    apiKey: "AIzaSyDi7xosmyNGJELn4KOpe8QEg5tNewkIsEc",
    authDomain: "studioers-arcade.firebaseapp.com",
    projectId: "studioers-arcade",
    storageBucket: "studioers-arcade.firebasestorage.app",
    messagingSenderId: "1096473829075",
    appId: "1:1096473829075:web:a0f0e3023ab7ac02847e26",
    measurementId: "G-0HYW1H5FV2"
  };

  const app = initializeApp(firebaseConfig);
  const auth = getAuth(app);
  const db = getFirestore(app);

  let currentUser = null;
  onAuthStateChanged(auth, (user) => { 
    currentUser = user;
    loadLeaderboard(); // Kullanıcı değiştiğinde veya yüklendiğinde listeyi çek
  });

  const canvas = document.getElementById('tetris');
  const context = canvas.getContext('2d');
  const startOverlayBtn = document.getElementById('startOverlayBtn');
  const container = document.getElementById('arcadeContainer');
  const fsToggleBtn = document.getElementById('fsToggleBtn');
  const leaderboardList = document.getElementById('leaderboardList');

  const ROW = 20;
  const COL = 10;
  const VACANT = "#13141c"; 

  let BLOCK_SIZE = 22; 
  let score = 0;
  let gameInterval = null;
  let isPlaying = false;
  let isCountingDown = false;
  let board = [];
  let isFullscreenMode = false;

  // LİDERLİK TABLOSUNU FİREBASE'DEN ÇEKEN MOTOR
  async function loadLeaderboard() {
    try {
      const q = query(collection(db, "users"), orderBy("highScore", "desc"), limit(10));
      const querySnapshot = await getDocs(q);
      
      leaderboardList.innerHTML = ""; // Temizle
      
      if(querySnapshot.empty) {
        leaderboardList.innerHTML = `<li class="leaderboard-item"><span class="miner-name">No miners recorded yet</span></li>`;
        return;
      }

      let index = 1;
      querySnapshot.forEach((doc) => {
        const data = doc.data();
        const name = data.displayName || data.email ? data.email.split('@')[0] : `Miner #${doc.id.substring(0,4)}`;
        const highScore = data.highScore || 0;

        const li = document.createElement('li');
        li.className = "leaderboard-item";
        li.innerHTML = `
          <span class="miner-name">${index}. ${name}</span>
          <span class="miner-score">${highScore} PTS</span>
        `;
        leaderboardList.appendChild(li);
        index++;
      });
    } catch (error) {
      console.error("Leaderboard loading error: ", error);
      leaderboardList.innerHTML = `<li class="leaderboard-item"><span class="miner-name" style="color:#ef1c24;">Sync Error</span></li>`;
    }
  }

  // DYNAMIC RESIZE ENGINE
  function resizeGame() {
    if (isFullscreenMode) {
      const maxBlockW = Math.floor(window.innerWidth / COL);
      const maxBlockH = Math.floor(window.innerHeight / ROW);
      BLOCK_SIZE = Math.min(maxBlockW, maxBlockH);
    } else {
      BLOCK_SIZE = window.innerWidth < 480 ? 25 : 28;
    }

    canvas.width = COL * BLOCK_SIZE;
    canvas.height = ROW * BLOCK_SIZE;
    
    drawLayout();
  }

  window.addEventListener('resize', resizeGame);

  // FULLSCREEN ENGINE WITH FALLBACKS
  fsToggleBtn.addEventListener('click', () => {
    toggleFullscreen(!isFullscreenMode);
  });

  function toggleFullscreen(enable) {
    isFullscreenMode = enable;
    if (isFullscreenMode) {
      container.classList.add('fullscreen-active');
      fsToggleBtn.innerText = "❌ EXIT";
      if (container.requestFullscreen) container.requestFullscreen().catch(() => {});
      else if (container.webkitRequestFullscreen) container.webkitRequestFullscreen();
    } else {
      container.classList.remove('fullscreen-active');
      fsToggleBtn.innerText = "📺 FULL";
      if (document.fullscreenElement || document.webkitFullscreenElement) {
        if (document.exitFullscreen) document.exitFullscreen().catch(() => {});
        else if (document.webkitExitFullscreen) document.webkitExitFullscreen();
      }
    }
    setTimeout(resizeGame, 150);
  }

  function initBoard() {
    board = [];
    for (let r = 0; r < ROW; r++) {
      board[r] = [];
      for (let c = 0; c < COL; c++) {
        board[r][c] = VACANT;
      }
    }
  }

  // CRYPTO LOGO ENGINE
  function drawSquare(x, y, color, cryptoInfo = null) {
    if (x < 0 || x >= COL || y < 0 || y >= ROW) return;
    
    context.fillStyle = color;
    context.fillRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
    
    context.strokeStyle = "#1a1b23";
    context.lineWidth = 1;
    context.strokeRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);

    if (color !== VACANT && cryptoInfo) {
      const centerX = (x * BLOCK_SIZE) + (BLOCK_SIZE / 2);
      const centerY = (y * BLOCK_SIZE) + (BLOCK_SIZE / 2);
      const radius = BLOCK_SIZE * 0.38;

      context.beginPath();
      context.arc(centerX, centerY, radius, 0, 2 * Math.PI);
      context.fillStyle = cryptoInfo.bg || "rgba(255,255,255,0.15)";
      context.fill();

      context.fillStyle = cryptoInfo.textColor || "#ffffff";
      context.font = `bold ${Math.floor(BLOCK_SIZE * 0.42)}px 'Arial Black', sans-serif`;
      context.textAlign = "center";
      context.textBaseline = "middle";
      context.fillText(cryptoInfo.ticker, centerX, centerY);
    }
  }

  function drawLayout() {
    context.fillStyle = "#13141c";
    context.fillRect(0, 0, canvas.width, canvas.height);

    for (let r = 0; r < ROW; r++) {
      for (let c = 0; c < COL; c++) {
        let cell = board[r][c];
        if (cell === VACANT) {
          drawSquare(c, r, VACANT);
        } else {
          drawSquare(c, r, cell.color, cell.crypto);
        }
      }
    }
    
    if (isPlaying && p && !isCountingDown) {
      p.draw();
    }
  }

  const PIECES = [
    [[[1,1,1,1]], [[1],[1],[1],[1]]], 
    [[[0,1,0],[1,1,1]], [[1,0],[1,1],[1,0]], [[1,1,1],[0,1,0]], [[0,1],[1,1],[0,1]]], 
    [[[1,0,0],[1,1,1]], [[1,1],[1,0],[1,0]], [[1,1,1],[0,0,1]], [[0,1],[0,1],[1,1]]], 
    [[[0,0,1],[1,1,1]], [[1,0],[1,0],[1,1]], [[1,1,1],[1,0,0]], [[1,1],[0,1],[0,1]]], 
    [[[0,1,1],[1,1,0]], [[1,0],[1,1],[0,1]]], 
    [[[1,1,0],[0,1,1]], [[0,1],[1,1],[1,0]]], 
    [[[1,1],[1,1]]] 
  ];
  
  const CRYPTO_ASSETS = [
    { color: "#f2a900", crypto: { ticker: "₿", bg: "#ffffff", textColor: "#f2a900" } },  // BTC
    { color: "#3c3c3d", crypto: { ticker: "Ξ", bg: "#627eea", textColor: "#ffffff" } },  // ETH
    { color: "#006097", crypto: { ticker: "✕", bg: "#ffffff", textColor: "#006097" } },  // XRP
    { color: "#14f195", crypto: { ticker: "S", bg: "#9945ff", textColor: "#14f195" } },  // SOL
    { color: "#c2a633", crypto: { ticker: "Ð", bg: "#ffffff", textColor: "#c2a633" } },  // DOGE
    { color: "#ef1c24", crypto: { ticker: "T", bg: "#ffffff", textColor: "#ef1c24" } },  // TRX
    { color: "#0052ff", crypto: { ticker: "B", bg: "#ffffff", textColor: "#0052ff" } }   
  ];

  function Piece(tetromino, assetIndex) {
    this.tetromino = tetromino;
    this.color = CRYPTO_ASSETS[assetIndex].color;
    this.crypto = CRYPTO_ASSETS[assetIndex].crypto;
    this.tetrominoN = 0;
    this.activeTetromino = this.tetromino[this.tetrominoN];
    this.x = Math.floor((COL - this.activeTetromino[0].length) / 2);
    this.y = 0;
  }

  Piece.prototype.draw = function() {
    if (!this.activeTetromino) return;
    for (let r = 0; r < this.activeTetromino.length; r++) {
      for (let c = 0; c < this.activeTetromino[r].length; c++) {
        if (this.activeTetromino[r][c]) {
          drawSquare(this.x + c, this.y + r, this.color, this.crypto);
        }
      }
    }
  };

  Piece.prototype.moveDown = function() {
    if (isCountingDown || !isPlaying) return;
    if (!this.collision(0, 1, this.activeTetromino)) {
      this.y++;
      drawLayout();
    } else {
      this.lock();
      p = randomPiece();
    }
  };

  // Diğer hareket fonksiyonları stabil kaldı kanka
  Piece.prototype.moveRight = function() { if (isCountingDown || !isPlaying) return; if (!this.collision(1, 0, this.activeTetromino)) { this.x++; drawLayout(); } };
  Piece.prototype.moveLeft = function() { if (isCountingDown || !isPlaying) return; if (!this.collision(-1, 0, this.activeTetromino)) { this.x--; drawLayout(); } };
  Piece.prototype.rotate = function() {
    if (isCountingDown || !isPlaying) return;
    let nextN = (this.tetrominoN + 1) % this.tetromino.length;
    let nextPattern = this.tetromino[nextN];
    let kick = 0;
    if (this.x + nextPattern[0].length > COL) kick = COL - (this.x + nextPattern[0].length);
    if (this.x < 0) kick = -this.x;
    if (!this.collision(kick, 0, nextPattern)) { this.x += kick; this.tetrominoN = nextN; this.activeTetromino = nextPattern; drawLayout(); }
  };

  Piece.prototype.collision = function(x, y, piece) {
    if (!piece) return true;
    for (let r = 0; r < piece.length; r++) {
      for (let c = 0; c < piece[r].length; c++) {
        if (!piece[r][c]) continue;
        let newX = this.x + c + x;
        let newY = this.y + r + y;
        if (newX < 0 || newX >= COL || newY >= ROW) return true;
        if (newY < 0) continue; 
        if (board[newY][newX] !== VACANT) return true;
      }
    }
    return false;
  };

  Piece.prototype.lock = function() {
    if (!isPlaying || !this.activeTetromino) return;
    let hitCeiling = false;
    for (let r = 0; r < this.activeTetromino.length; r++) {
      for (let c = 0; c < this.activeTetromino[r].length; c++) {
        if (!this.activeTetromino[r][c]) continue;
        let finalY = this.y + r;
        if (finalY <= 0) hitCeiling = true;
        if (finalY >= 0 && finalY < ROW) board[finalY][this.x + c] = { color: this.color, crypto: this.crypto };
      }
    }
    if (hitCeiling) { endGame(); return; }
    for (let r = 0; r < ROW; r++) {
      let isRowFull = true;
      for (let c = 0; c < COL; c++) { if (board[r][c] === VACANT) { isRowFull = false; break; } }
      if (isRowFull) {
        for (let y = r; y > 1; y--) board[y] = [...board[y-1]];
        board[0] = Array(COL).fill(VACANT);
        score += 100;
        document.getElementById('gameScore').innerText = score;
      }
    }
    drawLayout();
  };

  function randomPiece() { let r = Math.floor(Math.random() * PIECES.length); return new Piece(PIECES[r], r); }

  let p = null;

  // REKABETÇİ ENDGAME METODU
  async function endGame() {
    isPlaying = false;
    clearInterval(gameInterval);
    toggleFullscreen(false);

    startOverlayBtn.style.display = "block";
    startOverlayBtn.innerText = "RUN AGAIN";

    alert(`💥 MATRIX COLLAPSE! Game Score: ${score}`);

    // EN YÜKSEK SKORU FİREBASE'E GÖNDERME VE SENKRONİZASYON
    if (currentUser) {
      const userRef = doc(db, "users", currentUser.uid);
      try {
        const userDoc = await getDoc(userRef);
        let currentHighScore = 0;
        let currentBalance = 0;

        if (userDoc.exists()) {
          currentHighScore = userDoc.data().highScore || 0;
          currentBalance = userDoc.data().balance || 0;
        }

        const newHighScore = Math.max(currentHighScore, score);
        const earnedReward = parseFloat((score * 0.001).toFixed(2));

        // Veritabanını güncelle
        await setDoc(userRef, {
          highScore: newHighScore,
          balance: currentBalance + earnedReward,
          email: currentUser.email,
          lastPlayed: Date.now()
        }, { merge: true });

        alert(`🎯 SYNC COMPLETE! High Score: ${newHighScore} | Balance: +${earnedReward}`);
        loadLeaderboard(); // Skor gönderildiği an listeyi yenile!

      } catch (err) {
        console.error("Database updates failed:", err);
      }
    } else {
      alert("⚠️ NOTICE: You are playing anonymously. Login to secure your place on the leaderboard!");
    }
  }

  function runStartCountdown(callback) {
    isCountingDown = true;
    let count = 3;
    let countInterval = setInterval(() => {
      context.fillStyle = "#13141c"; context.fillRect(0, 0, canvas.width, canvas.height);
      for (let r = 0; r < ROW; r++) {
        for (let c = 0; c < COL; c++) {
          let cell = board[r][c];
          if (cell !== VACANT) drawSquare(c, r, cell.color, cell.crypto);
          else drawSquare(c, r, VACANT);
        }
      }
      context.fillStyle = "#ff007a"; context.font = `bold ${Math.floor(BLOCK_SIZE * 1.2)}px sans-serif`;
      context.textAlign = "center"; context.textBaseline = "middle";
      if (count > 0) context.fillText(count, canvas.width / 2, canvas.height / 2);
      else if (count === 0) { context.fillStyle = "#00ff88"; context.fillText("START!", canvas.width / 2, canvas.height / 2); }
      count--;
      if (count < -1) { clearInterval(countInterval); isCountingDown = false; callback(); }
    }, 800);
  }

  function startGameEngine() {
    if (isPlaying || isCountingDown) return;
    startOverlayBtn.style.display = "none";
    initBoard();
    score = 0;
    document.getElementById('gameScore').innerText = score;

    runStartCountdown(() => {
      isPlaying = true;
      p = randomPiece();
      drawLayout();
      gameInterval = setInterval(() => { if (isPlaying && !isCountingDown) p.moveDown(); }, 450); 
    });
  }

  startOverlayBtn.addEventListener('click', startGameEngine);

  // Klavye ve D-Pad dinleyicileri stabil kaldı
  document.addEventListener('keydown', (e) => {
    if (!isPlaying || isCountingDown || !p) return;
    if (e.key === "ArrowLeft" || e.key.toLowerCase() === "a") p.moveLeft();
    else if (e.key === "ArrowUp" || e.key.toLowerCase() === "w") p.rotate();
    else if (e.key === "ArrowRight" || e.key.toLowerCase() === "d") p.moveRight();
    else if (e.key === "ArrowDown" || e.key.toLowerCase() === "s") p.moveDown();
  });

  document.getElementById('btnLeft').addEventListener('click', () => { if (isPlaying && !isCountingDown && p) p.moveLeft(); });
  document.getElementById('btnRight').addEventListener('click', () => { if (isPlaying && !isCountingDown && p) p.moveRight(); });
  document.getElementById('btnRotate').addEventListener('click', () => { if (isPlaying && !isCountingDown && p) p.rotate(); });
  document.getElementById('btnDown').addEventListener('click', () => { if (isPlaying && !isCountingDown && p) p.moveDown(); });
  document.getElementById('btnUp').addEventListener('click', () => {
    if (isPlaying && !isCountingDown && p) {
      while (!p.collision(0, 1, p.activeTetromino)) { p.y++; }
      p.lock(); p = randomPiece();
    }
  });

  initBoard();
  resizeGame();
  loadLeaderboard(); // İlk açılışta verileri çek
</script>
