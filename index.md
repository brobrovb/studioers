---
layout: default
title: Free Crypto Arcade
---

<!-- Siber Atari Salonu Stili için GÜNCELLENMİŞ CSS -->
<style>
  /* Tüm sayfa gövdesini ve Minima temasının gri alanlarını siyah yapıyoruz */
  body, .arcade-body, .site-content, .wrapper { 
    background-color: #000000 !important; 
    color: #e0e0e0; 
    font-family: 'Courier New', Courier, monospace; /* Daha hacker/retro bir font */
  }

  .arcade-body { 
    padding: 20px;
    border-radius: 8px;
    margin-top: 10px;
  }

  /* Ana başlık neon parlamalı */
  .arcade-title {
    color: #00ff88;
    text-shadow: 0 0 10px rgba(0,255,136,0.7);
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 2px;
  }

  .stats-container { 
    display: flex; 
    gap: 15px; 
    justify-content: space-between; 
    margin-bottom: 30px; 
    flex-wrap: wrap; 
  }

  .stat-card { 
    background: #0a0a0a; /* Çok koyu gri, siyaha yakın */
    border: 1px solid #1a1a1a; 
    border-top: 3px solid #00ff88; /* Çizgiyi üste aldık, daha modern */
    padding: 15px; 
    border-radius: 4px; 
    flex: 1; 
    min-width: 200px; 
    box-shadow: 0 0 15px rgba(0,255,136,0.1); /* Hafif yeşil ışıma */
    text-align: center;
  }

  .stat-card h5 { 
    margin: 0; 
    color: #888; 
    text-transform: uppercase; 
    font-size: 12px; 
    letter-spacing: 1px; 
  }

  .stat-card p { 
    margin: 8px 0 0 0; 
    font-size: 24px; 
    font-weight: bold; 
    color: #00ff88; 
  }

  /* Bakiye kartı mavi temalı */
  .stat-card.balance-card { 
    border-top-color: #00e5ff; 
    box-shadow: 0 0 15px rgba(0,229,255,0.1);
  }
  .stat-card.balance-card p { 
    color: #00e5ff; 
  }

  .game-grid { 
    display: flex; 
    gap: 20px; 
    flex-wrap: wrap; 
    margin-top: 20px; 
    justify-content: center;
  }

  .game-card { 
    border: 1px solid #1a1a1a; 
    padding: 20px; 
    border-radius: 8px; 
    width: 300px; 
    background: #0a0a0a; 
    box-sizing: border-box;
    transition: all 0.3s ease; 
    position: relative;
    overflow: hidden;
  }

  /* Kartın üzerine gelince parlaması */
  .game-card:hover { 
    transform: translateY(-5px); 
    border-color: #00ff88; 
    box-shadow: 0 0 20px rgba(0,255,136,0.3);
  }

  /* Oyun Kartı Başlıkları */
  .game-title {
    margin-top:0; 
    color:#00ff88; 
    text-transform: uppercase;
    font-size: 18px;
  }

  .game-btn { 
    background: transparent;
    color: #00ff88; 
    border: 2px solid #00ff88;
    padding: 10px 15px; 
    border-radius: 4px; 
    text-decoration: none; 
    display: inline-block; 
    font-weight: bold; 
    margin-top: 15px; 
    text-align: center; 
    width: 100%; 
    box-sizing: border-box; 
    transition: all 0.2s;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .game-btn:hover {
    background: #00ff88;
    color: #000;
    box-shadow: 0 0 10px #00ff88;
  }

  .faucet-section { 
    margin: 50px 0; 
    padding: 30px; 
    background: #080808; 
    border: 2px dashed #00e5ff; 
    border-radius: 4px; 
    text-align: center; 
    box-shadow: 0 0 20px rgba(0,229,255,0.1);
  }

  .faucet-btn { 
    background: transparent; 
    color: #00e5ff; 
    border: 2px solid #00e5ff;
    padding: 12px 40px; 
    font-size: 16px; 
    border-radius: 4px; 
    cursor: pointer; 
    font-weight: bold; 
    text-transform: uppercase; 
    transition: all 0.2s;
  }

  .faucet-btn:hover {
    background: #00e5ff;
    color: #000;
    box-shadow: 0 0 15px #00e5ff;
  }

  .faucet-btn:disabled {
    border-color: #444 !important;
    color: #888 !important;
    background: transparent !important;
    cursor: not-allowed;
    box-shadow: none !important;
  }

  /* Minima temasının alt ve üst bilgi alanlarını da siyah yapalım */
  .site-header, .site-footer {
    background-color: #000000 !important;
    border-bottom: 1px solid #1a1a1a !important;
    border-top: 1px solid #1a1a1a !important;
  }
  .site-title, .site-nav .page-link {
    color: #00ff88 !important;
  }
</style>

<div class="arcade-body">

<h1 class="arcade-title">SYSTEM STATUS: STUDIOERS ARCADE</h1>

<p style="text-align: center; color: #aaa; font-size: 14px; margin-bottom: 30px;">
  [ONLINE] Play retro games. Build Virtual Power. Earn Crypto.
</p>

---

<!-- CANLI OYUNCU GÖSTERGELERI -->
<div class="stats-container">
  <div class="stat-card">
    <h5><i class="fas fa-microchip"></i> Your Mining Power</h5>
    <p id="userPower">0.000 Th/s</p>
  </div>
  <div class="stat-card">
    <h5><i class="fas fa-server"></i> Network Power</h5>
    <p>1,420.85 Ph/s</p>
  </div>
  <div class="stat-card balance-card">
    <h5><i class="fas fa-wallet"></i> Your Balance</h5>
    <p id="userBalance">0.00 Points</p>
  </div>
</div>

---

### 🕹️ Select Your Simulation

<div class="game-grid">
  <!-- OYUN 1: PROTETRIS -->
  <div class="game-card">
    <h3 class="game-title">🧩 Protetris_v1.0</h3>
    <p style="font-size:13px; color:#aaa; line-height: 1.6; min-height: 60px;">Legendary block puzzle. Stack bricks perfectly, clear lines, and boost your virtual GH/s mining power!</p>
    <p style="font-size:12px; margin-bottom: 0; color: #00ff88;">> Reward: Up to +50 Th/s Power</p>
    <a href="#" class="game-btn">INITIALIZE</a>
  </div>

  <!-- OYUN 2: CRYPTO MATCHER -->
  <div class="game-card">
    <h3 class="game-title" style="color:#00e5ff;">⚡ Crypto_Matcher</h3>
    <p style="font-size:13px; color:#aaa; line-height: 1.6; min-height: 60px;">Match classic crypto coins under a ticking clock. Fast-paced causal action for quick power boosts.</p>
    <p style="font-size:12px; margin-bottom: 0; color: #00e5ff;">> Reward: Up to +30 Th/s Power</p>
    <a href="#" class="game-btn" style="border-color:#00e5ff; color:#00e5ff;">INITIALIZE</a>
  </div>
</div>

---

<!-- SAATLIK BEDAVA MUSLUK (FAUCET) ALANI -->
<div class="faucet-section">
  <h3 style="margin-top:0; color:#00e5ff;">🎁 Daily Potion: Hourly Faucet</h3>
  <p style="color:#aaa; font-size:14px; margin-bottom:20px;">Need a quick data boost? Claim your free balance points every 60 minutes.</p>
  <button id="faucetBtn" class="faucet-btn">CLAIM_POINTS</button>
  <p id="faucetMsg" style="margin-top: 15px; font-weight: bold; color: #00ff88; min-height: 20px;"></p>
</div>

</div>

<!-- FontAwesome İkonları için Script (İkonlar görünmezse config'e de ekleyebiliriz) -->
<script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>

<!-- TARAYICI HAFIZASINDA GEÇICI DURUM YÖNETIMI -->
<script>
  let power = localStorage.getItem('power') || "0.000";
  let balance = localStorage.getItem('balance') || "0.00";
  
  document.getElementById('userPower').innerText = parseFloat(power).toFixed(3) + " Th/s";
  document.getElementById('userBalance').innerText = parseFloat(balance).toFixed(2) + " Points";

  document.getElementById('faucetBtn').addEventListener('click', function() {
    balance = parseFloat(balance) + 5.00;
    localStorage.setItem('balance', balance);
    
    document.getElementById('userBalance').innerText = balance.toFixed(2) + " Points";
    document.getElementById('faucetMsg').innerText = "> Success! +5.00 Points added to your net balance.";
    
    this.disabled = true;
  });
</script>
