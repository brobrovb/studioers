---
layout: default
title: Free Crypto Arcade
---

<style>
  html, body, .site-header, .site-footer, .page-content, .wrapper {
    background-color: #1a1b23 !important;
    color: #e2e8f0 !important;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
  .wrapper { max-width: 1200px !important; box-shadow: none !important; border: none !important; }
  .site-title, .site-title:visited, .site-nav .page-link { color: #00f0ff !important; font-weight: bold; text-transform: uppercase; }
  .arcade-body { padding: 10px 0; }

  /* Giriş Paneli Üst Alanı */
  .auth-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #242632;
    padding: 10px 20px;
    border-radius: 6px;
    border: 1px solid #2f3245;
    margin-bottom: 20px;
  }
  .user-info { display: flex; align-items: center; gap: 10px; font-weight: bold; }
  .user-avatar { width: 32px; height: 32px; border-radius: 50%; border: 2px solid #00f0ff; }
  
  .auth-btn {
    background: #00e5ff; color: #000; border: none; padding: 6px 15px;
    font-weight: bold; border-radius: 4px; cursor: pointer; text-transform: uppercase;
    box-shadow: 0 3px 0 #00a8bc; font-size: 12px;
  }
  .auth-btn:active { transform: translateY(2px); box-shadow: 0 1px 0 #00a8bc; }
  .logout-btn { background: #ff007a; color: #fff; box-shadow: 0 3px 0 #b00052; }
  .logout-btn:active { box-shadow: 0 1px 0 #b00052; }

  /* Göstergeler */
  .stats-container { display: flex; gap: 15px; justify-content: space-between; margin-bottom: 35px; flex-wrap: wrap; }
  .stat-card { background: #242632; border: 1px solid #2f3245; border-left: 5px solid #00f0ff; padding: 15px 20px; border-radius: 6px; flex: 1; min-width: 220px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); }
  .stat-card h5 { margin: 0; color: #94a3b8; text-transform: uppercase; font-size: 11px; letter-spacing: 1px; }
  .stat-card p { margin: 6px 0 0 0; font-size: 24px; font-weight: bold; color: #00f0ff; }
  .stat-card.balance-card { border-left-color: #ff007a; }
  .stat-card.balance-card p { color: #ff007a; }

  /* 8'li Oyun Izgarası */
  .game-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 20px; margin-top: 20px; }
  .rc-game-card { background: #242632; border: 1px solid #2f3245; border-radius: 8px; padding: 15px; display: flex; gap: 15px; align-items: center; box-shadow: 0 4px 10px rgba(0,0,0,0.15); }
  .rc-game-image { width: 85px; height: 85px; background: #13141c; border-radius: 8px; border: 1px solid #2f3245; display: flex; align-items: center; justify-content: center; font-size: 38px; box-shadow: inset 0 0 10px rgba(0,0,0,0.6); }
  .rc-game-details { flex: 1; display: flex; flex-direction: column; gap: 4px; }
  .rc-game-name { margin: 0; font-size: 15px; font-weight: bold; color: #ffffff; }
  .rc-difficulty-label { font-size: 11px; color: #00f0ff; margin: 0; text-transform: uppercase; }
  .rc-difficulty-bar { display: flex; gap: 3px; margin-bottom: 6px; }
  .rc-dot { width: 10px; height: 4px; background: #3a3f58; border-radius: 1px; }
  .rc-dot.active { background: #ff007a; box-shadow: 0 0 4px #ff007a; }
  
  .rc-start-btn { background: #00e5ff; color: #000; border: none; padding: 7px 0; border-radius: 6px; font-weight: bold; font-size: 12px; text-align: center; text-decoration: none; text-transform: uppercase; box-shadow: 0 3px 0 #00a8bc; display: block; width: 100%; }
  .rc-start-btn:active { transform: translateY(3px); box-shadow: 0 1px 0 #00a8bc; }

  /* Musluk */
  .faucet-section { margin: 40px 0; padding: 25px; background: #242632; border: 2px dashed #ff007a; border-radius: 8px; text-align: center; }
  .faucet-btn { background: #ff007a; color: #fff; border: none; padding: 12px 40px; font-size: 15px; border-radius: 6px; cursor: pointer; font-weight: bold; text-transform: uppercase; box-shadow: 0 4px 0 #b00052; }
  .faucet-btn:active { transform: translateY(3px); box-shadow: 0 1px 0 #b00052; }
  .faucet-btn:disabled { background: #4e5268 !important; box-shadow: none !important; cursor: not-allowed; color: #aaa; }
</style>

<div class="arcade-body">

  <div class="auth-bar">
    <div id="authUser" class="user-info">
      <span style="color: #94a3b8; font-size: 14px;">Kontrol ediliyor...</span>
    </div>
    <button id="authBtn" class="auth-btn">Google ile Giriş Yap</button>
  </div>

  <h1 style="text-align:center; font-size: 24px; color: #ffffff; margin-bottom: 25px;">🎮 STUDIOERS ARCADE STATION</h1>

  <div class="stats-container">
    <div class="stat-card">
      <h5>Your Mining Power</h5>
      <p id="userPower">0.000 Th/s</p>
    </div>
    <div class="stat-card">
      <h5>Network Power</h5>
      <p>1,420.85 Ph/s</p>
    </div>
    <div class="stat-card balance-card">
      <h5>Your Wallet</h5>
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
        <a href="#" class="rc-start-btn" style="background:#ff007a; box-shadow: 0 3px 0 #b00052; color:white;">🏁 START</a>
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

  <div class="faucet-section">
    <h3 style="margin-top:0; color:#ff007a;">🎁 Hourly Energy Refill</h3>
    <p style="color:#94a3b8; font-size:14px; margin-bottom:15px;">Claim an instant +5.00 points bonus directly to your wallet allocation.</p>
    <button id="faucetBtn" class="faucet-btn">CLAIM BONUS</button>
    <p id="faucetMsg" style="margin-top: 12px; font-weight: bold; color: #00ff88; min-height: 20px;"></p>
  </div>
</div>

<script type="module">
  // Modüler Firebase v10 paketlerini import ediyoruz
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
  import { getAuth, signInWithPopup, signOut, onAuthStateChanged, GoogleAuthProvider } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
  import { getFirestore, doc, onSnapshot, setDoc, runTransaction } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

  const firebaseConfig = {
    apiKey: "AIzaSyDi7xosmyNGJELn4KOpe8QEg5tNewkIsEc",
    authDomain: "studioers-arcade.firebaseapp.com",
    projectId: "studioers-arcade",
    storageBucket: "studioers-arcade.firebasestorage.app",
    messagingSenderId: "1096473829075",
    appId: "1:1096473829075:web:a0f0e3023ab7ac02847e26",
    measurementId: "G-0HYW1H5FV2"
  };

  // Firebase Başlatılıyor
  const app = initializeApp(firebaseConfig);
  const auth = getAuth(app);
  const db = getFirestore(app);
  const provider = new GoogleAuthProvider();

  // Tarayıcının popup'ı engellemesini önlemek için custom ayar
  provider.setCustomParameters({ prompt: 'select_account' });

  let currentUser = null;

  const authBtn = document.getElementById('authBtn');
  const authUserDiv = document.getElementById('authUser');
  const userPowerText = document.getElementById('userPower');
  const userBalanceText = document.getElementById('userBalance');

  // Kullanıcı Durum Dinleyicisi
  onAuthStateChanged(auth, (user) => {
    if (user) {
      currentUser = user;
      authBtn.innerText = "Çıkış Yap";
      authBtn.classList.add('logout-btn');
      authUserDiv.innerHTML = `<img src="${user.photoURL}" class="user-avatar" alt="avatar"> <span>${user.displayName}</span>`;
      
      // Buluttan veriyi gerçek zamanlı çekme
      const userRef = doc(db, "users", user.uid);
      onSnapshot(userRef, (snapshot) => {
        if (snapshot.exists()) {
          const data = snapshot.data();
          userPowerText.innerText = parseFloat(data.power || 0).toFixed(3) + " Th/s";
          userBalanceText.innerText = parseFloat(data.balance || 0).toFixed(2) + " Points";
        } else {
          // Doküman yoksa oluştur
          setDoc(userRef, { power: 0, balance: 0 }).catch(err => console.error("Kayıt Hatası:", err));
        }
      }, (error) => {
        console.error("Firestore Dinleme Hatası:", error);
      });
    } else {
      currentUser = null;
      authBtn.innerText = "Google ile Giriş Yap";
      authBtn.classList.remove('logout-btn');
      authUserDiv.innerHTML = `<span style="color: #94a3b8; font-size: 14px;">Giriş yapılmadı. Skorlar kaydedilmez!</span>`;
      userPowerText.innerText = "0.000 Th/s";
      userBalanceText.innerText = "0.00 Points";
    }
  });

  // Butona tıklandığında popup penceresini tetikliyoruz
  authBtn.addEventListener('click', () => {
    if (!currentUser) {
      signInWithPopup(auth, provider)
        .then((result) => {
          console.log("Başarıyla giriş yapıldı:", result.user);
        })
        .catch((error) => {
          console.error("Giriş Hatası:", error);
          if (error.code === 'auth/popup-blocked') {
            alert("Kanka tarayıcın açılır pencereyi engelledi! Lütfen adres çubuğunun sağındaki pop-up engelleyiciden bu siteye izin ver.");
          } else {
            alert("Giriş başarısız oldu. Hata: " + error.message);
          }
        });
    } else {
      signOut(auth).catch(err => console.error("Çıkış Hatası:", err));
    }
  });

  // Faucet Buton İşlemi
  document.getElementById('faucetBtn').addEventListener('click', function() {
    if (!currentUser) {
      alert("Lütfen önce Google ile giriş yap kanka!");
      return;
    }
    const userRef = doc(db, "users", currentUser.uid);
    runTransaction(db, async (transaction) => {
      const userDoc = await transaction.get(userRef);
      let currentBalance = userDoc.exists() ? (userDoc.data().balance || 0) : 0;
      transaction.update(userRef, { balance: currentBalance + 5.00 });
    }).then(() => {
      document.getElementById('faucetMsg').innerText = "⚡ Core Refilled! +5.00 Points saved.";
      this.disabled = true;
    }).catch(err => console.error("Transaction Hatası:", err));
  });
</script>
