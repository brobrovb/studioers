---
layout: null
---
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>RollerCoin Madencilik Rehberi & Bonus Kayıt</title>
  <style>
    /* Reset & Dark Cyberpunk UI */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    
    body {
      background-color: #12131a !important;
      color: #e2e8f0 !important;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      line-height: 1.6;
      width: 100%;
      min-height: 100vh;
      overflow-x: hidden;
    }

    .container {
      max-width: 960px;
      margin: 0 auto;
      padding: 20px 16px;
    }

    /* Top Logo Header */
    .brand-header {
      text-align: center;
      padding: 15px 0 25px 0;
    }

    .brand-logo {
      max-width: 260px;
      height: auto;
      filter: drop-shadow(0 0 10px rgba(0, 240, 255, 0.4));
    }

    /* Ad Container Placeholder */
    .ad-container {
      margin: 15px 0;
      text-align: center;
      width: 100%;
      min-height: 90px;
      background: #181a24;
      border: 1px dashed #2e3248;
      border-radius: 8px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }

    .ad-label {
      font-size: 10px;
      color: #64748b;
      letter-spacing: 1px;
      margin-bottom: 4px;
      text-transform: uppercase;
    }

    /* Hero Section */
    .hero-card {
      background: linear-gradient(180deg, #1d1f2c 0%, #171822 100%);
      border: 2px solid #00f0ff;
      border-radius: 16px;
      padding: 35px 20px;
      text-align: center;
      box-shadow: 0 0 25px rgba(0, 240, 255, 0.2);
      margin-bottom: 35px;
    }

    .bonus-badge {
      display: inline-block;
      background: #ff007a;
      color: #ffffff;
      font-size: 12px;
      font-weight: 800;
      padding: 6px 16px;
      border-radius: 20px;
      margin-bottom: 15px;
      text-transform: uppercase;
      letter-spacing: 1px;
      box-shadow: 0 0 10px rgba(255, 0, 122, 0.5);
    }

    .hero-title {
      color: #ffffff;
      font-size: 32px;
      margin-bottom: 12px;
      font-weight: 900;
      line-height: 1.2;
    }

    .hero-subtitle {
      color: #94a3b8;
      font-size: 15px;
      max-width: 720px;
      margin: 0 auto 25px auto;
    }

    /* CTA Button */
    .cta-btn {
      display: inline-block;
      background: linear-gradient(135deg, #00f0ff 0%, #00a8bc 100%);
      color: #000000 !important;
      font-weight: 900;
      font-size: 18px;
      padding: 16px 36px;
      border-radius: 10px;
      text-decoration: none !important;
      text-transform: uppercase;
      box-shadow: 0 6px 0 #007785, 0 0 20px rgba(0, 240, 255, 0.4);
      transition: all 0.15s ease-in-out;
      cursor: pointer;
    }

    .cta-btn:hover {
      transform: translateY(-3px);
      box-shadow: 0 9px 0 #007785, 0 0 30px rgba(0, 240, 255, 0.7);
    }

    .cta-btn:active {
      transform: translateY(3px);
      box-shadow: 0 2px 0 #007785;
    }

    /* Stats Counter Bar */
    .stats-bar {
      display: flex;
      justify-content: space-around;
      background: #1a1c28;
      border: 1px solid #2e3248;
      border-radius: 12px;
      padding: 15px 10px;
      margin-bottom: 35px;
      text-align: center;
      flex-wrap: wrap;
      gap: 15px;
    }

    .stat-item h4 {
      color: #00f0ff;
      font-size: 20px;
      font-weight: 800;
    }

    .stat-item p {
      color: #64748b;
      font-size: 12px;
      text-transform: uppercase;
      margin-top: 2px;
    }

    /* Screenshot Showcases */
    .screenshot-section {
      background: #181a24;
      border: 1px solid #2e3248;
      border-radius: 12px;
      padding: 25px;
      margin-bottom: 35px;
    }

    .section-header {
      text-align: center;
      margin-bottom: 25px;
    }

    .section-header h2 {
      color: #ffffff;
      font-size: 24px;
    }

    .section-header p {
      color: #94a3b8;
      font-size: 14px;
    }

    .preview-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 20px;
    }

    .preview-card {
      background: #202332;
      border: 1px solid #2e3248;
      border-radius: 10px;
      overflow: hidden;
      transition: transform 0.2s;
    }

    .preview-card:hover {
      transform: translateY(-4px);
      border-color: #00f0ff;
    }

    .preview-card img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      background-color: #12131a;
      display: block;
      border-bottom: 1px solid #2e3248;
    }

    .preview-card-body {
      padding: 15px;
    }

    .preview-card-body h3 {
      color: #00f0ff;
      font-size: 16px;
      margin-bottom: 6px;
    }

    .preview-card-body p {
      color: #cbd5e1;
      font-size: 13px;
    }

    /* Detailed Guide Section */
    .guide-section {
      background: #181a24;
      border: 1px solid #2e3248;
      border-radius: 12px;
      padding: 30px;
      margin-bottom: 35px;
    }

    .guide-section h2 {
      color: #ff007a;
      border-bottom: 1px solid #2e3248;
      padding-bottom: 12px;
      margin-bottom: 20px;
      font-size: 22px;
    }

    .guide-section h3 {
      color: #00f0ff;
      margin: 22px 0 8px 0;
      font-size: 17px;
    }

    .guide-section p, .guide-section li {
      color: #cbd5e1;
      font-size: 14px;
    }

    .guide-section ul, .guide-section ol {
      padding-left: 20px;
      margin-bottom: 15px;
    }

    .guide-section li {
      margin-bottom: 8px;
    }

    /* Live Proof Badge */
    .proof-box {
      background: #202332;
      border-left: 4px solid #00ff88;
      padding: 15px;
      border-radius: 6px;
      margin-top: 20px;
    }

    .proof-box strong {
      color: #00ff88;
    }

    @media (max-width: 600px) {
      .hero-title { font-size: 24px; }
      .hero-subtitle { font-size: 14px; }
      .cta-btn { font-size: 15px; padding: 14px 20px; width: 100%; text-align: center; }
      .guide-section, .screenshot-section { padding: 20px 15px; }
    }
  </style>
</head>
<body>

  <div class="container">

    <!-- ROLLERCOIN LOGO HEADER -->
    <div class="brand-header">
      <a href="https://rollercoin.com/?r=mqg589d7" target="_blank" rel="noopener noreferrer">
        <img src="https://rollercoin.com/static/images/logo.svg" alt="RollerCoin Logo" class="brand-logo">
      </a>
    </div>

    <!-- TOP AD SPONSOR -->
    <div class="ad-container">
      <span class="ad-label">Sponsored Mining Network</span>
    </div>

    <!-- HERO SECTION -->
    <div class="hero-card">
      <span class="bonus-badge">🎁 ANINDA 1000 SATOSHI BONUSU</span>
      <h1 class="hero-title">ROLLERCOIN GERÇEK MİNİNG SİMÜLATÖRÜ</h1>
      <p class="hero-subtitle">
        Mini oyunlar oynayarak sanal madencilik gücü (Hashrate) topla, kendi madencilik çiftliğini kur ve Bitcoin, Ethereum, Dogecoin kazancı elde et.
      </p>
      <a href="https://rollercoin.com/?r=mqg589d7" target="_blank" rel="noopener noreferrer" class="cta-btn">
        🚀 BONUSLA BİRLİKTE ÜCRETSİZ BAŞLA
      </a>
    </div>

    <!-- STATS COUNTER -->
    <div class="stats-bar">
      <div class="stat-item">
        <h4>4M+</h4>
        <p>Aktif Oyuncu</p>
      </div>
      <div class="stat-item">
        <h4>$5.2M+</h4>
        <p>Ödenen Ödül</p>
      </div>
      <div class="stat-item">
        <h4>7+ Yıl</h4>
        <p>Kesintisiz Hizmet</p>
      </div>
      <div class="stat-item">
        <h4>0 TRTL</h4>
        <p>Yatırım Şartı</p>
      </div>
    </div>

    <!-- ROLLERCOIN SHOWCASE SCREENSHOTS -->
    <div class="screenshot-section">
      <div class="section-header">
        <h2>🎮 RollerCoin Oyun ve Panel Arayüzü</h2>
        <p>Gerçek oyun içi madencilik odası ve sistem görselleri</p>
      </div>

      <div class="preview-grid">
        <!-- Card 1: Mining Room -->
        <div class="preview-card">
          <img src="https://rollercoin.com/static/images/blog/posts/mining_room_customization/room_customization_cover.png" alt="RollerCoin Mining Room">
          <div class="preview-card-body">
            <h3>⛏️ Sanal Madencilik Odan</h3>
            <p>Kazandığın veya satın aldığın rafları ve miner cihazlarını odana dizerek 7/24 pasif kripto kazımı yap.</p>
          </div>
        </div>

        <!-- Card 2: Games -->
        <div class="preview-card">
          <img src="https://rollercoin.com/static/images/blog/posts/games_update/games_update_cover.jpg" alt="RollerCoin Mini Games">
          <div class="preview-card-body">
            <h3>🕹️ Mini Oyunlar ile Güç Kazan</h3>
            <p>Coin-Flip, Flappy Rocket, Token Surfer ve Dr. Hamster gibi oyunları geçerek kazım gücünü (TH/s) katla.</p>
          </div>
        </div>

        <!-- Card 3: Marketplace & Rewards -->
        <div class="preview-card">
          <img src="https://rollercoin.com/static/images/blog/posts/marketplace_launch/marketplace_cover.jpg" alt="RollerCoin Marketplace">
          <div class="preview-card-body">
            <h3>🏪 Pazar Yeri & Kripto Çekimi</h3>
            <p>Cihazlarını diğer oyunculara sat ya da biriken BTC, ETH, DOGE ve SOL bakiyelerini doğrudan cüzdanına çek.</p>
          </div>
        </div>
      </div>
    </div>

    <!-- DETAILED GUIDE -->
    <div class="guide-section">
      <h2>📖 RollerCoin Strateji ve Başlangıç Rehberi</h2>
      <p>
        RollerCoin, yatırımsız olarak gerçek kripto biriktirmenize olanak tanır. Oyundaki temel amaç, mini oyunlarla veya cihaz yatırımlarıyla sanal kazım gücünüzü (GH/s - TH/s - EH/s) artırmaktır.
      </p>

      <h3>1. Oyun Oynayarak Kazım Gücü Elde Etme</h3>
      <p>
        Sisteme kaydolduktan sonra "Games" sekmesinden istediğiniz mini oyunu oynayın. Kazandığınız her oyun size sanal güç sağlar. Ne kadar çok oyun kazanırsanız sanal bilgisayarınızın seviyesi (PC Level) o kadar yükselir ve gücünüzün kalıcılık süresi 7 güne kadar çıkar.
      </p>

      <h3>2. Pasif Gelir Odası Oluşturma</h3>
      <p>
        Sürekli oyun oynamak istemiyorsanız, sezonsal etkinliklerden (Event Pass), kutulardan veya pazar yerinden (Marketplace) madenci cihazları (Miners) toplayabilirsiniz. Bu cihazlar odanızda durduğu sürece siz çevrimdışı olsanız dahi size kripto para kazandırmaya devam eder.
      </p>

      <h3>3. Hangi Coin'i Kazmalısınız?</h3>
      <ul>
        <li><strong>Bitcoin (BTC):</strong> En yüksek güvenilirlik ve uzun vadeli birikim için.</li>
        <li><strong>Dogecoin (DOGE) / Litecoin (LTC):</strong> Düşük çekim limitlerine hızlı ulaşmak için.</li>
        <li><strong>RollerToken (RLT):</strong> Oyun içi madenci cihazı ve raf almak için ana para birimi.</li>
      </ul>

      <div class="proof-box">
        <strong>💡 Hızlı İpucu:</strong> Kaydolduktan hemen sonra "My Power" kısmından kazım gücünüzü %100 oranında istediğiniz tek bir koin türüne veya RLT'ye yönlendirebilirsiniz.
      </div>

      <div style="text-align: center; margin-top: 35px;">
        <a href="https://rollercoin.com/?r=mqg589d7" target="_blank" rel="noopener noreferrer" class="cta-btn" style="background: linear-gradient(135deg, #ff007a 0%, #b00052 100%); color: #fff !important; box-shadow: 0 6px 0 #730035;">
          ⚡ 1000 SATOSHI BONUS İLE HESAP AÇ
        </a>
      </div>
    </div>

    <!-- BOTTOM AD SPONSOR -->
    <div class="ad-container">
      <span class="ad-label">Hardware Allocation Sponsor</span>
    </div>

  </div>

</body>
</html>
