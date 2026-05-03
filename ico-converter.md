---
layout: default
title: "Online PNG - ICO Dönüştürücü"
description: "Resimlerinizi saniyeler içinde favicon (.ico) formatına dönüştürün. Tamamen ücretsiz ve online."
---

<div class="tool-container" style="max-width: 600px; margin: 40px auto; padding: 20px; border: 1px solid #bbdefb; border-radius: 20px; background: #fff; box-shadow: 0 10px 25px rgba(0,0,0,0.05); text-align: center;">
  
  <h2 style="color: #1565c0;">🎨 Online ICO Dönüştürücü</h2>
  <p style="color: #666;">Resmini buraya sürükle veya seç, anında faviconun hazır olsun!</p>

  <div id="drop-area" style="border: 3px dashed #64b5f6; border-radius: 15px; padding: 40px; margin: 20px 0; cursor: pointer; transition: 0.3s;" onmouseover="this.style.background='#e3f2fd'" onmouseout="this.style.background='transparent'">
    <input type="file" id="fileElem" accept="image/*" style="display:none" onchange="handleFiles(this.files)">
    <label for="fileElem" style="cursor: pointer;">
      <img src="/images/favicon-32x32.png" style="width: 50px; opacity: 0.5; margin-bottom: 10px;"><br>
      <strong>Resim Seç veya Sürükle</strong>
    </label>
  </div>

  <div id="preview-area" style="display:none; margin-top: 20px;">
    <canvas id="canvas" width="32" height="32" style="border: 1px solid #ccc; border-radius: 4px;"></canvas>
    <p id="file-info" style="font-size: 14px; color: #444; margin: 10px 0;"></p>
    <a id="download-btn" href="#" download="favicon.ico" style="display: inline-block; background: #1565c0; color: white; padding: 12px 25px; border-radius: 10px; text-decoration: none; font-weight: bold; box-shadow: 0 4px 10px rgba(21, 101, 192, 0.3);">📥 .ICO Olarak İndir</a>
  </div>

</div>

<script>
  function handleFiles(files) {
    const file = files[0];
    if (!file) return;

    const reader = new FileReader();
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const previewArea = document.getElementById('preview-area');
    const downloadBtn = document.getElementById('download-btn');
    const fileInfo = document.getElementById('file-info');

    reader.onload = function(event) {
      const img = new Image();
      img.onload = function() {
        // Profesyonel favicon boyutları için temiz çizim
        ctx.clearRect(0, 0, 32, 32);
        ctx.drawImage(img, 0, 0, 32, 32);
        
        // Canvas içeriğini indirme linkine çevir
        const dataUrl = canvas.toDataURL('image/x-icon'); 
        downloadBtn.href = dataUrl;
        
        fileInfo.textContent = `"${file.name}" başarıyla dönüştürüldü!`;
        previewArea.style.display = 'block';
      };
      img.src = event.target.result;
    };
    reader.readAsDataURL(file);
  }

  // Sürükle-Bırak Desteği
  const dropArea = document.getElementById('drop-area');
  ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
    dropArea.addEventListener(eventName, preventDefaults, false);
  });
  function preventDefaults (e) { e.preventDefault(); e.stopPropagation(); }
  dropArea.addEventListener('drop', e => { handleFiles(e.dataTransfer.files); }, false);
</script>
