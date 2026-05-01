---
layout: page
title: Studioers AI Art Generator
permalink: /ai-art/
---

<style>
    :root {
        --accent: #00ff88;
        --surface: #111113;
        --text: #ffffff;
        --gray: #888;
        --border: #1f1f23;
        --bybit: #f7a600;
    }

    .ai-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100%;
        font-family: 'Plus Jakarta Sans', sans-serif;
    }

    .main-card { 
        background: var(--surface); 
        border: 1px solid var(--border); 
        padding: 40px; 
        border-radius: 32px; 
        width: 100%; 
        max-width: 500px; 
        text-align: center; 
        box-shadow: 0 40px 100px rgba(0,0,0,0.5); 
        margin-bottom: 60px;
    }

    .ai-card-h1 { font-size: 32px; background: linear-gradient(135deg, var(--accent), #00bdff); -webkit-background-clip: text; -webkit-text-fill-color: transparent; margin-bottom: 5px; font-weight: 800; letter-spacing: -1.5px; border:none; }
    .subtitle { color: var(--gray); font-size: 11px; margin-bottom: 30px; text-transform: uppercase; letter-spacing: 3px; font-weight: bold; }

    .ai-input { width: 100%; padding: 18px; background: #000; border: 2px solid var(--border); color: #fff; border-radius: 16px; outline: none; transition: 0.3s; font-size: 16px; margin-bottom: 15px; text-align: center; box-sizing: border-box; }
    .ai-input:focus { border-color: var(--accent); }

    .btn-generate { background: var(--accent); color: #000; border: none; padding: 18px; border-radius: 16px; cursor: pointer; font-weight: 800; width: 100%; font-size: 16px; transition: 0.3s; text-transform: uppercase; margin-bottom: 10px; }
    .btn-generate:hover { transform: translateY(-2px); box-shadow: 0 10px 20px rgba(0,255,136,0.2); }

    .promo-area { display: flex; gap: 10px; margin-bottom: 25px; }
    .btn-promo { 
        flex: 1; text-decoration: none; font-size: 10px; font-weight: 800; padding: 12px; 
        border-radius: 12px; border: 1px solid var(--border); color: var(--gray);
        transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; text-align: center;
    }
    .btn-promo:hover { border-color: #ff424d; color: #ff424d; background: rgba(255, 66, 77, 0.05); }

    #res-container { margin-top: 10px; min-height: 350px; display: flex; flex-direction: column; align-items: center; justify-content: center; border: 2px dashed var(--border); border-radius: 28px; background: #000; overflow: hidden; position: relative; width: 100%; }
    #result { width: 100%; border-radius: 24px; display: none; }
    
    .spinner { display: none; width: 40px; height: 40px; border: 4px solid rgba(0, 255, 136, 0.1); border-left-color: var(--accent); border-radius: 50%; animation: spin 1s linear infinite; margin-bottom: 15px; }
    @keyframes spin { to { transform: rotate(360deg); } }
    .loading-text { display: none; color: var(--accent); font-family: 'JetBrains Mono', monospace; font-size: 12px; }

    .btn-download { background: #fff; color: #000; border: none; padding: 14px; border-radius: 16px; cursor: pointer; font-weight: 800; width: 100%; margin-top: 25px; display: none; font-size: 14px; }

    .seo-content { width: 100%; text-align: left; line-height: 1.8; color: #555; font-size: 15px; margin-top: 50px; border-top: 1px solid #ddd; padding-top: 30px; }
    .seo-content h2 { color: #1565c0; font-size: 28px; margin-top: 40px; }
    .highlight { color: #1565c0; font-weight: 600; }
</style>

<div class="ai-container">
    <div class="main-card">
        <h1 class="ai-card-h1">STUDIOERS <span style="color:#fff">AI</span></h1>
        <p class="subtitle">Sinirsel Vizyon Motoru</p>
        
        <input type="text" id="prompt" class="ai-input" placeholder="Örn: Gece vakti siberpunk Antalya şehri...">
        <button class="btn-generate" onclick="generate()">Şaheser Oluştur</button>
        
        <div class="promo-area">
            <a href="https://www.patreon.com/studioers" target="_blank" class="btn-promo">Laboratuvarı Destekle</a>
            <a href="https://www.bybit.com/invite?ref=6L9ZDXJ" target="_blank" class="btn-promo bybit">Bybit'te İşlem Yap</a>
        </div>

        <div id="res-container">
            <div id="spinner" class="spinner"></div>
            <div id="loaderText" class="loading-text">BAŞLATILIYOR...</div>
            <img id="result" alt="Studioers AI Görüntü Oluşturucu">
        </div>

        <button id="dlBtn" class="btn-download" onclick="download()">SANAT ESERİNİ İNDİR (PNG)</button>
    </div>

    <article class="seo-content">
        <h2>Üretken Yapay Zekanın Gücünü Serbest Bırakın: Studioers Vizyonu</h2>
        <p>Dijital yaratıcılığın sınırına hoş geldiniz. <span class="highlight">Studioers.xyz</span> olarak, sorunsuz ve ücretsiz bir yapay zeka sanatı deneyimi sunmak için en yeni <span class="white-bold">Stable Diffusion</span> ve <span class="white-bold">FLUX.1</span> modellerini kullanıyoruz. Motorumuz sadece bir araç değil; insan hayal gücü ile makine hassasiyeti arasında bir köprüdür.</p>

        <h3>Yaratıcı Projeleriniz İçin Neden Studioers AI?</h3>
        <p>Yapay zekanın hızla gelişen dünyasında hız ve kalite her şeydir. İster <span class="white-bold">Godot Engine</span> üzerinde çalışan bir oyun geliştiricisi, ister bir Android mimarı veya benzersiz sosyal medya içerikleri arayan bir dijital göçebe olun, Sinirsel Vizyon Motorumuz saniyeler içinde yüksek kaliteli sonuçlar sunar.</p>

        <h3>Latent Diffusion Modellerinin Bilimi</h3>
        <p>"Oluştur" butonuna bastığınız her anın arkasında karmaşık bir <span class="highlight">Sinir Ağları</span> yapısı çalışır. Sistemimiz, yapay zekanın saf gürültüden başlayarak bunu adım adım net ve yüksek çözünürlüklü bir görüntüye dönüştürdüğü <strong>Latent Diffusion</strong> sürecini kullanır. Bu teknoloji, stil, ışık ve kompozisyon üzerinde benzersiz bir kontrol sağlar.</p>

        <h3>Geleceği Kurmak: Geliştiriciler ve Göçebeler</h3>
        <p>Studioers, <span class="highlight">Göçebe Teknoloji Yaşam Tarzı</span>'nın merkezinde doğdu. Antalya'daki bir karavandan Tokyo'daki bir gökdelene kadar her yerde çalışan güçlü araçlara duyulan ihtiyacı anlıyoruz. Yapay zekayı iş akışınıza dahil ederek pahalı stok fotoğraflara olan ihtiyacı ortadan kaldırırsınız. Üretir, yineler ve yayına alırsınız.</p>
    </article>
</div>

<script>
    const loadingMessages = ["PİKSELLER SENKRONİZE EDİLİYOR...", "İÇERİK HAYAL EDİLİYOR...", "SANAT STİLLENDİRİLİYOR...", "NEREDEYSE HAZIR...", "SONUÇLANDIRILIYOR..."];
    let messageInterval;

    function generate() {
        const prompt = document.getElementById('prompt').value.trim();
        const spinner = document.getElementById('spinner');
        const loaderText = document.getElementById('loaderText');
        const img = document.getElementById('result');
        const dlBtn = document.getElementById('dlBtn');

        if(!prompt) return alert("Lütfen bir açıklama girin!");

        spinner.style.display = 'block';
        loaderText.style.display = 'block';
        img.style.display = 'none';
        dlBtn.style.display = 'none';

        let i = 0;
        clearInterval(messageInterval);
        messageInterval = setInterval(() => {
            loaderText.innerText = loadingMessages[i % loadingMessages.length];
            i++;
        }, 2000);

        const seed = Math.floor(Math.random() * 9999999);
        const url = `https://image.pollinations.ai/prompt/${encodeURIComponent(prompt)}?width=1024&height=1024&seed=${seed}&nologo=true`;

        img.src = url;
        img.onload = () => {
            clearInterval(messageInterval);
            spinner.style.display = 'none';
            loaderText.style.display = 'none';
            img.style.display = 'block';
            dlBtn.style.display = 'block';
        };
        img.onerror = () => {
            clearInterval(messageInterval);
            spinner.style.display = 'none';
            loaderText.style.display = 'none';
            alert("Bağlantı hatası. Lütfen tekrar deneyin!");
        };
    }

    async function download() {
        try {
            const img = document.getElementById('result');
            const response = await fetch(img.src);
            const blob = await response.blob();
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `StudioersAI_${Date.now()}.png`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
        } catch (err) {
            alert("Resmi kaydetmek için lütfen üzerine sağ tıklayıp farklı kaydet deyin.");
        }
    }
</script>
