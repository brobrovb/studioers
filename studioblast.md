---
layout: page
title: StudioBlast
permalink: /studioblast/
---

<div id="game-wrapper" style="width: 100%; display: flex; justify-content: center; align-items: flex-start; padding: 20px 0; background: #0d1117; min-height: 90vh;">
    <div id="game-outer-container" style="position: relative; width: 100%; max-width: 450px; aspect-ratio: 2 / 3; background: #010409; border-radius: 20px; overflow: hidden; border: 3px solid #00ff88; box-shadow: 0 0 30px rgba(0, 255, 136, 0.2);">
        <div id="game-container" style="position: relative; width: 100%; height: 100%; background: #000;">
            
            <!-- UI Katmanı -->
            <div id="ui" style="position: absolute; top: 0; left: 0; right: 0; padding: 15px; display: flex; justify-content: space-between; align-items: flex-start; color: #00ff88; font-family: sans-serif; font-size: 14px; font-weight: 800; pointer-events: none; z-index: 10; background: linear-gradient(to bottom, rgba(0,0,0,0.9), transparent); text-shadow: 0 0 8px rgba(0,255,136,0.6);">
                <div>SEVİYE: <span id="level">1</span><br>SKOR: <span id="score">0</span></div>
                <div id="ammo-status" style="text-align:center; color:#ff3366; letter-spacing: 1px;">NORMAL MOD</div>
                <div style="text-align:right; margin-right:45px;">TEHLİKE<br><span id="misses">0</span> / 3</div>
            </div>

            <!-- Ses Kontrol -->
            <div id="mute-btn" style="position: absolute; top: 15px; right: 15px; width: 38px; height: 38px; background: rgba(0, 255, 136, 0.15); border: 1px solid #00ff88; border-radius: 50%; cursor: pointer; z-index: 100; display: flex; justify-content: center; align-items: center; backdrop-filter: blur(5px);">
                <svg id="sound-on" viewBox="0 0 24 24" style="width: 20px; height: 20px; fill: #00ff88;"><path d="M14,3.23V5.29C16.89,6.15 19,8.83 19,12C19,15.17 16.89,17.85 14,18.71V20.77C18.07,19.86 21,16.28 21,12C21,7.72 18.07,4.14 14,3.23M16.5,12C16.5,10.23 15.5,8.71 14,7.97V16.02C15.5,15.29 16.5,13.77 16.5,12M3,9V15H7L12,20V4L7,9H3Z"/></svg>
                <svg id="sound-off" style="display:none; width: 20px; height: 20px; fill: #00ff88;" viewBox="0 0 24 24"><path d="M12,4L9.91,6.09L12,8.18M4.27,3L3,4.27L7.73,9H3V15H7L12,20V13.27L16.25,17.53C15.58,18.04 14.83,18.46 14,18.7V20.77C15.38,20.45 16.63,19.82 17.68,18.96L19.73,21L21,19.73L12,10.73M19,12C19,12.94 18.8,13.82 18.46,14.64L19.97,16.15C20.62,14.91 21,13.5 21,12C21,7.72 18.07,4.14 14,3.23V5.29C16.89,6.15 19,8.83 19,12M16.5,12C16.5,11.28 16.34,10.6 16.07,10L14,7.97V12.18L16.45,14.63C16.48,14.42 16.5,14.21 16.5,12Z"/></svg>
            </div>

            <canvas id="game" style="background: radial-gradient(circle at center, #1e293b 0%, #020617 100%); touch-action: none; display: block;"></canvas>

            <!-- Modallar -->
            <div id="startScreen" class="game-modal" style="position: absolute; inset: 0; background: rgba(0, 0, 0, 0.98); display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; text-align: center; padding: 20px;">
                <h1 style="color:#00ff88; font-size: 42px; margin:0; letter-spacing: 2px; text-shadow: 0 0 20px rgba(0,255,136,0.4);">STUDIOBLAST</h1>
                <p style="color:#fff; opacity: 0.6; margin-top: 5px;">A Product of Studioers</p>
                <div style="color:#fff; font-size: 12px; margin: 20px 0; line-height: 1.6; opacity: 0.8;">Her 6 atışta bir BOMBA gelir.<br>Topların çizgiyi geçmesine izin verme!</div>
                <button id="startBtn" style="padding: 18px 40px; font-size: 18px; background: #00ff88; color: #000; border: none; border-radius: 50px; cursor: pointer; font-weight: 900;">BAŞLAT</button>
            </div>

            <div id="gameover" class="game-modal" style="position: absolute; inset: 0; background: rgba(0, 0, 0, 0.96); display: none; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; text-align: center;">
                <h1 style="color:#ff3366; font-size: 38px;">BAŞARISIZ</h1>
                <p style="color:#fff;">Sektör Güvenliği İhlal Edildi</p>
                <button id="restartBtn" style="padding: 15px 40px; background: #ff3366; color: #fff; border: none; border-radius: 50px; font-weight: 900; margin-top: 20px;">YENİDEN DENE</button>
            </div>

            <div id="levelclear" class="game-modal" style="position: absolute; inset: 0; background: rgba(0, 0, 0, 0.96); display: none; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; text-align: center;">
                <h1 style="color:#00ff88; font-size: 38px;">TEMİZLENDİ</h1>
                <button id="nextBtn" style="padding: 15px 40px; background: #00ff88; color: #000; border: none; border-radius: 50px; font-weight: 900; margin-top: 20px;">SONRAKİ SEKTÖR</button>
            </div>
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
            osc.frequency.setValueAtTime(280, now); osc.frequency.exponentialRampToValueAtTime(100, now + 0.1);
            gain.gain.setValueAtTime(0.1, now); gain.gain.linearRampToValueAtTime(0, now + 0.1);
            osc.connect(gain); osc.start(); osc.stop(now + 0.1);
        } else if (type === 'pop') {
            const osc = this.ctx.createOscillator(); osc.type = 'sine';
            osc.frequency.setValueAtTime(450, now);
            gain.gain.setValueAtTime(0.08, now); gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
            osc.connect(gain); osc.start(); osc.stop(now + 0.08);
        } else if (type === 'boom') {
            const osc = this.ctx.createOscillator(); osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(80, now); osc.frequency.exponentialRampToValueAtTime(30, now + 0.4);
            gain.gain.setValueAtTime(0.2, now); gain.gain.linearRampToValueAtTime(0, now + 0.4);
            osc.connect(gain); osc.start(); osc.stop(now + 0.4);
        }
    }
};

const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');
const container = document.getElementById('game-container');
const COLS = 10, ROWS = 20; 
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
    ballRadius = width / (COLS * 2.15);
}

function setupLevel() {
    resize();
    grid = [];
    shootCount = 0;
    missCount = 0;
    document.querySelectorAll('.game-modal').forEach(m => m.style.display = 'none');
    
    // DÜZELTME: Seviye ne olursa olsun her zaman 6 satırla başla (Adil zorluk)
    let fillRows = 6; 
    
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
    let isBomb = (shootCount % 6 === 0);
    let activeColors = new Set();
    for(let r=0; r<ROWS; r++) for(let c=0; c<COLS; c++) if(grid[r][c] && grid[r][c].active && grid[r][c].gravity === 0) activeColors.add(grid[r][c].color);
    let avail = activeColors.size > 0 ? Array.from(activeColors) : COLORS;
    
    currentBubble = { 
        x: width / 2, 
        y: height - ballRadius * 2, 
        color: isBomb ? '#ffcc00' : nextBubble.color, 
        isBomb: isBomb,
        vx: 0, vy: 0, fired: false, active: true 
    };
    
    nextBubble = { color: avail[Math.floor(Math.random()*avail.length)], isBomb: ((shootCount + 1) % 6 === 0) };
    document.getElementById('ammo-status').textContent = isBomb ? "BOMBA YÜKLENDİ" : "NORMAL MOD";
    document.getElementById('ammo-status').style.color = isBomb ? "#ffcc00" : "#00ff88";
}

function getPos(r, c) {
    let offset = (r % 2) * ballRadius;
    return { x: c * (ballRadius * 2) + ballRadius + offset, y: r * (ballRadius * 1.72) + ballRadius + 80 };
}

function burstEffect(x, y, color) {
    effects.push({ x, y, color, life: 1 });
}

function drawBubble(x, y, color, scale = 1, isFired = false, isBomb = false) {
    if (scale <= 0) return;
    ctx.save(); ctx.translate(x, y); ctx.scale(scale, scale);
    if(isBomb) {
        ctx.shadowBlur = 25; ctx.shadowColor = "#ffcc00";
        let grad = ctx.createRadialGradient(0, 0, 0, 0, 0, ballRadius);
        grad.addColorStop(0, "#fff"); grad.addColorStop(0.3, "#ffcc00"); grad.addColorStop(1, "#000");
        ctx.beginPath(); ctx.arc(0, 0, ballRadius - 1, 0, Math.PI * 2); ctx.fillStyle = grad; ctx.fill();
        ctx.strokeStyle = "#fff"; ctx.lineWidth = 2; ctx.stroke();
    } else {
        ctx.shadowBlur = isFired ? 15 : 8; ctx.shadowColor = color;
        let grad = ctx.createRadialGradient(-ballRadius/3, -ballRadius/3, ballRadius/8, 0, 0, ballRadius);
        grad.addColorStop(0, "rgba(255,255,255,0.9)"); grad.addColorStop(0.3, color); grad.addColorStop(1, "#000");
        ctx.beginPath(); ctx.arc(0, 0, ballRadius - 1, 0, Math.PI * 2); ctx.fillStyle = grad; ctx.fill();
    }
    ctx.restore();
}

function loop() {
    ctx.clearRect(0, 0, width, height);
    ctx.setLineDash([8, 8]); ctx.strokeStyle = "rgba(255, 30, 86, 0.4)";
    ctx.lineWidth = 2;
    ctx.beginPath(); ctx.moveTo(0, height * 0.82); ctx.lineTo(width, height * 0.82); ctx.stroke();
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
                    drawBubble(p.x, p.y + b.y_drift, b.color, b.scale);
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
            ctx.beginPath(); ctx.strokeStyle = "rgba(0, 255, 136, 0.2)"; ctx.setLineDash([5, 5]);
            ctx.moveTo(currentBubble.x, currentBubble.y);
            ctx.lineTo(currentBubble.x + Math.cos(angle) * 100, currentBubble.y + Math.sin(angle) * 100);
            ctx.stroke(); ctx.setLineDash([]);
            drawBubble(currentBubble.x, currentBubble.y, currentBubble.color, 1, false, currentBubble.isBomb);
        }
    }

    for (let i = effects.length - 1; i >= 0; i--) {
        let e = effects[i];
        ctx.save(); ctx.globalAlpha = e.life;
        ctx.beginPath(); ctx.strokeStyle = e.color; ctx.lineWidth = 4;
        ctx.arc(e.x, e.y, ballRadius * (2.5 - e.life), 0, Math.PI * 2); ctx.stroke();
        ctx.restore();
        e.life -= 0.08; if (e.life <= 0) effects.splice(i, 1);
    }
    animationId = requestAnimationFrame(loop);
}

function checkCollision() {
    let hit = (currentBubble.y < 80);
    if (!hit) {
        for (let r = 0; r < ROWS; r++) {
            for (let c = 0; c < COLS; c++) {
                let b = grid[r][c];
                if (b && b.active && b.gravity === 0) {
                    let p = getPos(r, c);
                    if (Math.hypot(currentBubble.x - p.x, currentBubble.y - p.y) < ballRadius * 1.5) { hit = true; break; }
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
    for(let r=0; r<ROWS; r++) for(let c=0; c<COLS; c++) if(grid[r][c] && grid[r][c].active && grid[r][c].gravity === 0 && getPos(r,c).y > height * 0.82) lost = true;
    if (lost) { document.getElementById('gameover').style.display = 'flex'; }
    else { updateUI(); spawnNext(); isProcessing = false; }
}

function explode(r, c) {
    burstEffect(getPos(r, c).x, getPos(r, c).y, "#ffcc00");
    let neighbors = r % 2 === 0 ? [[0,-1],[0,1],[-1,0],[-1,-1],[1,0],[1,-1],[0,0]] : [[0,-1],[0,1],[-1,0],[-1,1],[1,0],[1,1],[0,0]];
    for(let [dr, dc] of neighbors) {
        let nr = r+dr, nc = c+dc;
        if(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && grid[nr][nc] && grid[nr][nc].active) {
            burstEffect(getPos(nr, nc).x, getPos(nr, nc).y, grid[nr][nc].color);
            grid[nr][nc].active = false;
            score += 25;
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
        score += matches.length * 20; return true;
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
    angle = Math.atan2((touch.clientY - rect.top) - (currentBubble.y), (touch.clientX - rect.left) - (width / 2));
    if(angle > -0.2) angle = -0.2; if(angle < -Math.PI + 0.2) angle = -Math.PI + 0.2;
};

const handleFire = () => {
    if (isProcessing || !currentBubble || currentBubble.fired) return;
    AudioEngine.play(currentBubble.isBomb ? 'boom' : 'shoot');
    currentBubble.vx = Math.cos(angle) * 16; currentBubble.vy = Math.sin(angle) * 16;
    currentBubble.fired = true;
};

document.getElementById('startBtn').onclick = () => { AudioEngine.init(); setupLevel(); };
document.getElementById('restartBtn').onclick = () => { score = 0; level = 1; setupLevel(); };
document.getElementById('nextBtn').onclick = () => { level++; setupLevel(); };

canvas.addEventListener("touchstart", (e) => { e.preventDefault(); handleInput(e); }, {passive: false});
canvas.addEventListener("touchend", (e) => { e.preventDefault(); handleFire(); }, {passive: false});
canvas.addEventListener("mousemove", handleInput);
canvas.addEventListener("mousedown", handleFire);
window.addEventListener('resize', resize);
window.onload = () => { resize(); requestAnimationFrame(loop); };
</script>
