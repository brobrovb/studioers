---
layout: default
title: Free Crypto Arcade
---

<!-- Minima Temasının Dış Çerçevesini ve Arka Planını Tamamen Siyah Yapan Ayar -->
<style>
  /* Sayfanın en dış katmanından en iç katmanına kadar her yeri koyu yapıyoruz */
  html, body, .site-header, .site-footer, .page-content, .wrapper {
    background-color: #0a0a0a !important;
    background: #0a0a0a !important;
    color: #e0e0e0 !important;
  }

  /* Screenshot_42.png'deki o sırıtan beyaz-mavi kenarlıkları ve gölgeleri uçuruyoruz */
  .wrapper {
    max-width: 1000px !important;
    box-shadow: none !important;
    border: none !important;
  }

  /* Üst menüdeki başlık ve linklerin renklerini neon yeşili yapıyoruz */
  .site-title, .site-title:visited, .site-nav .page-link {
    color: #00ff88 !important;
    font-family: 'Courier New', Courier, monospace;
  }

  /* Alt taraftaki Minima çizgisini gizliyoruz */
  .site-header {
    border-bottom: 1px solid #1a1a1a !important;
  }
  .site-footer {
    border-top: 1px solid #1a1a1a !important;
  }

  /* İçerik Alanı Tasarımı */
  .arcade-body { 
    color: #ffffff; 
    font-family: 'Courier New', Courier, monospace;
    padding: 10px 0;
  }

  .arcade-title {
    color: #00ff88;
    text-shadow: 0 0 10px rgba(0,255,136,0.5);
    text-transform: uppercase;
    letter-spacing: 2px;
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
    background: #121212; 
    border: 1px solid #222; 
    border-top: 3px solid #00ff88; 
    padding: 20px; 
    border-radius: 6px; 
    flex: 1; 
    min-width: 220px; 
    box-shadow: 0 4px 10px rgba(0,0,0,0.5);
    text-align: center;
  }

  .stat-card h5 { 
    margin: 0; 
    color: #888; 
    text-transform: uppercase; 
    font-size: 11px; 
    letter-spacing: 1px; 
  }

  .stat-card p { 
    margin: 8px 0 0 0; 
    font-size: 24px; 
    font-weight: bold; 
    color: #00ff88; 
  }

  .stat-card.balance-card { 
    border-top-color: #00e5ff; 
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
    border: 1px solid #222; 
    padding: 20px; 
    border-radius: 8px; 
    width: 48%; 
    min-width: 280px;
    background: #121212; 
    box-sizing: border-box;
    transition: all 0.3s ease; 
  }

  .game-card:hover { 
    transform: translateY(-3px); 
    border-color: #00ff88; 
    box-shadow: 0 0 15px rgba(0,255,136,0.2);
  }

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
  }

  .game-btn:hover {
    background: #00ff88;
    color: #000;
  }

  .faucet-section { 
    margin: 40px 0; 
    padding: 30px; 
    background: #121212; 
    border: 2px dashed #00e5ff; 
    border-radius: 6px; 
    text-align: center; 
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
  }

  .faucet-btn:disabled {
    border-color: #444 !important;
    color: #888 !important;
    cursor: not-allowed;
  }
</style>

<div class="arcade-body">

<h1 class="arcade-title">SYSTEM STATUS: STUDIOERS ARCADE</h1>

<p style="color: #aaa; font-size: 14px; margin-bottom: 30px;">
  [ONLINE] Play web games. Build Virtual Power. Earn Crypto.
</p>

---

<!-- CANLI GÖSTERGELER -->
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

### 🕹️ Select Simulation

<div class="game-grid">
  <div class="game-card">
    <h3 class="game-title">🧩 Protetris_v1.0</h3>
    <p style="font-size:13px; color:#aaa; line-height: 1.6; min-height: 50px;">Legendary block puzzle. Stack bricks perfectly, clear lines, and boost your virtual mining power!</p>
    <p style="font-size:12px; margin-bottom: 0; color: #00ff88;">> Reward: Up to +50 Th/s Power</p>
    <a href="#" class="game-btn">INITIALIZE</a>
  </div>

  <div class="game-card">
    <h3 class="game-title" style="color:#00e5ff;">⚡ Crypto_Matcher</h3>
    <p style="font-size:13px; color:#aaa; line-height: 1.6; min-height: 50px;">Match classic crypto coins under a ticking clock. Fast-paced action for quick power boosts.</p>
    <p style="font-size:12px; margin-bottom: 0; color: #00e5ff;">> Reward: Up to +30 Th/s Power</p>
    <a href="/crypto-matcher" class="game-btn" style="border-color:#00e5ff; color:#00e5ff;">INITIALIZE</a>
  </div>
</div>

---

<!-- MUSLUK ALANI -->
<div class="faucet-section">
  <h3 style="margin-top:0; color:#00e5ff;">🎁 Hourly Data Faucet</h3>
  <p style="color:#aaa; font-size:14px; margin-bottom:20px;">Claim free bonus balance points every 60 minutes instantly.</p>
  <button id="faucetBtn" class="faucet-btn">CLAIM_POINTS</button>
  <p id="faucetMsg" style="margin-top: 15px; font-weight: bold; color: #00ff88; min-height: 20px;"></p>
</div>

</div>

<script>
  let power = localStorage.getItem('power') || "0.000";
  let balance = localStorage.getItem('balance') || "0.00";
  
  document.getElementById('userPower').innerText = parseFloat(power).toFixed(3) + " Th/s";
  document.getElementById('userBalance').innerText = parseFloat(balance).toFixed(2) + " Points";

  document.getElementById('faucetBtn').addEventListener('click', function() {
    balance = parseFloat(balance) + 5.00;
    localStorage.setItem('balance', balance);
    
    document.getElementById('userBalance').innerText = balance.toFixed(2) + " Points";
    document.getElementById('faucetMsg').innerText = "> Success! +5.00 Points added to your balance.";
    
    this.disabled = true;
  });
</script>
