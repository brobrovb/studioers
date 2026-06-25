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

  /* Normal Mode Container */
  .arcade-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #1a1b23 !important;
    transition: all 0.2s ease;
  }

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

  /* --- FULLSCREEN STYLES --- */
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

  .fullscreen-active h1, .fullscreen-active p {
    display: none !important;
  }

  .fullscreen-active .game-ui-top {
    position: absolute;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 100;
    width: 90%;
    max-width: 340px;
    background: rgba(36, 38, 50, 0.85);
    backdrop-filter: blur(5px);
    border-radius: 8px;
  }

  .fullscreen-active .canvas-wrapper {
    border: none;
    border-radius: 0;
    width: 100vw !important;
    height: 100vh !important;
    box-shadow: none;
  }

  .fullscreen-active .d-pad {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 100;
    width: 90%;
    max-width: 320px;
    background: rgba(26, 27, 35, 0.6);
    padding: 8px;
    border-radius: 12px;
    backdrop-filter: blur(4px);
  }
  
  .fullscreen-active .pad-btn {
    background: rgba(36, 38, 50, 0.9);
    border-color: #3a3f58;
  }
</style>

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

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getAuth, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
  import { getFirestore, doc, runTransaction } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js"; // Firestore importunu sabitledik

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
  // Firestore entegrasyonu projenin geri kalanına göre dinamik bağlanabilir

  let currentUser = null;
  onAuthStateChanged(auth, (user) => { currentUser = user; });

  const canvas = document.getElementById('tetris');
  const context = canvas.getContext('2d');
  const startOverlayBtn = document.getElementById('startOverlayBtn');
  const container = document.getElementById('arcadeContainer');
  const fsToggleBtn = document.getElementById('fsToggleBtn');

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
    setTimeout(resizeGame, 150); // Sync layouts
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

  // CRYPTO LOGO ENGINE (HIGH QUALITY VECTOR DRAWING)
  function drawSquare(x, y, color, cryptoInfo = null) {
    if (x < 0 || x >= COL || y < 0 || y >= ROW) return;
    
    // Base block background
    context.fillStyle = color;
    context.fillRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
    
    // Block grid border
    context.strokeStyle = "#1a1b23";
    context.lineWidth = 1;
    context.strokeRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);

    // Render internal vector badge if piece is active or locked
    if (color !== VACANT && cryptoInfo) {
      const centerX = (x * BLOCK_SIZE) + (BLOCK_SIZE / 2);
      const centerY = (y * BLOCK_SIZE) + (BLOCK_SIZE / 2);
      const radius = BLOCK_SIZE * 0.38;

      // Draw standard inner token circle boundary
      context.beginPath();
      context.arc(centerX, centerY, radius, 0, 2 * Math.PI);
      context.fillStyle = cryptoInfo.bg || "rgba(255,255,255,0.15)";
      context.fill();

      // Draw stylized text token ticker indicator
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
  
  // Custom asset registry for crypto identities
  const CRYPTO_ASSETS = [
    { color: "#f2a900", crypto: { ticker: "₿", bg: "#ffffff", textColor: "#f2a900" } },  // BTC
    { color: "#3c3c3d", crypto: { ticker: "Ξ", bg: "#627eea", textColor: "#ffffff" } },  // ETH
    { color: "#006097", crypto: { ticker: "✕", bg: "#ffffff", textColor: "#006097" } },  // XRP
    { color: "#14f195", crypto: { ticker: "S", bg: "#9945ff", textColor: "#14f195" } },  // SOL
    { color: "#c2a633", crypto: { ticker: "Ð", bg: "#ffffff", textColor: "#c2a633" } },  // DOGE
    { color: "#ef1c24", crypto: { ticker: "T", bg: "#ffffff", textColor: "#ef1c24" } },  // TRX
    { color: "#0052ff", crypto: { ticker: "B", bg: "#ffffff", textColor: "#0052ff" } }   // BASE / BNB Alternative
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
    
    let kick = 0;
    if (this.x + nextPattern[0].length > COL) {
      kick = COL - (this.x + nextPattern[0].length);
    }
    if (this.x < 0) kick = -this.x;

    if (!this.collision(kick, 0, nextPattern)) {
      this.x += kick;
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
          board[finalY][this.x + c] = { color: this.color, crypto: this.crypto };
        }
      }
    }
    
    if (hitCeiling) {
      endGame();
      return;
    }
    
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
    return new Piece(PIECES[r], r);
  }

  let p = null;

  // CRITICAL ENDGAME SYSTEM RESET
  function endGame() {
    isPlaying = false;
    clearInterval(gameInterval);

    // Kapsayıcıyı ve mod durumunu senkronize olarak normal ekrana çekiyoruz kanka
    toggleFullscreen(false);

    startOverlayBtn.style.display = "block";
    startOverlayBtn.innerText = "RUN AGAIN";

    alert(`💥 MATRIX COLLAPSE! Total Score: ${score}. Safe exit triggered.`);
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
            drawSquare(c, r, cell.color, cell.crypto);
          } else {
            drawSquare(c, r, VACANT);
          }
        }
      }
      
      context.fillStyle = "#ff007a";
      context.font = `bold ${Math.floor(BLOCK_SIZE * 1.2)}px sans-serif`;
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
      }, 450); 
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

  // RUN DEPLOYMENT
  initBoard();
  resizeGame();
</script>
