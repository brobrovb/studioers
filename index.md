---
layout: default
title: Free Crypto Arcade
---

<!-- Siber Atari Salonu Stili için Özel CSS -->
<style>
  .arcade-body { 
    background-color: #121212; 
    color: #ffffff; 
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
    padding: 20px;
    border-radius: 8px;
    margin-top: 20px;
  }
  .stats-container { 
    display: flex; 
    gap: 15px; 
    justify-content: space-between; 
    margin-bottom: 30px; 
    flex-wrap: wrap; 
  }
  .stat-card { 
    background: #1e1e1e; 
    border: 1px solid #333; 
    border-left: 4px solid #00ff88; 
    padding: 15px; 
    border-radius: 6px; 
    flex: 1; 
    min-width: 200px; 
    box-shadow: 0 4px 6px rgba(0,0,0,0.3); 
  }
  .stat-card h5 { 
    margin: 0; 
    color: #888; 
    text-transform: uppercase; 
    font-size: 11px; 
    letter-spacing: 1px; 
  }
  .stat-card p { 
    margin: 5px 0 0 0; 
    font-size: 22px; 
    font-weight: bold; 
    color: #00ff88; 
  }
  .stat-card.balance-card { 
    border-left-color: #00e5ff; 
  }
  .stat-card.balance-card p { 
    color: #00e5ff; 
  }
  .game-grid { 
    display: flex; 
    gap: 20px; 
    flex-wrap: wrap; 
    margin-top: 20px; 
  }
  .game-card { 
    border: 1px solid #333; 
    padding: 20px; 
    border-radius: 8px; 
    width: 290px; 
    background: #1a1a1a; 
    box-sizing: border-box;
    transition: transform 0.2s, border-color 0.2s; 
  }
  .game-card:hover { 
    transform: translateY(-5px); 
    border-color: #00ff88; 
  }
  .game-btn { 
    background: #00ff88; 
    color: #121212; 
    padding: 10px 15px; 
    border-radius: 4px; 
    text-decoration: none; 
    display: inline-block; 
    font-weight: bold; 
    margin-top: 15px; 
    text-align: center; 
    width: 100%; 
    box-sizing: border-box; 
  }
  .faucet-section { 
    margin: 40px 0; 
    padding: 25px; 
    background: #1a1a1a; 
    border: 1px dashed #00e5ff; 
    border-radius: 8px; 
    text-align: center; 
  }
  .faucet-btn { 
    background: #00e5ff; 
    color: #121212; 
    border: none; 
    padding: 12px 30px; 
    font-size: 16px; 
    border-radius: 4px; 
    cursor: pointer; 
    font-weight: bold; 
    text-transform: uppercase; 
  }
  .faucet-btn:disabled {
    background: #444 !important;
    color: #888 !important;
    cursor: not-allowed;
  }
</style>

<div class="arcade-body">

# 🎮 Studioers Play2Earn Arcade

Welcome to the ultimate crypto station! Play our lightweight web games to build your **Virtual Mining Power**, or claim free points every hour. Turn your gaming time into real crypto rewards!

---

<!-- CANLI OYUNCU GÖSTERGELERI (ROLLERCOIN ESINTISI) -->
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
    <h5>Your Balance</h5>
    <p id="userBalance">0.00 Points</p>
  </div>
</div>

---

### 🕹️ Available Arcade Games

<div class="game-grid">
  <!-- OYUN 1: PROTETRIS -->
  <div class="game-card">
    <h3 style="margin-top:0; color:#00ff88;">🧩 Protetris Web</h3>
    <p style="font-size:14px; color:#aaa; line-height: 1.5;">The legendary block puzzle game. Clear lines, stack bricks perfectly, and boost your virtual mining power!</p>
    <p style="font-size:13px; margin-bottom: 0;"><strong>Reward:</strong> Up to +50 Th/s Power</p>
    <a href="#" class="game-btn">PLAY & EARN</a>
  </div>

  <!-- OYUN 2: CRYPTO MATCHER -->
  <div class="game-card">
    <h3 style="margin-top:0; color:#00ff88;">⚡ Crypto Matcher</h3>
    <p style="font-size:14px; color:#aaa; line-height: 1.5;">Match classic crypto coins under a ticking clock. Fast-paced casual action for quick power boosts.</p>
    <p style="font-size:13px; margin-bottom: 0;"><strong>Reward:</strong> Up to +30 Th/s Power</p>
    <a href="#" class="game-btn" style="background:#00e5ff;">PLAY & EARN</a>
  </div>
</div>

---

<!-- SAATLIK BEDAVA MUSLUK (FAUCET) ALANI -->
<div class="faucet-section">
  <h3 style="margin-top:0; color:#00e5ff;">🎁 Hourly Crypto Faucet</h3>
  <p style="color:#aaa; font-size:14px; margin-bottom:15px;">Don't want to play right now? Get an instant free balance boost every 60 minutes!</p>
  <button id="faucetBtn" class="faucet-btn">Claim Free Points</button>
  <p id="faucetMsg" style="margin-top: 12px; font-weight: bold; color: #00ff88; min-height: 20px;"></p>
</div>

</div>

<!-- TARAYICI HAFIZASINDA GEÇICI DURUM YÖNETIMI -->
<script>
  // Tarayıcı hafızasından (localStorage) mevcut verileri çekiyoruz
  let power = localStorage.getItem('power') || "0.000";
  let balance = localStorage.getItem('balance') || "0.00";
  
  // Arayüzdeki göstergeleri güncelliyoruz
  document.getElementById('userPower').innerText = parseFloat(power).toFixed(3) + " Th/s";
  document.getElementById('userBalance').innerText = parseFloat(balance).toFixed(2) + " Points";

  // Musluk butonuna tıklama aksiyonu
  document.getElementById('faucetBtn').addEventListener('click', function() {
    balance = parseFloat(balance) + 5.00;
    localStorage.setItem('balance', balance);
    
    document.getElementById('userBalance').innerText = balance.toFixed(2) + " Points";
    document.getElementById('faucetMsg').innerText = "🎉 Success! +5.00 Points added to your balance.";
    
    // Butonu geçici olarak devre dışı bırakıyoruz (Sayfa yenilenene kadar)
    this.disabled = true;
  });
</script>
