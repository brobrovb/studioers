---
layout: default
title: Free Crypto Arcade
---

<style>
  /* Global CSS Styling & Absolute Mobile Constraint */
  html, body {
    background-color: #1a1b23 !important;
    color: #e2e8f0 !important;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    box-sizing: border-box;
    margin: 0 !important;
    padding: 0 !important;
    width: 100% !important;
    overflow-x: hidden !important; /* Prevents layout breakdown on mobile viewports */
    -webkit-text-size-adjust: 100%;
  }

  *, *:before, *:after {
    box-sizing: inherit;
  }
  
  /* Force Jekyll theme structure to respect layout boundaries and eliminate hidden padding overflow */
  .site-header, .site-footer, .page-content, .wrapper, .arcade-body {
    background-color: #1a1b23 !important;
    max-width: 100% !important;
    width: 100% !important;
    overflow-x: hidden !important;
    margin-left: auto !important;
    margin-right: auto !important;
  }
  
  /* Desktop constraints for readability */
  .wrapper { 
    max-width: 1200px !important; 
    box-shadow: none !important; 
    border: none !important; 
    padding: 0 12px !important;
  }
  
  .site-title, .site-title:visited, .site-nav .page-link { 
    color: #00f0ff !important; 
    font-weight: bold; 
    text-transform: uppercase; 
  }
  
  .arcade-body { 
    padding: 10px 0; 
  }

  /* Authentication Control Bar */
  .auth-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #242632;
    padding: 10px 15px;
    border-radius: 6px;
    border: 1px solid #2f3245;
    margin-bottom: 20px;
    gap: 10px;
    width: 100%;
  }
  .user-info { 
    display: flex; 
    align-items: center; 
    gap: 8px; 
    font-weight: bold; 
    font-size: 13px;
    overflow: hidden;
    text-overflow: ellipsis; white-space: nowrap;
  }
  .user-avatar { width: 28px; height: 28px; border-radius: 50%; border: 2px solid #00f0ff; flex-shrink: 0; }
  
  .auth-btn {
    background: #00e5ff; color: #000; border: none; padding: 6px 12px;
    font-weight: bold; border-radius: 4px; cursor: pointer; text-transform: uppercase;
    box-shadow: 0 3px 0 #00a8bc; font-size: 11px;
    white-space: nowrap;
    flex-shrink: 0;
  }
  .auth-btn:active { transform: translateY(2px); box-shadow: 0 1px 0 #00a8bc; }
  .logout-btn { background: #ff007a; color: #fff; box-shadow: 0 3px 0 #b00052; }
  .logout-btn:active { box-shadow: 0 1px 0 #b00052; }

  /* Dashboard Telemetry Metrics - Wallet Tek Başına Tam Genişlik */
  .stats-container { 
    display: flex; 
    margin-bottom: 25px; 
    width: 100%;
  }
  .stat-card { 
    background: #242632; 
    border: 1px solid #2f3245; 
    border-left: 5px solid #ff007a; 
    padding: 15px; 
    border-radius: 6px; 
    width: 100%;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2); 
  }
  .stat-card h5 { margin: 0; color: #94a3b8; text-transform: uppercase; font-size: 11px; letter-spacing: 0.5px; }
  .stat-card p { margin: 6px 0 0 0; font-size: 22px; font-weight: bold; color: #ff007a; }

  /* Optimized Responsive Game Grid */
  .game-grid { 
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); 
    gap: 15px; 
    margin-top: 20px; 
    width: 100%;
  }
  
  /* Individual Game Interface Component */
  .rc-game-card { 
    background: #242632; 
    border: 1px solid #2f3245; 
    border-radius: 8px; 
    padding: 12px; 
    display: flex; 
    gap: 12px; 
    align-items: center; 
    box-shadow: 0 4px 10px rgba(0,0,0,0.15); 
    width: 100%;
  }
  .rc-game-image { 
    width: 70px; 
    height: 70px; 
    background: #13141c; 
    border-radius: 8px; 
    border: 1px solid #2f3245; 
    display: flex; 
    align-items: center; 
    justify-content: center; 
    font-size: 30px; 
    box-shadow: inset 0 0 10px rgba(0,0,0,0.6); 
    flex-shrink: 0;
  }
  .rc-game-details { flex: 1; display: flex; flex-direction: column; gap: 2px; min-width: 0; }
  .rc-game-name { margin: 0; font-size: 14px; font-weight: bold; color: #ffffff; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .rc-difficulty-label { font-size: 10px; color: #00f0ff; margin: 0; text-transform: uppercase; }
  .rc-difficulty-bar { display: flex; gap: 2px; margin-bottom: 4px; }
  .rc-dot { width: 8px; height: 4px; background: #3a3f58; border-radius: 1px; }
  .rc-dot.active { background: #ff007a; box-shadow: 0 0 4px #ff007a; }
  
  .rc-start-btn { 
    background: #00e5ff; color: #000; border: none; padding: 6px 0; border-radius: 6px; 
    font-weight: bold; font-size: 11px; text-align: center; text-decoration: none; 
    text-transform: uppercase; box-shadow: 0 3px 0 #00a8bc; display: block; width: 100%; 
  }
  .rc-start-btn:active { transform: translateY(3px); box-shadow: 0 1px 0 #00a8bc; }

  /* Faucet Interaction Space */
  .faucet-section { margin: 30px 0; padding: 20px 15px; background: #242632; border: 2px dashed #ff007a; border-radius: 8px; text-align: center; width: 100%; }
  .faucet-btn { background: #ff007a; color: #fff; border: none; padding: 10px 35px; font-size: 14px; border-radius: 6px; cursor: pointer; font-weight: bold; text-transform: uppercase; box-shadow: 0 4px 0 #b00052; max-width: 100%; }
  .faucet-btn:active { transform: translateY(3px); box-shadow: 0 1px 0 #b00052; }
  .faucet-btn:disabled { background: #4e5268 !important; box-shadow: none !important; cursor: not-allowed; color: #aaa; }

  /* Cyberpunk Leaderboard UI Panel */
  .leaderboard-section { margin: 30px 0; padding: 20px 15px; background: #242632; border: 1px solid #2f3245; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); width: 100%; }
  .leaderboard-table { width: 100%; border-collapse: collapse; text-align: left; font-size: 13px; margin-top: 10px; }
  .leaderboard-table th { color: #00f0ff; text-transform: uppercase; font-size: 11px; padding: 10px; border-bottom: 2px solid #2f3245; letter-spacing: 0.5px; }
  .leaderboard-table td { padding: 10px; border-bottom: 1px solid #1e202b; vertical-align: middle; }
  .leaderboard-table tr:hover { background: #1e202b; }
  .rank-badge { font-weight: bold; display: inline-block; width: 22px; height: 22px; line-height: 22px; text-align: center; border-radius: 4px; background: #13141c; color: #94a3b8; }
  .rank-1 { background: #ffd700 !important; color: #000 !important; box-shadow: 0 0 8px #ffd700; }
  .rank-2 { background: #c0c0c0 !important; color: #000 !important; }
  .rank-3 { background: #cd7f32 !important; color: #000 !important; }
  .leader-user { display: flex; align-items: center; gap: 8px; font-weight: 600; max-width: 180px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .leader-avatar { width: 24px; height: 24px; border-radius: 50%; border: 1px solid #2f3245; background: #13141c; }

  /* Adsterra/Coinzilla Banner Placements */
  .ad-container {
    margin: 20px auto;
    text-align: center;
    width: 100%;
    min-height: 90px;
    background: #1e202b;
    border: 1px solid #2f3245;
    border-radius: 6px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .ad-label {
    font-size: 9px;
    color: #4f5675;
    letter-spacing: 1px;
    margin-bottom: 4px;
    text-transform: uppercase;
  }

  /* Targeted Media Queries Adjusting Flex Rules For Specific Mobile Widths */
  @media (max-width: 580px) {
    h1 { font-size: 18px !important; margin-bottom: 15px !important; }
    .stat-card p { font-size: 19px !important; }
    .game-grid { grid-template-columns: 1fr !important; gap: 12px !important; }
    .rc-game-card { padding: 10px !important; gap: 10px !important; }
    .rc-game-image { width: 60px !important; height: 60px !important; font-size: 24px !important; }
    .leader-user { max-width: 110px !important; }
  }
</style>

<div class="arcade-body">

  <div class="ad-container">
    <span class="ad-label">Sponsored Mining Network</span>
    </div>

  <div class="auth-bar">
    <div id="authUser" class="user-info">
      <span style="color: #94a3b8; font-size: 14px;">Checking status...</span>
    </div>
    <button id="authBtn" class="auth-btn">Sign In with Google</button>
  </div>

  <h1 style="text-align:center; font-size: 24px; color: #ffffff; margin-bottom: 25px;">🎮 STUDIOERS ARCADE STATION</h1>

  <div class="stats-container">
    <div class="stat-card">
      <h5>Your Wallet Allocation</h5>
      <p id="userBalance">0.00 Points</p>
    </div>
  </div>

  <h3 style="border-bottom: 1px solid #2f3245; padding-bottom: 10px; color: #94a3b8; font-size: 16px; text-transform: uppercase;">🕹️ Arcade Lobby</h3>

  <div class="game-grid">
    <div class="rc-game-card">
      <div class="rc-game-image" style="text-shadow: 0 0 10px #00f0ff;">⚡</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Coin-match</h4>
        <p class="rc-difficulty-label">difficulty: 3</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div></div>
        <a href="/crypto-matcher" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
    
    <div class="rc-game-card">
      <div class="rc-game-image" style="text-shadow: 0 0 10px #ff007a;">🧩</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Protetris</h4>
        <p class="rc-difficulty-label">difficulty: 5</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div><div class="rc-dot"></div></div>
        <a href="/protetris" class="rc-start-btn" style="background:#ff007a; box-shadow: 0 3px 0 #b00052; color:white;">🏁 START</a>
      </div>
    </div>
    
    <div class="rc-game-card">
      <div class="rc-game-image">🚀</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Token Blaster</h4>
        <p class="rc-difficulty-label">difficulty: 4</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div></div>
        <a href="#" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
    <div class="rc-game-card">
      <div class="rc-game-image">🐹</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Hamster Surfer</h4>
        <p class="rc-difficulty-label">difficulty: 2</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div></div>
        <a href="#" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
    <div class="rc-game-card">
      <div class="rc-game-image">🎣</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Coin Fisher</h4>
        <p class="rc-difficulty-label">difficulty: 6</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div></div>
        <a href="#" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
    <div class="rc-game-card">
      <div class="rc-game-image">🐦</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Flappy Rocket</h4>
        <p class="rc-difficulty-label">difficulty: 7</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div></div>
        <a href="#" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
    <div class="rc-game-card">
      <div class="rc-game-image">🧱</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Block Blocker</h4>
        <p class="rc-difficulty-label">difficulty: 1</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div></div>
        <a href="#" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
    <div class="rc-game-card">
      <div class="rc-game-image">🔢</div>
      <div class="rc-game-details">
        <h4 class="rc-game-name">Crypto 2048</h4>
        <p class="rc-difficulty-label">difficulty: 4</p>
        <div class="rc-difficulty-bar"><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot active"></div><div class="rc-dot"></div><div class="rc-dot"></div><div class="rc-dot"></div></div>
        <a href="#" class="rc-start-btn">🏁 START</a>
      </div>
    </div>
  </div>

  <div class="leaderboard-section">
    <h3 style="margin-top:0; color:#00f0ff; font-size:16px; text-transform:uppercase; border-bottom:1px solid #2f3245; padding-bottom:8px; letter-spacing:1px;">🏆 TOP 10 BALANCES</h3>
    <table class="leaderboard-table">
      <thead>
        <tr>
          <th style="width: 50px;">Rank</th>
          <th>Miner</th>
          <th style="text-align: right;">Wallet Allocation</th>
        </tr>
      </thead>
      <tbody id="leaderboardBody">
        <tr>
          <td colspan="3" style="text-align:center; color:#94a3b8; padding:20px;">Syncing with decentralized network matrix...</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="faucet-section">
    <h3 style="margin-top:0; color:#ff007a;">🎁 Hourly Energy Refill</h3>
    <p style="color:#94a3b8; font-size:14px; margin-bottom:15px;">Claim an instant +5.00 points bonus directly to your wallet allocation.</p>
    <button id="faucetBtn" class="faucet-btn">CLAIM BONUS</button>
    <p id="faucetMsg" style="margin-top: 12px; font-weight: bold; color: #00ff88; min-height: 20px;"></p>
  </div>

  <div class="ad-container" style="margin-top: 25px;">
    <span class="ad-label">Hardware Allocation Sponsor</span>
    </div>

</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getAuth, signInWithPopup, signInWithRedirect, getRedirectResult, signOut, onAuthStateChanged, GoogleAuthProvider } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
  import { getFirestore, doc, onSnapshot, setDoc, runTransaction, collection, query, orderBy, limit } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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
  const provider = new GoogleAuthProvider();

  provider.setCustomParameters({ prompt: 'select_account' });

  let currentUser = null;
  let countdownInterval = null;

  const authBtn = document.getElementById('authBtn');
  const authUserDiv = document.getElementById('authUser');
  const userBalanceText = document.getElementById('userBalance');
  const faucetBtn = document.getElementById('faucetBtn');
  const faucetMsg = document.getElementById('faucetMsg');
  const leaderboardBody = document.getElementById('leaderboardBody');

  const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) || window.innerWidth < 768;

  if (isMobile) {
    getRedirectResult(auth)
      .then((result) => {
        if (result?.user) {
          console.log("Redirect login successful:", result.user);
        }
      })
      .catch((error) => {
        console.error("Redirect authentication error:", error);
      });
  }

  function startFaucetCountdown(durationInSeconds) {
    if (countdownInterval) clearInterval(countdownInterval);
    
    faucetBtn.disabled = true;
    
    let timer = durationInSeconds;
    countdownInterval = setInterval(() => {
      const minutes = Math.floor(timer / 60);
      const seconds = timer % 60;
      
      const displayMinutes = minutes < 10 ? "0" + minutes : minutes;
      const displaySeconds = seconds < 10 ? "0" + seconds : seconds;
      
      faucetBtn.innerText = `NEXT CLAIM IN ${displayMinutes}:${displaySeconds}`;
      
      if (--timer < 0) {
        clearInterval(countdownInterval);
        faucetBtn.disabled = false;
        faucetBtn.innerText = "CLAIM BONUS";
        faucetMsg.innerText = "";
        localStorage.removeItem('faucetNextClaim');
      }
    }, 1000);
  }

  function checkExistingTimer() {
    const nextClaimTime = localStorage.getItem('faucetNextClaim');
    if (nextClaimTime) {
      const currentTime = Date.now();
      const timeLeft = Math.floor((parseInt(nextClaimTime) - currentTime) / 1000);
      
      if (timeLeft > 0) {
        startFaucetCountdown(timeLeft);
      } else {
        localStorage.removeItem('faucetNextClaim');
      }
    }
  }

  checkExistingTimer();

  // --- REAL-TIME LEADERBOARD SENSOR ---
  function initLeaderboard() {
    const usersRef = collection(db, "users");
    const q = query(usersRef, orderBy("balance", "desc"), limit(10));
    
    onSnapshot(q, (snapshot) => {
      leaderboardBody.innerHTML = "";
      if (snapshot.empty) {
        leaderboardBody.innerHTML = `<tr><td colspan="3" style="text-align:center; color:#94a3b8;">No miners found in core database matrix.</td></tr>`;
        return;
      }
      
      let index = 1;
      snapshot.forEach((docSnap) => {
        const data = docSnap.data();
        const userId = docSnap.id;
        
        const displayName = data.displayName || (auth.currentUser && auth.currentUser.uid === userId ? auth.currentUser.displayName : `Gamer_${userId.substring(0, 4)}`);
        const photoURL = data.photoURL || (auth.currentUser && auth.currentUser.uid === userId ? auth.currentUser.photoURL : "https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/anonymous.png");
        const balance = parseFloat(data.balance || 0).toFixed(2);
        
        let rankClass = "";
        if (index === 1) rankClass = "rank-1";
        else if (index === 2) rankClass = "rank-2";
        else if (index === 3) rankClass = "rank-3";
        
        const row = document.createElement('tr');
        if (currentUser && currentUser.uid === userId) {
          row.style.background = "rgba(0, 240, 255, 0.08)";
          row.style.borderLeft = "2px solid #00f0ff";
        }
        
        row.innerHTML = `
          <td><span class="rank-badge ${rankClass}">${index}</span></td>
          <td>
            <div class="leader-user">
              <img src="${photoURL}" class="leader-avatar" alt="avatar" onerror="this.src='https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/anonymous.png'">
              <span>${displayName}</span>
            </div>
          </td>
          <td style="text-align: right; font-weight: bold; color: #ff007a;">${balance} Points</td>
        `;
        leaderboardBody.appendChild(row);
        index++;
      });
    }, (error) => {
      console.error("Leaderboard subscription matrix error:", error);
    });
  }

  initLeaderboard();

  onAuthStateChanged(auth, (user) => {
    if (user) {
      currentUser = user;
      authBtn.innerText = "Sign Out";
      authBtn.classList.add('logout-btn');
      authUserDiv.innerHTML = `<img src="${user.photoURL}" class="user-avatar" alt="avatar"> <span>${user.displayName}</span>`;
      
      const userRef = doc(db, "users", user.uid);
      onSnapshot(userRef, (snapshot) => {
        if (snapshot.exists()) {
          const data = snapshot.data();
          userBalanceText.innerText = parseFloat(data.balance || 0).toFixed(2) + " Points";
          
          if (!data.displayName || !data.photoURL) {
            setDoc(userRef, { 
              displayName: user.displayName, 
              photoURL: user.photoURL 
            }, { merge: true }).catch(err => console.error("Update profile metadata error:", err));
          }
        } else {
          setDoc(userRef, { 
            balance: 0, 
            displayName: user.displayName, 
            photoURL: user.photoURL 
          }).catch(err => console.error("Database initialization error:", err));
        }
      }, (error) => {
        console.error("Firestore subscription error:", error);
      });
    } else {
      currentUser = null;
      authBtn.innerText = "Sign In with Google";
      authBtn.classList.remove('logout-btn');
      authUserDiv.innerHTML = `<span style="color: #94a3b8; font-size: 14px;">Not authenticated. Scores will not be tracked!</span>`;
      userBalanceText.innerText = "0.00 Points";
    }
  });

  authBtn.addEventListener('click', () => {
    if (!currentUser) {
      if (isMobile) {
        signInWithRedirect(auth, provider);
      } else {
        signInWithPopup(auth, provider)
          .then((result) => { console.log("Desktop login successful:", result.user); })
          .catch((error) => {
            console.error("Authentication error:", error);
            if (error.code === 'auth/popup-blocked') {
              alert("Popup blocked by browser! Please enable popups or access via mobile device.");
            }
          });
      }
    } else {
      signOut(auth).catch(err => console.error("Sign out error:", err));
    }
  });

  faucetBtn.addEventListener('click', function() {
    if (!currentUser) {
      alert("Please authenticate using Google before claiming rewards!");
      return;
    }
    
    const userRef = doc(db, "users", currentUser.uid);
    
    faucetBtn.disabled = true;
    faucetBtn.innerText = "PROCESSING...";

    runTransaction(db, async (transaction) => {
      const userDoc = await transaction.get(userRef);
      let currentBalance = userDoc.exists() ? (userDoc.data().balance || 0) : 0;
      transaction.update(userRef, { 
        balance: currentBalance + 5.00,
        displayName: currentUser.displayName,
        photoURL: currentUser.photoURL
      });
    }).then(() => {
      faucetMsg.innerText = "⚡ Core Refilled! +5.00 Points saved.";
      
      const oneHourInSeconds = 3600;
      const nextClaimTimestamp = Date.now() + (oneHourInSeconds * 1000);
      localStorage.setItem('faucetNextClaim', nextClaimTimestamp);
      
      startFaucetCountdown(oneHourInSeconds);
    }).catch(err => {
      console.error("Transaction processing error:", err);
      faucetBtn.disabled = false;
      faucetBtn.innerText = "CLAIM BONUS";
      faucetMsg.innerText = "Error processing transaction. Try again.";
    });
  });
</script>
