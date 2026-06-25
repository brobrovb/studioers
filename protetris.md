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
    overflow-x: hidden !important;
  }

  .arcade-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 15px;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #1a1b23 !important;
  }

  .game-header {
    display: flex;
    justify-content: center;
    width: 100%;
    max-width: 192px;
    background: #242632;
    border: 1px solid #2f3245;
    padding: 8px 12px;
    border-radius: 6px;
    margin-bottom: 15px;
    font-weight: bold;
    font-size: 14px;
    text-transform: uppercase;
    box-sizing: border-box;
  }
  .stat-val { color: #00f0ff; }

  .canvas-wrapper {
    position: relative;
    background: #13141c;
    border: 3px solid #2f3245;
    border-radius: 8px;
    box-shadow: 0 0 15px rgba(0, 240, 255, 0.1);
    width: 192px;
    height: 240px;
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
    padding: 12px 20px;
    font-weight: bold;
    border-radius: 6px;
    cursor: pointer;
    text-transform: uppercase;
    box-shadow: 0 4px 0 #00a8bc;
    font-size: 12px;
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
    max-width: 192px;
    margin-top: 15px;
  }
  .pad-btn {
    background: #242632;
    border: 1px solid #2f3245;
    color: #fff;
    padding: 12px 0;
    text-align: center;
    border-radius: 6px;
    font-weight: bold;
    font-size: 16px;
    cursor: pointer;
    user-select: none;
    box-shadow: 0 3px 0 #13141c;
  }
  .pad-btn:active { transform: translateY(2px); box-shadow: 0 1px 0 #13141c; }
</style>

<div class="arcade-container">
  <h1 style="text-align:center; font-size: 18px; color: #ffffff; margin-bottom: 5px; text-transform: uppercase; letter-spacing: 1px;">🧩 PROTETRIS CRYPTO</h1>
  <p style="text-align:center; font-size: 11px; color: #94a3b8; margin: 0 0 15px 0; text-transform: uppercase;">8x10 Grid Endless Mining Mode</p>

  <div class="game-header">
    <div>SCORE: <span id="gameScore" class="stat-val">0</span></div>
  </div>

  <div class="canvas-wrapper">
    <canvas id="tetris" width="192" height="240"></canvas>
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

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getAuth, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
  import { getFirestore, doc, runTransaction } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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
  onAuthStateChanged(auth, (user) => { currentUser = user; });

  const canvas = document.getElementById('tetris');
  const context = canvas.getContext('2d');
  const startOverlayBtn = document.getElementById('startOverlayBtn');

  // YENİ 8x10 GRID YAPISI VE DENGELİ BLOK BOYUTU
  const BLOCK_SIZE = 24;
  const ROW = 10;
  const COL = 8;
  const VACANT = "#13141c"; 

  let score = 0;
  let gameInterval = null;
  let isPlaying = false;
  let isCountingDown = false;
  let board = [];

  function initBoard() {
    board = [];
    for (let r = 0; r < ROW; r++) {
      board[r] = [];
      for (let c = 0; c < COL; c++) {
        board[r][c] = VACANT;
      }
    }
  }

  function drawSquare(x, y, color, symbol = "") {
    if (x < 0 || x >= COL || y < 0 || y >= ROW) return;
    
    context.fillStyle = color;
    context.fillRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
    
    context.strokeStyle = "#1a1b23";
    context.lineWidth = 1;
    context.strokeRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);

    if (color !== VACANT && symbol) {
      context.fillStyle = "#ffffff";
      context.font = "13px Arial";
      context.textAlign = "center";
      context.textBaseline = "middle";
      context.fillText(symbol, (x * BLOCK_SIZE) + (BLOCK_SIZE / 2), (y * BLOCK_SIZE) + (BLOCK_SIZE / 2));
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
          drawSquare(c, r, cell.color, cell.symbol);
        }
      }
    }
    
    if (isPlaying && p && !isCountingDown) {
      p.draw();
    }
  }

  // 8x10 için optimize edilmiş kompakt rotasyon matrisleri
  const PIECES = [
    [[[0,1,0,0],[0,1,0,0],[0,1,0,0],[0,1,0,0]], [[0,0,0,0],[1,1,1,1],[0,0,0,0],[0,0,0,0]]], // I
    [[[0,1,0],[1,1,1],[0,0,0]], [[0,1,0],[0,1,1],[0,1,0]], [[0,0,0],[1,1,1],[0,1,0]], [[0,1,0],[1,1,0],[0,1,0]]], // T
    [[[1,0,0],[1,1,1],[0,0,0]], [[0,1,1],[0,1,0],[0,1,0]], [[0,0,0],[1,1,1],[0,0,1]], [[0,1,0],[0,1,0],[1,1,0]]], // L
    [[[0,0,1],[1,1,1],[0,0,0]], [[0,1,0],[0,1,0],[0,1,1]], [[0,0,0],[1,1,1],[1,0,0]], [[1,1,0],[0,1,0],[0,1,0]]], // J
    [[[0,1,1],[1,1,0],[0,0,0]], [[0,1,0],[0,1,1],[0,0,1]]], // S
    [[[1,1,0],[0,1,1],[0,0,0]], [[0,0,1],[0,1,1],[0,1,0]]], // Z
    [[[1,1],[1,1]]] // O
  ];
  
  const COLORS = ["#00f0ff", "#ff007a", "#00ff88", "#ffb700", "#9d00ff", "#ff003c", "#3300ff"];
  const SYMBOLS = ["🪙", "💎", "⚡", "🚀", "📈", "🔗", "🧱"];

  function Piece(tetromino, color, symbol) {
    this.tetromino = tetromino;
    this.color = color;
    this.symbol = symbol;
    this.tetrominoN = 0;
    this.activeTetromino = this.tetromino[this.tetrominoN];
    this.x = 3; // 8 kolon olduğu için ortalama noktası 3 yapıldı
    this.y = 0;
  }

  Piece.prototype.draw = function() {
    if (!this.activeTetromino) return;
    for (let r = 0; r < this.activeTetromino.length; r++) {
      for (let c = 0; c < this.activeTetromino[r].length; c++) {
        if (this.activeTetromino[r][c]) {
          drawSquare(this.x + c, this.y + r, this.color, this.symbol);
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

  Piece.prototype.moveRight = function() {
    if (isCountingDown || !isPlaying) return;
    if (!this.collision(1, 0, this.activeTetromino)) {
      this.x++;
      drawLayout();
    }
  };

  Piece.prototype.moveLeft = function() {
    if (isCountingDown || !isPlaying) return;
    if (!this.collision(-1, 0, this.activeTetromino)) {
      this.x--;
      drawLayout();
    }
  };

  Piece.prototype.rotate = function() {
    if (isCountingDown || !isPlaying) return;
    let nextN = (this.tetrominoN + 1) % this.tetromino.length;
    let nextPattern = this.tetromino[nextN];
    if (!this.collision(0, 0, nextPattern)) {
      this.tetrominoN = nextN;
      this.activeTetromino = nextPattern;
      drawLayout();
    }
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
        if (finalY <= 0) {
          hitCeiling = true;
        }
        if (finalY >= 0 && finalY < ROW) {
          board[finalY][this.x + c] = { color: this.color, symbol: this.symbol };
        }
      }
    }
    
    if (hitCeiling) {
      endGame();
      return;
    }
    
    // Satır Patlatma Mantığı
    for (let r = 0; r < ROW; r++) {
      let isRowFull = true;
      for (let c = 0; c < COL; c++) {
        if (board[r][c] === VACANT) { isRowFull = false; break; }
      }
      if (isRowFull) {
        for (let y = r; y > 1; y--) {
          board[y] = [...board[y-1]];
        }
        board[0] = Array(COL).fill(VACANT);
        score += 100;
        document.getElementById('gameScore').innerText = score;
      }
    }
    drawLayout();
  };

  function randomPiece() {
    let r = Math.floor(Math.random() * PIECES.length);
    return new Piece(PIECES[r], COLORS[r], SYMBOLS[r]);
  }

  let p = null;

  function endGame() {
    isPlaying = false;
    clearInterval(gameInterval);
    startOverlayBtn.style.display = "block";
    startOverlayBtn.innerText = "RUN AGAIN";

    const dynamicReward = parseFloat((score * 0.001).toFixed(2));

    alert(`💥 MATRIX COLLAPSE! Match score: ${score}. Sycing rewards with db...`);

    // SKORU VE BAKİYEYİ VERİTABANINA GÜVENLİ TRANSACTION İLE YAZMA
    if (dynamicReward > 0 && currentUser) {
      const userRef = doc(db, "users", currentUser.uid);
      runTransaction(db, async (transaction) => {
        const userDoc = await transaction.get(userRef);
        let currentBalance = userDoc.exists() ? (userDoc.data().balance || 0) : 0;
        transaction.update(userRef, { balance: currentBalance + dynamicReward });
      }).then(() => {
        alert(`🎁 SUCCESS! +${dynamicReward} Points allocation synced to your user balance.`);
      }).catch(err => console.error("Database sync runtime failure:", err));
    } else if (!currentUser) {
      alert("⚠️ NOTICE: You are not authenticated. Score cannot be recorded.");
    }
  }

  function runStartCountdown(callback) {
    isCountingDown = true;
    let count = 3;
    
    let countInterval = setInterval(() => {
      context.fillStyle = "#13141c";
      context.fillRect(0, 0, canvas.width, canvas.height);
      for (let r = 0; r < ROW; r++) {
        for (let c = 0; c < COL; c++) {
          let cell = board[r][c];
          if (cell !== VACANT) {
            drawSquare(c, r, cell.color, cell.symbol);
          } else {
            drawSquare(c, r, VACANT);
          }
        }
      }
      
      context.fillStyle = "#ff007a";
      context.font = "bold 26px sans-serif";
      context.textAlign = "center";
      context.textBaseline = "middle";
      
      if (count > 0) {
        context.fillText(count, canvas.width / 2, canvas.height / 2);
      } else if (count === 0) {
        context.fillStyle = "#00ff88";
        context.fillText("START!", canvas.width / 2, canvas.height / 2);
      }

      count--;

      if (count < -1) {
        clearInterval(countInterval);
        isCountingDown = false;
        callback();
      }
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

      gameInterval = setInterval(() => {
        if (isPlaying && !isCountingDown) p.moveDown();
      }, 500); // 8x10 alanda daha dengeli bir akış hızı
    });
  }

  startOverlayBtn.addEventListener('click', startGameEngine);

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
      while (!p.collision(0, 1, p.activeTetromino)) {
        p.y++;
      }
      p.lock();
      p = randomPiece();
    }
  });

  initBoard();
  drawLayout();
</script>
