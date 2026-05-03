---
layout: page
title: StudioBlast
permalink: /studioblast/
---

<div id="game-outer-container" style="background: #010409; display: flex; justify-content: center; align-items: center; height: 90vh; min-height: 500px; border-radius: 15px; overflow: hidden; position: relative; margin: 10px auto; max-width: 600px; border: 2px solid #00ff88;">
    <div id="game-container" style="position: relative; width: 100%; height: 100%; background: #000; overflow: hidden;">
        
        <!-- UI Katmanı -->
        <div id="ui" style="position: absolute; top: 0; left: 0; right: 0; padding: 10px 15px; display: flex; justify-content: space-between; align-items: flex-start; color: #00ff88; font-size: 13px; font-weight: bold; pointer-events: none; z-index: 10; background: linear-gradient(to bottom, rgba(0,0,0,0.8), transparent); text-shadow: 0 0 10px rgba(0,255,136,0.5);">
            <div>SEVİYE: <span id="level">1</span><br>SKOR: <span id="score">0</span></div>
            <div id="ammo-status" style="text-align:center; color:#ff3366;">NORMAL MOD</div>
            <div style="text-align:right; margin-right:45px;">TEHLİKE<br><span id="misses">0</span> / 3</div>
        </div>

        <!-- Ses Kontrol -->
        <div id="mute-btn" style="position: absolute; top: 10px; right: 15px; width: 35px; height: 35px; background: rgba(0, 255, 136, 0.1); border: 1px solid #00ff88; border-radius: 50%; cursor: pointer; z-index: 100; display: flex; justify-content: center; align-items: center;">
            <svg id="sound-on" viewBox="0 0 24 24" style="width: 18px; height: 18px; fill: #00ff88;"><path d="M14,3.23V5.29C16.89,6.15 19,8.83 19,12C19,15.17 16.89,17.85 14,18.71V20.77C18.07,19.86 21,16.28 21,12C21,7.72 18.07,4.14 14,3.23M16.5,12C16.5,10.23 15.5,8.71 14,7.97V16.02C15.5,15.29 16.5,13.77 16.5,12M3,9V15H7L12,20V4L7,9H3Z"/></svg>
            <svg id="sound-off" style="display:none; width: 18px; height: 18px; fill: #00ff88;" viewBox="0 0 24 24"><path d="M12,4L9.91,6.09L12,8.18M4.27,3L3,4.27L7.73,9H3V15H7L12,20V13.27L16.25,17.53C15.58,18.04 14.83,18.46 14,18.7V20.77C15.38,20.45 16.63,19.82 17.68,18.96L19.73,21L21,19.73L12,10.73M19,12C19,12.94 18.8,13.82 18.46,14.64L19.97,16.15C20.62,14.91 21,13.5 21,12C21,7.72 18.07,4.14 14,3.23V5.29C16.89,6.15 19,8.83 19,12M16.5,12C16.5,11.28 16.34,10.6 16.07,10L14,7.97V12.18L16.45,14.63C16.48,14.42 16.5,14.21 16.5,12Z"/></svg>
        </div>

        <canvas id="game" style="background: radial-gradient(circle, #0f172a 0%, #020617 100%); touch-action: none; display: block;"></canvas>

        <div id="startScreen" class="game-modal" style="position: absolute; inset: 0; background: rgba(0, 0, 0, 0.96); display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; text-align: center; padding: 20px;">
            <h1 style="color:#00ff88; font-size: clamp(24px, 8vw, 38px); margin:0;">STUDIOBLAST</h1>
            <p style="color:#fff; opacity: 0.7;">Studioers Tarafından Geliştirildi</p>
            <button id="startBtn" style="padding: 15px 35px; font-size: 16px; background: #00ff88; color: #000; border: none; border-radius: 12px; cursor: pointer; font-weight: bold; margin-top: 20px;">GÖREVE BAŞLA</button>
        </div>

        <div id="gameover" class="game-modal" style="position: absolute; inset: 0; background: rgba(0, 0, 0, 0.96); display: none; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; text-align: center;">
            <h1 style="color:#ff3366; font-size: 32px;">OYUN BİTTİ</h1>
            <p style="color:#fff;">Sektör Sınırı Aşıldı</p>
            <button id="restartBtn" style="padding: 15px 35px; background: #ff3366; color: #fff; border: none; border-radius: 12px; font-weight: bold; margin-top: 20px;">TEKRAR DENE</button>
        </div>

        <div id="levelclear" class="game-modal" style="position: absolute; inset: 0; background: rgba(0, 0, 0, 0.96); display: none; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; text-align: center;">
            <h1 style="color:#00ff88; font-size: 32px;">TEMİZLENDİ</h1>
            <button id="nextBtn" style="padding: 15px 35px; background: #00ff88; color: #000; border: none; border-radius: 12px; font-weight: bold; margin-top: 20px;">SIRAKİ SEKTÖR</button>
        </div>
    </div>
</div>

<script>
const AudioEngine = {
    ctx: null, isMuted: false,
    init() { if(!this.ctx) this.ctx = new (window.AudioContext || window.webkitAudioContext)(); },
    play(type) {
        if(this.isMuted) return; this.init();
        if (this.ctx.state === 'suspended') this.ctx.resume();
        const now = this.ctx.currentTime;
        const gain = this.ctx.createGain(); gain.connect(this.ctx.destination);
        if (type === 'shoot') {
            const osc = this.ctx.createOscillator(); osc.type = 'sine';
            osc.frequency.setValueAtTime(250, now); osc.frequency.exponentialRampToValueAtTime(80, now + 0.12);
            gain.gain.setValueAtTime(0.15, now); gain.gain.linearRampToValueAtTime(0, now + 0.12);
            osc.connect(gain); osc.start(); osc.stop(now + 0.12);
        } else if (type === 'pop') {
            const osc = this.ctx.createOscillator(); osc.type = 'triangle';
            osc.frequency.setValueAtTime(400, now);
            gain.gain.setValueAtTime(0.1, now); gain.gain.exponentialRampToValueAtTime(0.01, now + 0.1);
            osc.connect(gain); osc.start(); osc.stop(now + 0.1);
        } else if (type === 'boom') {
            const osc = this.ctx.createOscillator(); osc.type = 'square';
            osc.frequency.setValueAtTime(100, now); osc.frequency.exponentialRampToValueAtTime(40, now + 0.3);
            gain.gain.setValueAtTime(0.3, now); gain.gain.linearRampToValueAtTime(0, now + 0.3);
            osc.connect(gain); osc.start(); osc.stop(now + 0.3);
        }
    }
};

const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');
const container = document.getElementById('game-container');
const COLS = 10, ROWS = 22; 
const COLORS = ['#FF1E56', '#00F5FF', '#F9D923', '#A125FB', '#32FF6A'];

let width, height, ballRadius, animationId;
let grid = [], score = 0, level = 1, missCount = 0, shootCount = 0;
let currentBubble = null, nextBubble = null, angle = -Math.PI / 2;
let isProcessing = true, effects = []; 

function resize() {
    width = container.offsetWidth;
    height = container.offsetHeight;
    canvas.width = width;
    canvas.height = height;
    ballRadius = width / (COLS * 2.1);
}

function setupLevel() {
    resize();
    grid = [];
    shootCount = 0;
    document.querySelectorAll('.game-modal').forEach(m => m.style.display = 'none');
    let fillRows = Math.min(5 + level, 12); // Zorluk artırıldı
    for (let r = 0; r < ROWS; r++) {
        grid[r] = [];
        for (let c = 0; c < COLS; c++) {
            if (r < fillRows) grid[r][c] = { color: COLORS[Math.floor(Math.random() * COLORS.length)], active: true, scale: 1, gravity: 0, y_drift: 0 };
            else grid[r][c] = null;
        }
    }
    nextBubble = { color: COLORS[Math.floor(Math.random() * COLORS.length)], isBomb: false };
    spawnNext();
    isProcessing = false;
    updateUI();
}

function spawnNext() {
    shootCount++;
    let isBomb = (shootCount % 6 === 0); // Her 6 atışta bir bomba
    
    let activeColors = new Set();
    for(let r=0; r<ROWS; r++) for(let c=0; c<COLS; c++) if(grid[r][c] && grid[r][c].active && grid[r][c].gravity === 0) activeColors.add(grid[r][c].color);
    let avail = activeColors.size > 0 ? Array.from(activeColors) : COLORS;
    
    currentBubble = { 
        x: width / 2, 
        y: height - 60, 
        color: isBomb ? '#000000' : nextBubble.color, 
        isBomb: isBomb,
        vx: 0, vy: 0, fired: false, active: true 
    };
    
    nextBubble = { color: avail[Math.floor(Math.random()*avail.length)], isBomb: ((shootCount + 1) % 6 === 0) };
    document.getElementById('ammo-status').textContent = isBomb ? "!!! BOMBA MODU !!!" : "NORMAL MOD";
    document.getElementById('ammo-status').style.color = isBomb ? "#ff3366" : "#00ff88";
}

function getPos(r, c) {
    let offset = (r % 2) * ballRadius;
    return { x: c * (ballRadius * 2) + ballRadius + offset, y: r * (ballRadius * 1.72) + ballRadius + 60 };
}

function burstEffect(x, y, color) {
    effects.push({ x, y, color, size: ballRadius, life: 1 });
}

function drawBubble(x, y, color, scale = 1, isFired = false, isBomb = false) {
    if (scale <= 0) return;
    ctx.save(); ctx.translate(x, y); ctx.scale(scale, scale);
    
    if(isBomb) {
        ctx.shadowBlur = 20; ctx.shadowColor = "#ff0000";
        let grad = ctx.createRadialGradient(0, 0, 0, 0, 0, ballRadius);
        grad.addColorStop(0, "#440000"); grad.addColorStop(0.5, "#000000"); grad.addColorStop(1, "#ff0000");
        ctx.beginPath(); ctx.arc(0, 0, ballRadius - 2, 0, Math.PI * 2); ctx.fillStyle = grad; ctx.fill();
        // Bomba ikonu (fitil)
        ctx.fillStyle = "white"; ctx.fillRect(-2, -ballRadius, 4, 6);
    } else {
        ctx.shadowBlur = isFired ? 15 : 8; ctx.shadowColor = color;
        let grad = ctx.createRadialGradient(-ballRadius/3, -ballRadius/3, ballRadius/10, 0, 0, ballRadius);
        grad.addColorStop(0, "#fff"); grad.addColorStop(0.2, color); grad.addColorStop(1, "#000");
        ctx.beginPath(); ctx.arc(0, 0, ballRadius - 2, 0, Math.PI * 2); ctx.fillStyle = grad; ctx.fill();
    }
    ctx.restore();
}

function loop() {
    ctx.clearRect(0, 0, width, height);
    
    ctx.setLineDash([5, 5]); ctx.strokeStyle = "rgba(255, 30, 86, 0.4)";
    ctx.beginPath(); ctx.moveTo(0, height * 0.8); ctx.lineTo(width, height * 0.8); ctx.stroke();
    ctx.setLineDash([]);

    if (!isProcessing && grid.length > 0) {
        let activeInGrid = 0;
        for (let r = 0; r < ROWS; r++) {
            for (let c = 0; c < COLS; c++) {
                let b = grid[r][c];
                if (b && b.active) {
                    if (b.gravity === 0) activeInGrid++;
                    let p = getPos(r, c);
                    if (b.gravity > 0) { b.y_drift += b.gravity; b.gravity += 0.5; if (p.y + b.y_drift > height + 50) b.active = false; }
                    drawBubble(p.x, p.y + b.y_drift, b.color, b.scale, false, false);
                }
            }
        }
        if (activeInGrid === 0 && !currentBubble.fired) {
            document.getElementById('levelclear').style.display = 'flex';
            isProcessing = true;
        }
    }

    if (currentBubble && currentBubble.active) {
        if (currentBubble.fired) {
            currentBubble.x += currentBubble.vx; currentBubble.y += currentBubble.vy;
            if (currentBubble.x < ballRadius || currentBubble.x > width - ballRadius) currentBubble.vx *= -1;
            checkCollision();
            drawBubble(currentBubble.x, currentBubble.y, currentBubble.color, 1, true, currentBubble.isBomb);
        } else if (!isProcessing) {
            drawBubble(currentBubble.x, currentBubble.y, currentBubble.color, 1, false, currentBubble.isBomb);
        }
    }

    // Patlama efektlerini çiz
    for (let i = effects.length - 1; i >= 0; i--) {
        let e = effects[i];
        ctx.save(); ctx.globalAlpha = e.life;
        ctx.beginPath(); ctx.strokeStyle = e.color; ctx.lineWidth = 3;
        ctx.arc(e.x, e.y, ballRadius * (2 - e.life), 0, Math.PI * 2); ctx.stroke();
        ctx.restore();
        e.life -= 0.05; if (e.life <= 0) effects.splice(i, 1);
    }

    animationId = requestAnimationFrame(loop);
}

function checkCollision() {
    let hit = (currentBubble.y < 60);
    if (!hit) {
        for (let r = 0; r < ROWS; r++) {
            for (let c = 0; c < COLS; c++) {
                let b = grid[r][c];
                if (b && b.active && b.gravity === 0) {
                    let p = getPos(r, c);
                    if (Math.hypot(currentBubble.x - p.x, currentBubble.y - p.y) < ballRadius * 1.4) { hit = true; break; }
                }
            }
            if(hit) break;
        }
    }
    if (hit) snap();
}

function snap() {
    isProcessing = true;
    let bestDist = Infinity, tr = 0, tc = 0;
    for (let r = 0; r < ROWS; r++) {
        for (let c = 0; c < COLS; c++) {
            if (grid[r][c] && grid[r][c].active) continue;
            let p = getPos(r, c);
            let d = Math.hypot(currentBubble.x - p.x, currentBubble.y - p.y);
            if (d < bestDist) { bestDist = d; tr = r; tc = c; }
        }
    }

    if (currentBubble.isBomb) {
        AudioEngine.play('boom');
        explode(tr, tc);
    } else {
        grid[tr][tc] = { color: currentBubble.color, active: true, scale: 1, y_drift: 0, gravity: 0 };
        if (checkMatches(tr, tc)) {
            AudioEngine.play('pop');
            missCount = 0;
        } else {
            missCount++;
            if (missCount >= 3) {
                grid.unshift(new Array(COLS).fill(null).map(() => ({ color: COLORS[Math.floor(Math.random()*COLORS.length)], active: true, scale: 1, y_drift: 0, gravity: 0 })));
                grid.pop(); missCount = 0;
            }
        }
    }
    
    currentBubble.active = false;
    handleFloating();

    let lost = false;
    for(let r=0; r<ROWS; r++) for(let c=0; c<COLS; c++) if(grid[r][c] && grid[r][c].active && grid[r][c].gravity === 0 && getPos(r,c).y > height * 0.8) lost = true;
    if (lost) { document.getElementById('gameover').style.display = 'flex'; isProcessing = true; }
    else { updateUI(); spawnNext(); isProcessing = false; }
}

function explode(r, c) {
    burstEffect(getPos(r, c).x, getPos(r, c).y, "#ff3366");
    let neighbors = r % 2 === 0 ? [[0,-1],[0,1],[-1,0],[-1,-1],[1,0],[1,-1],[0,0]] : [[0,-1],[0,1],[-1,0],[-1,1],[1,0],[1,1],[0,0]];
    for(let [dr, dc] of neighbors) {
        let nr = r+dr, nc = c+dc;
        if(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && grid[nr][nc] && grid[nr][nc].active) {
            burstEffect(getPos(nr, nc).x, getPos(nr, nc).y, grid[nr][nr] ? grid[nr][nc].color : "#fff");
            grid[nr][nc].active = false;
            score += 20;
        }
    }
}

function checkMatches(r, c) {
    let color = grid[r][c].color, matches = [[r, c]], visited = new Set([r+","+c]), i = 0;
    while(i < matches.length) {
        let [currR, currC] = matches[i++];
        let neighbors = currR % 2 === 0 ? [[0,-1],[0,1],[-1,0],[-1,-1],[1,0],[1,-1]] : [[0,-1],[0,1],[-1,0],[-1,1],[1,0],[1,1]];
        for(let [dr, dc] of neighbors) {
            let nr = currR+dr, nc = currC+dc;
            if(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && grid[nr][nc] && grid[nr][nc].active && grid[nr][nc].color === color && !visited.has(nr+","+nc)) {
                visited.add(nr+","+nc); matches.push([nr, nc]);
            }
        }
    }
    if (matches.length >= 3) {
        matches.forEach(([mr, mc]) => { 
            burstEffect(getPos(mr, mc).x, getPos(mr, mc).y, color);
            grid[mr][mc].active = false; 
        });
        score += matches.length * 15; return true;
    }
    return false;
}

function handleFloating() {
    let connected = new Set(), queue = [];
    for (let c = 0; c < COLS; c++) if (grid[0][c] && grid[0][c].active) { connected.add("0,"+c); queue.push([0, c]); }
    let i = 0;
    while(i < queue.length) {
        let [currR, currC] = queue[i++];
        let neighbors = currR % 2 === 0 ? [[0,-1],[0,1],[-1,0],[-1,-1],[1,0],[1,-1]] : [[0,-1],[0,1],[-1,0],[-1,1],[1,0],[1,1]];
        for(let [dr, dc] of neighbors) {
            let nr = currR+dr, nc = currC+dc;
            if(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && grid[nr][nc] && grid[nr][nc].active && !connected.has(nr+","+nc)) {
                connected.add(nr+","+nc); queue.push([nr, nc]);
            }
        }
    }
    for (let r = 0; r < ROWS; r++) for (let c = 0; c < COLS; c++) if (grid[r][c] && grid[r][c].active && !connected.has(r+","+c)) grid[r][c].gravity = 1;
}

function updateUI() {
    document.getElementById('score').textContent = score;
    document.getElementById('level').textContent = level;
    document.getElementById('misses').textContent = missCount;
}

const handleInput = (e) => {
    if(isProcessing || !currentBubble) return;
    const rect = canvas.getBoundingClientRect();
    const touch = e.touches ? e.touches[0] : e;
    angle = Math.atan2((touch.clientY - rect.top) - (height - 60), (touch.clientX - rect.left) - (width / 2));
};

const handleFire = () => {
    if (isProcessing || !currentBubble || currentBubble.fired) return;
    AudioEngine.play(currentBubble.isBomb ? 'boom' : 'shoot');
    currentBubble.vx = Math.cos(angle) * 15; currentBubble.vy = Math.sin(angle) * 15;
    currentBubble.fired = true;
};

document.getElementById('startBtn').onclick = () => { AudioEngine.init(); setupLevel(); };
document.getElementById('restartBtn').onclick = () => setupLevel();
document.getElementById('nextBtn').onclick = () => { level++; setupLevel(); };

canvas.addEventListener("touchstart", (e) => { e.preventDefault(); handleInput(e); }, {passive: false});
canvas.addEventListener("touchend", (e) => { e.preventDefault(); handleFire(); }, {passive: false});
canvas.addEventListener("mousemove", handleInput);
canvas.addEventListener("mousedown", handleFire);

window.addEventListener('resize', resize);
window.onload = () => { resize(); requestAnimationFrame(loop); };
</script>

### Oyun Hakkında
**StudioBlast**, Studioers tarafından geliştirilen, karavan hayatının minimalist enerjisini dijital dünyaya taşıyan estetik bir balon patlatma oyunudur.

**Yenilikler:**
*   **Bomba Topu:** Her 6 atışta bir gelen özel yıkıcı top!
*   **Artan Zorluk:** Seviye ilerledikçe daha fazla satırla mücadele et.
*   **Gelişmiş Görsel:** Geri dönen patlama efektleri ve daha geniş oyun alanı.

**Nasıl Oynanır?**
*   **Nişan Al:** Fareyi hareket ettir veya ekrana dokun.
*   **Ateş Et:** Tıkla veya dokunmayı bırak.
*   **Patlat:** Aynı renkten 3 veya daha fazla balonu birleştir. Bomba gelirse stratejik kullan!
