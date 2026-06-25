---
layout: default
title: Protetris - Crypto Arcade
---

<style>
  /* Global CSS & Absolute Canvas Constraint */
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
  }

  /* Game Stats Display */
  .game-header {
    display: flex;
    justify-content: space-between;
    width: 100%;
    max-width: 340px;
    background: #242632;
    border: 1px solid #2f3245;
    padding: 10px 15px;
    border-radius: 6px;
    margin-bottom: 15px;
    font-weight: bold;
    font-size: 14px;
    text-transform: uppercase;
  }
  .stat-val { color: #00f0ff; }
  .timer-val { color: #ff007a; }

  /* Tetris Canvas Container */
  .canvas-wrapper {
    position: relative;
    background: #13141c;
    border: 4px solid #2f3245;
    border-radius: 8px;
    box-shadow: 0 0 20px rgba(0, 240, 255, 0.1);
  }
  canvas {
    display: block;
    background: #13141c;
  }

  /* Game Controls Overlays & Buttons */
  .control-panel {
    margin-top: 15px;
    width: 100%;
    max-width: 340px;
    display: flex;
    gap: 10px;
  }
  .game-btn {
    flex: 1;
    background: #00e5ff;
    color: #000;
    border: none;
    padding: 12px;
    font-weight: bold;
    border-radius: 6px;
    cursor: pointer;
    text-transform: uppercase;
    box-shadow: 0 4px 0 #00a8bc;
    font-size: 13px;
  }
  .game-btn:active { transform: translateY(2px); box-shadow: 0 2px 0 #00a8bc; }
  
  /* Mobile Touch Controls Overlay */
  .d-pad {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    width: 100%;
    max-width: 240px;
    margin-top: 20px;
  }
  .pad-btn {
    background: #242632;
    border: 1px solid #2f3245;
    color: #fff;
    padding: 15px 0;
    text-align: center;
    border-radius: 6px;
    font-weight: bold;
    font-size: 18px;
    cursor: pointer;
    user-select: none;
    box-shadow: 0 3px 0 #13141c;
  }
  .pad-btn:active { transform: translateY(2px); box-shadow: 0 1px 0 #13141c; }

  @media (max-width: 480px) {
    .game-header { max-width: 300px; font-size: 12px; }
    .control-panel { max-width: 300px; }
  }
</style>

<div class="arcade-container">
  <h1 style="text-align:center; font-size: 22px; color: #ffffff; margin-bottom: 10px; text-transform: uppercase; letter-spacing: 1px;">🧩 PROTETRIS</h1>
  <p style="text-align:center; font-size: 12px; color: #94a3b8; margin: 0 0 15px 0; text-transform: uppercase;">Survive 3 minutes. Points are auto-credited upon completion.</p>

  <div class="game-header">
    <div>TIME: <span id="gameTimer" class="timer-val">03:00</span></div>
    <div>SCORE: <span id="gameScore" class="stat-val">0</span></div>
  </div>

  <div class="canvas-wrapper">
    <canvas id="tetris" width="240" height="480"></canvas>
  </div>

  <div class="control-panel">
    <button id="startBtn" class="game-btn">START MATRIX</button>
  </div>

  <!-- Mobile Adaptive Navigation D-Pad -->
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
  context.scale(20, 20);

  const ROW = 24;
  const COL = 12;
  const VACANT = "#13141c"; 

  let score = 0;
  let gameDuration = 180;
  let timerInterval = null;
  let gameInterval = null;
  let isPlaying = false;
  let board = [];

  function initBoard() {
    for (let r = 0; r < ROW; r++) {
      board[r] = [];
      for (let c = 0; c < COL; c++) {
        board[r][c] = VACANT;
      }
    }
  }

  function drawSquare(x, y, color) {
    context.fillStyle = color;
    context.fillRect(x, y, 1, 1);
    context.strokeStyle = "#1a1b23";
    context.lineWidth = 0.05;
    context.strokeRect(x, y, 1, 1);
  }

  // Ekranı temizleyip her şeyi sıfırdan çizen ana render fonksiyonu
  function drawLayout() {
    context.clearRect(0, 0, COL, ROW); // Canvas'ı temizler, akışı pürüzsüzleştirir
    for (let r = 0; r < ROW; r++) {
      for (let c = 0; c < COL; c++) {
        drawSquare(c, r, board[r][c]);
      }
    }
    if (isPlaying && p) {
      p.draw();
    }
  }

  const PIECES = [
    [[1,1,1,1], [0,0,0,0], [0,0,0,0], [0,0,0,0]], // I
    [[1,1,1], [0,1,0], [0,0,0]],                 // T
    [[1,1,1], [1,0,0], [0,0,0]],                 // L
    [[1,1,1], [0,0,1], [0,0,0]],                 // J
    [[0,1,1], [1,1,0], [0,0,0]],                 // S
    [[1,1,0], [0,1,1], [0,0,0]],                 // Z
    [[1,1], [1,1]]                               // O
  ];
  const COLORS = ["#00f0ff", "#ff007a", "#00ff88", "#ffb700", "#9d00ff", "#ff003c", "#0026ff"];

  function Piece(tetromino, color) {
    this.tetromino = tetromino;
    this.color = color;
    this.activeTetromino = this.tetromino[0];
    this.tetrominoN = 0;
    this.x = 4;
    this.y = 0; // Taşma problemini çözmek için başlangıç noktası düzeltildi
  }

  Piece.prototype.draw = function() {
    for (let r = 0; r < this.activeTetromino.length; r++) {
      for (let c = 0; c < this.activeTetromino[r].length; c++) {
        if (this.activeTetromino[r][c]) {
          drawSquare(this.x + c, this.y + r, this.color);
        }
      }
    }
  };

  Piece.prototype.moveDown = function() {
    if (!this.collision(0, 1, this.activeTetromino)) {
      this.y++;
      drawLayout();
    } else {
      this.lock();
      p = randomPiece();
    }
  };

  Piece.prototype.moveRight = function() {
    if (!this.collision(1, 0, this.activeTetromino)) {
      this.x++;
      drawLayout();
    }
  };

  Piece.prototype.moveLeft = function() {
    if (!this.collision(-1, 0, this.activeTetromino)) {
      this.x--;
      drawLayout();
    }
  };

  Piece.prototype.rotate = function() {
    let nextPattern = this.tetromino[(this.tetrominoN + 1) % this.tetromino.length];
    if (!this.collision(0, 0, nextPattern)) {
      this.tetrominoN = (this.tetrominoN + 1) % this.tetromino.length;
      this.activeTetromino = nextPattern;
      drawLayout();
    }
  };

  Piece.prototype.collision = function(x, y, piece) {
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
    for (let r = 0; r < this.activeTetromino.length; r++) {
      for (let c = 0; c < this.activeTetromino[r].length; c++) {
        if (!this.activeTetromino[r][c]) continue;
        if (this.y + r < 0) {
          endGame(false);
          return;
        }
        board[this.y + r][this.x + c] = this.color;
      }
    }
    
    // Satır patlatma algoritması
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

    // Yeni gelen taş anında çarparsa oyunu bitir
    if (p.collision(0, 0, p.activeTetromino)) {
      endGame(false);
    }
  };

  function randomPiece() {
    let r = Math.floor(Math.random() * PIECES.length);
    return new Piece(PIECES[r], COLORS[r]);
  }

  let p = null;

  function endGame(timeExpired = false) {
    isPlaying = false;
    clearInterval(gameInterval);
    clearInterval(timerInterval);
    document.getElementById('startBtn').disabled = false;
    document.getElementById('startBtn').innerText = "RUN MATRIX AGAIN";

    const dynamicReward = parseFloat((score * 0.001).toFixed(2));

    if (timeExpired) {
      alert(`⏱️ TIME EXPIRED! Match score: ${score}. Processing calculated payload...`);
    } else {
      alert(`💥 MATRIX COLLAPSE! Game over. Match score: ${score}. Processing calculated payload...`);
    }

    if (dynamicReward > 0 && currentUser) {
      const userRef = doc(db, "users", currentUser.uid);
      runTransaction(db, async (transaction) => {
        const userDoc = await transaction.get(userRef);
        let currentBalance = userDoc.exists() ? (userDoc.data().balance || 0) : 0;
        transaction.update(userRef, { balance: currentBalance + dynamicReward });
      }).then(() => {
        alert(`🎁 SUCCESS! +${dynamicReward} Points successfully written to your linked Web3 Arcade core account.`);
      }).catch(err => console.error("Database secure writing failure:", err));
    } else if (!currentUser) {
      alert("⚠️ NOTICE: You are not authenticated via Google. Session score discarded!");
    }
  }

  function startGame() {
    if (isPlaying) return;
    initBoard();
    score = 0;
    gameDuration = 180;
    isPlaying = true;
    document.getElementById('gameScore').innerText = score;
    document.getElementById('startBtn').disabled = true;
    document.getElementById('startBtn').innerText = "MATRIX ENGAGED";

    p = randomPiece();
    drawLayout();

    // Aşağı akış hız döngüsü (450ms standardı)
    gameInterval = setInterval(() => {
      if (isPlaying) p.moveDown();
    }, 450);

    timerInterval = setInterval(() => {
      gameDuration--;
      let mins = Math.floor(gameDuration / 60);
      let secs = gameDuration % 60;
      document.getElementById('gameTimer').innerText = `${mins < 10 ? '0' : ''}${mins}:${secs < 10 ? '0' : ''}${secs}`;

      if (gameDuration <= 0) {
        endGame(true);
      }
    }, 1000);
  }

  document.getElementById('startBtn').addEventListener('click', startGame);

  document.addEventListener('keydown', (e) => {
    if (!isPlaying) return;
    if (e.key === "ArrowLeft" || e.key.toLowerCase() === "a") p.moveLeft();
    else if (e.key === "ArrowUp" || e.key.toLowerCase() === "w") p.rotate();
    else if (e.key === "ArrowRight" || e.key.toLowerCase() === "d") p.moveRight();
    else if (e.key === "ArrowDown" || e.key.toLowerCase() === "s") p.moveDown();
  });

  document.getElementById('btnLeft').addEventListener('click', () => { if (isPlaying) p.moveLeft(); });
  document.getElementById('btnRight').addEventListener('click', () => { if (isPlaying) p.moveRight(); });
  document.getElementById('btnRotate').addEventListener('click', () => { if (isPlaying) p.rotate(); });
  document.getElementById('btnDown').addEventListener('click', () => { if (isPlaying) p.moveDown(); });
  document.getElementById('btnUp').addEventListener('click', () => {
    if (isPlaying) {
      while (!p.collision(0, 1, p.activeTetromino)) {
        p.y++;
      }
      p.lock();
      p = randomPiece();
    }
  });
</script>
