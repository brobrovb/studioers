---
layout: default
title: Free Crypto Arcade
---

<!-- RollerCoin Birebir Kart Tasarımı CSS Ayarları -->
<style>
  html, body, .site-header, .site-footer, .page-content, .wrapper {
    background-color: #1a1b23 !important; /* RollerCoin'in o tatlı lacivert-gri koyu arka planı */
    color: #e2e8f0 !important;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  .wrapper {
    max-width: 1100px !important;
    box-shadow: none !important;
    border: none !important;
  }

  /* Üst Menü Düzenlemeleri */
  .site-title, .site-title:visited, .site-nav .page-link {
    color: #00f0ff !important;
    font-weight: bold;
    text-transform: uppercase;
  }

  .arcade-body { 
    padding: 10px 0;
  }

  /* Üst Panel Panosu (Dashboard) */
  .stats-container { 
    display: flex; 
    gap: 15px; 
    justify-content: space-between; 
    margin-bottom: 35px; 
    flex-wrap: wrap; 
  }

  .stat-card { 
    background: #242632; 
    border: 1px solid #2f3245; 
    border-left: 5px solid #00f0ff; 
    padding: 15px 20px; 
    border-radius: 6px; 
    flex: 1; 
    min-width: 220px; 
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  }

  .stat-card h5 { 
    margin: 0; 
    color: #94a3b8; 
    text-transform: uppercase; 
    font-size: 11px; 
    letter-spacing: 1px; 
  }

  .stat-card p { 
    margin: 6px 0 0 0; 
    font-size: 24px; 
    font-weight: bold; 
    color: #00f0ff; 
  }

  .stat-card.balance-card { 
    border-left-color: #ff007a; 
  }
  .stat-card.balance-card p { 
    color: #ff007a; 
  }

  /* OYUN KARTLARI ALANI (GRID) */
  .game-grid { 
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 20px; 
    margin-top: 20px;
  }

  /* Screenshot_43.png'deki Birebir Kart Yapısı */
  .rc-game-card {
    background: #242632;
    border: 1px solid #2f3245;
    border-radius: 8px;
    padding: 15px;
    display: flex;
    gap: 15px;
    align-items: center;
    box-shadow: 0 4px 10px rgba(0,0,0,0.15);
  }

  /* Sol taraftaki Oyun Görsel Kutusu */
  .rc-game-image {
    width: 90px;
    height: 90px;
    background: #13141c;
    border-radius: 6px;
    border: 1px solid #2f3245;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 36px;
    box-shadow: inset 0 0 10px rgba(0,0,0,0.5);
  }

  /* Sağ taraftaki Detay Alanı */
  .rc-game-details {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .rc-game-name {
    margin: 0;
    font-size: 16px;
    font-weight: bold;
    color: #ffffff;
  }

  /* Zorluk Derecesi (Difficulty Bar) */
  .rc-difficulty-label {
    font-size: 11px;
    color: #00f0ff;
    margin: 0;
  }
  
  .rc-difficulty-bar {
    display: flex;
    gap: 3px;
    margin-bottom: 5px;
  }

  .rc-dot {
    width: 12px;
    height: 4px;
    background: #3a3f58;
    border-radius: 1px;
  }

  .rc-dot.active {
    background: #ff007a; /* Ekrandaki pembe zorluk çizgisi */
    box-shadow: 0 0 5px #ff007a;
  }

  /* Screenshot_43.png'deki O ŞIK 3D START BUTONU */
  .rc-start-btn {
    background: #00e5ff;
    color: #000000;
    border: none;
    padding: 8px 0;
    border-radius: 6px;
    font-weight: bold;
    font-size: 13px;
    text-align: center;
    text-decoration: none;
    text-transform: uppercase;
    letter-spacing: 1px;
    box-shadow: 0 4px 0 #00a8bc; /* Alt taraftaki koyu gölge 3D hissi veriyor */
    transition: all 0.1s ease;
    display: block;
    width: 100%;
  }

  .rc-start-btn:active {
    transform: translateY(3px);
    box-shadow: 0 1px 0 #00a8bc; /* Basılınca çökme efekti */
  }

  /* Saatlik Musluk Alanı */
  .faucet-section { 
    margin: 40px 0; 
    padding: 25px; 
    background: #242632; 
    border: 2px dashed #ff007a; 
    border-radius: 8px; 
    text-align: center; 
  }

  .faucet-btn { 
    background: #ff007a; 
    color: #ffffff; 
    border: none; 
    padding: 12px 40px; 
    font-size: 15px; 
    border-radius: 6px; 
    cursor: pointer; 
    font-weight: bold; 
    text-transform: uppercase; 
    box-shadow: 0 4px 0 #b00052;
  }

  .faucet-btn:active {
    transform: translateY(3px);
    box-shadow: 0 1px 0 #b00052;
  }

  .faucet-btn:disabled {
    background: #4e5268 !important;
    box-shadow: none !important;
    cursor: not-allowed;
    color: #aaa;
  }
</style>

<div class="arcade-body">

<h1 style="text-align:center; font-size: 26px; color: #ffffff; letter-spacing: 1px; margin-bottom: 25px;">🎮 STUDIOERS VIRTUAL STATION</h1>

<!-- GÖSTERGELER (DASHBOARD) -->
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

<h3 style="border-bottom: 1px solid #2f3245; padding-bottom: 10px; color: #94a3b8;">🕹️ Arcade Simulations</h3>

<!-- ROLLERCOIN BİREBİR KART IZGARASI -->
<div class="game-grid">

  <!-- KART 1: CRYPTO MATCHER (Geliştirdiğimiz Oyun) -->
  <div class="rc-game-card">
    <div class="rc-game-image">⚡</div>
    <div class="rc-game-details">
      <h4 class="rc-game-name">Crypto-match</h4>
      <p class="rc-difficulty-label">difficulty: 3</p>
      <div class="rc-difficulty-bar">
        <div class="rc-dot active"></div>
        <div class="rc-dot active"></div>
        <div class="rc-dot active"></div>
        <div class="rc-dot"></div>
        <div class="rc-dot"></div>
        <div class="rc-dot"></div>
        <div class="rc-dot"></div>
      </div>
      <a href="/crypto-matcher" class="rc-start-btn">🏁 START</a>
    </div>
  </div>

  <!-- KART 2: PROTETRIS WEB (Sıradaki Oyun) -->
  <div class="rc-game-card">
    <div class="rc-game-image">🧩</div>
    <div class="rc-game-details">
      <h4 class="rc-game-name">Protetris</h4>
      <p class="rc-difficulty-label">difficulty: 5</p>
      <div class="rc-difficulty-bar">
        <div class="rc-dot active"></div>
        <div class="rc-dot active"></div>
        <div class="rc-dot active"></div>
        <div class="rc-dot active"></div>
        <div class="rc-dot active"></div>
        <div class="rc-dot"></div>
        <div class="rc-dot"></div>
      </div>
      <a href="#" class="rc-start-btn" style="background:#ff007a; box-shadow: 0 4px 0 #b00052; color:white;">🏁 START</a>
    </div>
  </div>

</div>

<!-- SAATLİK MUSLUK ALANI -->
<div class="faucet-section">
  <h3 style="margin-top:0; color:#ff007a;">🎁 Hourly Energy Refill</h3>
  <p style="color:#94a3b8; font-size:14px; margin-bottom:15px;">Claim an instant +5.00 points bonus directly to your wallet allocation.</p>
  <button id="faucetBtn" class="faucet-btn">CLAIM BONUS</button>
  <p id="faucetMsg" style="margin-top: 12px; font-weight: bold; color: #00ff88; min-height: 20px;"></p>
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
    document.getElementById('faucetMsg').innerText = "⚡ Core Refilled! +5.00 Points saved.";
    this.disabled = true;
  });
</script>
