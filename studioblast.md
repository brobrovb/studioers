# StudioBlast - Aesthetic Explosion (Studioers Edition)

Bu proje, **Studioers** markasının dijital yol haritasının bir parçası olarak, estetik ve modern bir oyun deneyimi sunar.

---

## Tam Kaynak Kod (index.html)
```html
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>StudioBlast - Estetik Patlama</title>
    <style>
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; outline: none; }
        body { 
            margin: 0; padding: 0; background: #010409; 
            display: flex; justify-content: center; align-items: center; 
            height: 100vh; width: 100vw; overflow: hidden; 
            font-family: 'Segoe UI', Roboto, sans-serif; position: fixed;
        }
        #game-container { 
            position: relative; width: 100%; height: 100%; max-width: 450px; max-height: 850px; 
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            background: #000; box-shadow: 0 0 50px rgba(0,0,0,0.5); overflow: hidden;
        }
        canvas { background: radial-gradient(circle, #0f172a 0%, #020617 100%); touch-action: none; display: block; width: 100%; height: 100%; }
        
        #ui { 
            position: absolute; top: 0; left: 0; right: 0; padding: 15px 20px;
            display: flex; justify-content: space-between; align-items: flex-start;
            color: #00ff88; font-size: 14px; font-weight: bold; pointer-events: none; z-index: 10; 
            background: linear-gradient(to bottom, rgba(0,0,0,0.8), transparent);
            text-shadow: 0 0 10px rgba(0,255,136,0.5);
        }

        #mute-btn {
            position: absolute; top: 15px; right: 20px;
            width: 40px; height: 40px;
            background: rgba(0, 255, 136, 0.1);
            border: 1px solid #00ff88; border-radius: 50%;
            cursor: pointer; z-index: 100; pointer-events: auto;
            display: flex; justify-content: center; align-items: center;
            box-shadow: 0 0 15px rgba(0,255,136,0.2);
        }
        #mute-btn svg { width: 20px; height: 20px; fill: #00ff88; }

        .modal { position: absolute; inset: 0; background: rgba(0, 0, 0, 0.96); display: none; flex-direction: column; justify-content: center; align-items: center; z-index: 2000; backdrop-filter: blur(15px); padding: 20px; text-align: center; }
        h1 { color:#00ff88; font-size: 42px; margin:0; text-shadow: 0 0 20px rgba(0,255,136,0.6); letter-spacing: 2px; }
        button { 
            padding: 18px 45px; font-size: 20px; background: #00ff88; color: #000; 
            border: none; border-radius: 12px; cursor: pointer; font-weight: bold; 
            margin-top: 25px; text-transform: uppercase; box-shadow: 0 0 20px rgba(0,255,136,0.4);
            transition: transform 0.1s;
        }
        button:active { transform: scale(0.95); }
    </style>
</head>
<body>

    <div id="game-container">
        <div id="ui">
            <div>SEVİYE: <span id="level">1</span><br>SKOR: <span id="score">0</span></div>
            <div style="text-align:right; margin-right:50px;">TEHLİKE<br><span id="misses">0</span> / 3</div>
        </div>

        <div id="mute-btn">
            <svg id="sound-on" viewBox="0 0 24 24"><path d="M14,3.23V5.29C16.89,6.15 19,8.83 19,12C19,15.17 16.89,17.85 14,18.71V20.77C18.07,19.86 21,16.28 21,12C21,7.72 18.07,4.14 14,3.23M16.5,12C16.5,10.23 15.5,8.71 14,7.97V16.02C15.5,15.29 16.5,13.77 16.5,12M3,9V15H7L12,20V4L7,9H3Z"/></svg>
            <svg id="sound-off" style="display:none;" viewBox="0 0 24 24"><path d="M12,4L9.91,6.09L12,8.18M4.27,3L3,4.27L7.73,9H3V15H7L12,20V13.27L16.25,17.53C15.58,18.04 14.83,18.46 14,18.7V20.77C15.38,20.45 16.63,19.82 17.68,18.96L19.73,21L21,19.73L12,10.73M19,12C19,12.94 18.8,13.82 18.46,14.64L19.97,16.15C20.62,14.91 21,13.5 21,12C21,7.72 18.07,4.14 14,3.23V5.29C16.89,6.15 19,8.83 19,12M16.5,12C16.5,11.28 16.34,10.6 16.07,10L14,7.97V12.18L16.45,14.63C16.48,14.42 16.5,14.21 16.5,12Z"/></svg>
        </div>

        <canvas id="game"></canvas>

        <div id="startScreen" class="modal" style="display: flex;">
            <h1>STUDIOBLAST</h1>
            <p style="color:#fff; opacity: 0.7; margin-top:10px;">by Studioers</p>
            <button id="startBtn">GÖREVE BAŞLA</button>
        </div>

        <div id="gameover" class="modal">
            <h1 style="color:#ff3366;">OYUN BİTTİ</h1>
            <p style="color:#fff; opacity: 0.8; margin-top:10px;">Sektör Kaybedildi</p>
            <button id="restartBtn">TEKRAR DENE</button>
        </div>

        <div id="levelclear" class="modal">
            <h1>TEMİZLENDİ</h1>
            <button id="nextBtn">SIRAKİ SEKTÖR</button>
        </div>
    </div>

<script>
// Logic ve Ses Motoru (Dokunulmadı)
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
        }
    }
};

const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');
const container = document.getElementById('game-container');
const COLS = 10, ROWS = 22; 
const COLORS = ['#FF1E56', '#00F5FF', '#F9D923', '#A125FB', '#32FF6A'];

let width, height, ballRadius, animationId;
let grid = [], score = 0, level = 1, missCount = 0;
let currentBubble = null, nextBubble = null, angle = -Math.PI / 2;
let isProcessing = true, particles = [], effects = []; 

function resize() {
    width = container.clientWidth; height = container.clientHeight;
    canvas.width = width; canvas.height = height;
    ballRadius = width / (COLS * 2.1);
}

function init() {
    resize();
    document.getElementById('startScreen').style.display = 'flex';
    requestAnimationFrame(loop);
}

function resetGame() {
    score = 0; level = 1; missCount = 0; effects = []; particles = [];
    setupLevel();
}

function nextLevel() {
    level++; missCount = 0; effects = []; particles = [];
    setupLevel();
}

function setupLevel() {
    resize();
    grid = [];
    document.querySelectorAll('.modal').forEach(m => m.style.display = 'none');
    let fillRows = Math.min(4 + level, 9);
    for (let r = 0; r < ROWS; r++) {
        grid[r] = [];
        for (let c = 0; c < COLS; c++) {
            if (r < fillRows) grid[r][c] = { color: COLORS[Math.floor(Math.random() * COLORS.length)], active: true, scale: 1, gravity: 0, y_drift: 0, animX: 0, animY: 0 };
            else grid[r][c] = null;
        }
    }
    nextBubble = { color: COLORS[Math.floor(Math.random() * COLORS.length)] };
    spawnNext();
    isProcessing = false;
    updateUI();
}

function spawnNext() {
    let activeColors = new Set();
    for(let r=0; r<ROWS; r++) for(let c="0;" c<COLS; c++) if(grid[r][c] && grid[r][c].active grid[r][c].gravity="==" 0) activeColors.add(grid[r][c].color); let avail="activeColors.size"> 0 ? Array.from(activeColors) : COLORS;
    let targetColor = avail.includes(nextBubble.color) ? nextBubble.color : avail[Math.floor(Math.random()*avail.length)];
    currentBubble = { x: width / 2, y: height - 80, color: targetColor, vx: 0, vy: 0, fired: false, active: true };
    nextBubble = { color: avail[Math.floor(Math.random()*avail.length)] };
}

function getPos(r, c) {
    let offset = (r % 2) * ballRadius;
    return { x: c * (ballRadius * 2) + ballRadius + offset, y: r * (ballRadius * 1.72) + ballRadius + 85 };
}

function burstEffect(x, y, color) {
    effects.push({ x, y, color, size: ballRadius, life: 1 });
}

function drawBubble(x, y, color, scale = 1, isFired = false) {
    if (scale <= 0) return;
    ctx.save(); ctx.translate(x, y); ctx.scale(scale, scale);
    ctx.shadowBlur = isFired ? 15 : 8; ctx.shadowColor = color;
    let grad = ctx.createRadialGradient(-ballRadius/3, -ballRadius/3, ballRadius/10, 0, 0, ballRadius);
    grad.addColorStop(0, "#fff"); grad.addColorStop(0.2, color); grad.addColorStop(1, "#000");
    ctx.beginPath(); ctx.arc(0, 0, ballRadius - 2, 0, Math.PI * 2); ctx.fillStyle = grad; ctx.fill();
    ctx.beginPath(); ctx.arc(-ballRadius/3.5, -ballRadius/3.5, ballRadius/4, 0, Math.PI*2); ctx.fillStyle = "rgba(255,255,255,0.3)"; ctx.fill();
    ctx.restore();
}

function loop() {
    ctx.clearRect(0, 0, width, height);
    ctx.fillStyle = "#020617"; ctx.fillRect(0,0,width,height);
    
    ctx.setLineDash([10, 10]); ctx.strokeStyle = "rgba(255, 30, 86, 0.2)"; ctx.lineWidth = 2;
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
                    if(b.animX) { p.x += b.animX; b.animX *= 0.7; if(Math.abs(b.animX) < 0.1) b.animX = 0; }
                    if(b.animY) { p.y += b.animY; b.animY *= 0.7; if(Math.abs(b.animY) < 0.1) b.animY = 0; }
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
            drawBubble(currentBubble.x, currentBubble.y, currentBubble.color, 1, true);
        } else if (!isProcessing) {
            for (let i = 1; i <= 6; i++) {
                ctx.fillStyle = currentBubble.color; ctx.globalAlpha = 1 - (i/7);
                ctx.beginPath(); ctx.arc(width/2 + Math.cos(angle)*(i*18), (height-80) + Math.sin(angle)*(i*18), 3, 0, Math.PI*2); ctx.fill();
            }
            ctx.globalAlpha = 1;
            drawBubble(currentBubble.x, currentBubble.y, currentBubble.color);
        }
    }

    for (let i = effects.length - 1; i >= 0; i--) {
        let e = effects[i];
        ctx.save(); ctx.globalAlpha = e.life; ctx.shadowBlur = 20; ctx.shadowColor = e.color;
        ctx.beginPath(); ctx.strokeStyle = "rgba(255,255,255, 0.5)"; ctx.lineWidth = 3 * e.life; ctx.arc(e.x, e.y, e.size * 1.5 * (2 - e.life), 0, Math.PI * 2); ctx.stroke();
        ctx.beginPath(); ctx.strokeStyle = e.color; ctx.lineWidth = 1; ctx.arc(e.x, e.y, e.size * 2 * (1.5 - e.life), 0, Math.PI * 2); ctx.stroke();
        ctx.restore();
        e.life *= 0.85; if (e.life < 0.1) effects.splice(i, 1);
    }

    for (let i = particles.length - 1; i >= 0; i--) {
        let p = particles[i]; p.x += p.vx; p.y += p.vy; p.vy += 0.2; p.life -= 0.02;
        if (p.life <= 0) particles.splice(i, 1);
        else { ctx.fillStyle = p.color; ctx.globalAlpha = p.life; ctx.beginPath(); ctx.arc(p.x, p.y, 2, 0, Math.PI*2); ctx.fill(); }
    }
    ctx.globalAlpha = 1;
    animationId = requestAnimationFrame(loop);
}

function checkCollision() {
    let hit = (currentBubble.y < 85);
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
    let finalP = getPos(tr, tc);
    grid[tr][tc] = { color: currentBubble.color, active: true, scale: 1, y_drift: 0, gravity: 0, animX: currentBubble.x - finalP.x, animY: currentBubble.y - finalP.y };
    currentBubble.active = false;
    
    if (checkMatches(tr, tc)) { AudioEngine.play('pop'); missCount = 0; handleFloating(); }
    else { missCount++; if (missCount >= 3) { grid.unshift(new Array(COLS).fill(null).map(() => ({ color: COLORS[Math.floor(Math.random()*COLORS.length)], active: true, scale: 1, y_drift: 0, gravity: 0, animX: 0, animY: -20 }))); grid.pop(); missCount = 0; handleFloating(); } }
    
    let lost = false;
    for(let r=0; r<ROWS; r++) for(let c="0;" c<COLS; c++) if(grid[r][c] && grid[r][c].active grid[r][c].gravity="==" 0 getPos(r,c).y> height * 0.8) lost = true;
    
    if (lost) { document.getElementById('gameover').style.display = 'flex'; isProcessing = true; }
    else { updateUI(); spawnNext(); isProcessing = false; }
}

function checkMatches(r, c) {
    let color = grid[r][c].color, matches = [[r, c]], visited = new Set([r+","+c]), i = 0;
    while(i < matches.length) {
        let [currR, currC] = matches[i++];
        let neighbors = currR % 2 === 0 ? [[0,-1],[0,1],[-1,0],[-1,-1],[1,0],[1,-1]] : [[0,-1],[0,1],[-1,0],[-1,1],[1,0],[1,1]];
        for(let [dr, dc] of neighbors) {
            let nr = currR+dr, nc = currC+dc;
            if(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && grid[nr][nc] grid[nr][nc].active grid[nr][nc].color="==" color !visited.has(nr+","+nc)) { visited.add(nr+","+nc); matches.push([nr, nc]); } if (matches.length>= 3) {
        matches.forEach(([mr, mc]) => {
            let p = getPos(mr, mc);
            for(let k=0; k<6; k++) particles.push({ x: p.x, y: p.y, vx: Math.random()*2-1, vy: Math.random()*2-1, life: 1, color: color });
            burstEffect(p.x, p.y, color);
            grid[mr][mc].active = false;
        });
        score += matches.length * 10; return true;
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
            if(nr>=0 && nr<ROWS && nc>=0 && nc<COLS && grid[nr][nc] grid[nr][nc].active !connected.has(nr+","+nc)) { connected.add(nr+","+nc); queue.push([nr, nc]); } for (let r < ROWS; r++) c COLS; c++) if (grid[r][c] grid[r][c].active !connected.has(r+","+c)) let p="getPos(r,c);" burstEffect(p.x, p.y, grid[r][c].color); grid[r][c].gravity="1;" function updateUI() document.getElementById('score').textContent="score;" document.getElementById('level').textContent="level;" document.getElementById('misses').textContent="missCount;" const handleInput="(e)"> {
    if(isProcessing || !currentBubble) return;
    const rect = canvas.getBoundingClientRect();
    const touch = e.touches ? e.touches[0] : e;
    angle = Math.atan2((touch.clientY - rect.top) - (height - 80), (touch.clientX - rect.left) - (width / 2));
};

const handleFire = () => {
    if (isProcessing || !currentBubble || currentBubble.fired) return;
    AudioEngine.play('shoot');
    currentBubble.vx = Math.cos(angle) * 16; currentBubble.vy = Math.sin(angle) * 16;
    currentBubble.fired = true;
};

document.getElementById('mute-btn').onclick = () => {
    AudioEngine.isMuted = !AudioEngine.isMuted;
    document.getElementById('sound-on').style.display = AudioEngine.isMuted ? 'none' : 'block';
    document.getElementById('sound-off').style.display = AudioEngine.isMuted ? 'block' : 'none';
};

document.getElementById('startBtn').onclick = () => resetGame();
document.getElementById('restartBtn').onclick = () => resetGame();
document.getElementById('nextBtn').onclick = () => nextLevel();

canvas.addEventListener("touchstart", (e) => { e.preventDefault(); handleInput(e); }, {passive: false});
canvas.addEventListener("touchmove", (e) => { e.preventDefault(); handleInput(e); }, {passive: false});
canvas.addEventListener("touchend", (e) => { e.preventDefault(); handleFire(); }, {passive: false});
canvas.addEventListener("mousemove", handleInput);
canvas.addEventListener("mousedown", handleFire);

window.onload = init;
</script>
</body>
</html>
