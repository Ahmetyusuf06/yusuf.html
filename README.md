<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard</title>
<style>
  :root {
    --bg: #f7f5f0;
    --surface: #ffffff;
    --surface2: #f0ede6;
    --border: rgba(0,0,0,0.08);
    --border2: rgba(0,0,0,0.14);
    --text: #1a1a1a;
    --muted: #7a7570;
    --accent: #2d6a4f;
    --accent2: #40916c;
    --accent-light: #d8f3dc;
    --red: #c0392b;
    --amber: #b7600a;
    --blue: #1a5a8a;
    --radius: 12px;
    --radius-sm: 8px;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #111210;
      --surface: #1c1e1b;
      --surface2: #242621;
      --border: rgba(255,255,255,0.07);
      --border2: rgba(255,255,255,0.13);
      --text: #e8e5df;
      --muted: #8a8780;
      --accent: #52b788;
      --accent2: #74c69d;
      --accent-light: rgba(82,183,136,0.15);
      --red: #e05c4c;
      --amber: #e09030;
      --blue: #5294c8;
    }
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Georgia', 'Times New Roman', serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 2rem 1.5rem;
  }
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    margin-bottom: 2rem;
    padding-bottom: 1.5rem;
    border-bottom: 1px solid var(--border);
  }
  .logo { font-size: 1.5rem; font-weight: normal; letter-spacing: -0.02em; color: var(--text); }
  .logo span { color: var(--accent); }
  .clock { font-size: 2rem; font-weight: normal; font-variant-numeric: tabular-nums; color: var(--muted); }
  .date-str { font-size: 0.8rem; color: var(--muted); text-align: right; margin-top: 2px; font-family: 'Courier New', monospace; }

  /* Search */
  .search-wrap {
    position: relative;
    margin-bottom: 2rem;
  }
  .search-input {
    width: 100%;
    padding: 0.9rem 1.2rem 0.9rem 3rem;
    border: 1px solid var(--border2);
    border-radius: var(--radius);
    background: var(--surface);
    font-size: 1rem;
    color: var(--text);
    outline: none;
    font-family: inherit;
    transition: border-color 0.15s;
  }
  .search-input:focus { border-color: var(--accent2); }
  .search-icon {
    position: absolute;
    left: 1rem;
    top: 50%;
    transform: translateY(-50%);
    color: var(--muted);
    font-size: 1rem;
  }
  .search-results {
    display: none;
    position: absolute;
    top: calc(100% + 6px);
    left: 0; right: 0;
    background: var(--surface);
    border: 1px solid var(--border2);
    border-radius: var(--radius-sm);
    z-index: 10;
    box-shadow: 0 8px 24px rgba(0,0,0,0.1);
    overflow: hidden;
  }
  .search-results.open { display: block; }
  .search-result-item {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.75rem 1rem;
    cursor: pointer;
    border-bottom: 1px solid var(--border);
    transition: background 0.1s;
    text-decoration: none;
    color: var(--text);
  }
  .search-result-item:last-child { border-bottom: none; }
  .search-result-item:hover { background: var(--surface2); }
  .result-icon { font-size: 1.1rem; width: 24px; flex-shrink: 0; }
  .result-text { font-size: 0.88rem; }
  .result-sub { font-size: 0.75rem; color: var(--muted); }

  /* Grid */
  .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1rem; }
  .grid-3 { grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); }

  /* Card */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.25rem;
  }
  .card-title {
    font-size: 0.7rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 1rem;
    font-family: 'Courier New', monospace;
  }

  /* Quick Links */
  .links-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0.5rem;
  }
  .link-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    padding: 0.75rem 0.5rem;
    border-radius: var(--radius-sm);
    background: var(--surface2);
    cursor: pointer;
    text-decoration: none;
    color: var(--text);
    transition: background 0.15s, transform 0.1s;
    border: 1px solid transparent;
  }
  .link-item:hover { background: var(--accent-light); border-color: var(--accent2); transform: translateY(-2px); }
  .link-icon { font-size: 1.4rem; }
  .link-label { font-size: 0.7rem; color: var(--muted); text-align: center; }

  /* Notes */
  .note-area {
    width: 100%;
    min-height: 120px;
    background: transparent;
    border: none;
    outline: none;
    font-size: 0.9rem;
    font-family: 'Courier New', monospace;
    color: var(--text);
    resize: none;
    line-height: 1.7;
  }
  .note-saved { font-size: 0.7rem; color: var(--accent); margin-top: 0.5rem; opacity: 0; transition: opacity 0.3s; }
  .note-saved.show { opacity: 1; }

  /* Weather mock */
  .weather-big { font-size: 3rem; font-weight: normal; line-height: 1; margin-bottom: 0.25rem; }
  .weather-desc { font-size: 0.85rem; color: var(--muted); }
  .weather-row { display: flex; gap: 1.5rem; margin-top: 0.75rem; }
  .weather-item { font-size: 0.78rem; color: var(--muted); }
  .weather-item strong { color: var(--text); display: block; font-size: 0.88rem; }

  /* Todo */
  .todo-input-wrap { display: flex; gap: 0.5rem; margin-bottom: 0.75rem; }
  .todo-input {
    flex: 1;
    padding: 0.5rem 0.75rem;
    border: 1px solid var(--border2);
    border-radius: var(--radius-sm);
    background: var(--surface2);
    color: var(--text);
    font-size: 0.85rem;
    outline: none;
    font-family: inherit;
  }
  .btn-sm {
    padding: 0.5rem 0.9rem;
    border: 1px solid var(--accent2);
    border-radius: var(--radius-sm);
    background: transparent;
    color: var(--accent);
    font-size: 0.8rem;
    cursor: pointer;
    transition: background 0.15s;
    font-family: inherit;
  }
  .btn-sm:hover { background: var(--accent-light); }
  .todo-list { list-style: none; }
  .todo-item {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.4rem 0;
    border-bottom: 1px solid var(--border);
    font-size: 0.85rem;
  }
  .todo-item:last-child { border-bottom: none; }
  .todo-check {
    width: 16px; height: 16px;
    border: 1px solid var(--border2);
    border-radius: 4px;
    cursor: pointer;
    flex-shrink: 0;
    appearance: none;
    background: transparent;
    position: relative;
    transition: all 0.15s;
  }
  .todo-check:checked { background: var(--accent); border-color: var(--accent); }
  .todo-check:checked::after { content: '✓'; position: absolute; top: -2px; left: 2px; font-size: 11px; color: white; }
  .todo-text { flex: 1; }
  .todo-text.done { text-decoration: line-through; color: var(--muted); }
  .todo-del { color: var(--muted); cursor: pointer; font-size: 0.85rem; }
  .todo-del:hover { color: var(--red); }

  /* Games section */
  .section-title {
    font-size: 0.7rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
    font-family: 'Courier New', monospace;
    margin: 2rem 0 1rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  .section-title::after { content: ''; flex: 1; height: 1px; background: var(--border); }

  /* Game tabs */
  .game-tabs { display: flex; gap: 0.5rem; margin-bottom: 1rem; }
  .game-tab {
    padding: 0.4rem 1rem;
    border: 1px solid var(--border2);
    border-radius: 20px;
    background: transparent;
    color: var(--muted);
    font-size: 0.78rem;
    cursor: pointer;
    transition: all 0.15s;
    font-family: inherit;
  }
  .game-tab.active { background: var(--accent); border-color: var(--accent); color: white; }
  .game-panel { display: none; }
  .game-panel.active { display: block; }

  /* Snake */
  #snakeCanvas {
    display: block;
    border: 1px solid var(--border2);
    border-radius: var(--radius-sm);
    background: var(--surface2);
    cursor: pointer;
  }
  .game-info { display: flex; justify-content: space-between; align-items: center; margin-bottom: 0.75rem; }
  .score-badge {
    font-size: 0.85rem;
    font-family: 'Courier New', monospace;
    color: var(--accent);
  }
  .game-hint { font-size: 0.72rem; color: var(--muted); }

  /* Memory */
  .memory-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0.5rem;
    max-width: 320px;
  }
  .mem-card {
    aspect-ratio: 1;
    border: 1px solid var(--border2);
    border-radius: var(--radius-sm);
    background: var(--surface2);
    font-size: 1.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: transform 0.2s, background 0.2s;
    user-select: none;
  }
  .mem-card:hover { transform: scale(1.04); }
  .mem-card.flip { background: var(--accent-light); border-color: var(--accent2); }
  .mem-card.matched { background: var(--accent-light); border-color: var(--accent); opacity: 0.6; cursor: default; }
  .mem-card .face { display: none; }
  .mem-card.flip .face, .mem-card.matched .face { display: block; }
  .mem-card .back { font-size: 1.2rem; color: var(--border2); }
  .mem-card.flip .back, .mem-card.matched .back { display: none; }

  /* Word game */
  .word-display {
    font-size: 1.8rem;
    letter-spacing: 0.2em;
    text-align: center;
    font-family: 'Courier New', monospace;
    margin: 1rem 0;
    color: var(--accent);
  }
  .hangman-letters {
    display: flex;
    flex-wrap: wrap;
    gap: 0.4rem;
    margin: 1rem 0;
  }
  .letter-btn {
    width: 32px; height: 32px;
    border: 1px solid var(--border2);
    border-radius: 6px;
    background: var(--surface2);
    color: var(--text);
    font-size: 0.78rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.1s;
    font-family: 'Courier New', monospace;
  }
  .letter-btn:hover:not(:disabled) { background: var(--accent-light); border-color: var(--accent); }
  .letter-btn:disabled { opacity: 0.3; cursor: default; }
  .letter-btn.wrong { background: rgba(192,57,43,0.1); border-color: var(--red); color: var(--red); }
  .hangman-status { text-align: center; font-size: 0.85rem; color: var(--muted); margin-bottom: 0.5rem; }
  .hangman-lives { display: flex; gap: 4px; justify-content: center; margin-bottom: 0.5rem; }
  .life-dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: var(--accent);
    transition: background 0.3s;
  }
  .life-dot.lost { background: var(--red); }

  .btn-restart {
    padding: 0.5rem 1.2rem;
    border: 1px solid var(--accent2);
    border-radius: var(--radius-sm);
    background: transparent;
    color: var(--accent);
    font-size: 0.8rem;
    cursor: pointer;
    font-family: inherit;
    transition: background 0.15s;
  }
  .btn-restart:hover { background: var(--accent-light); }

  @media (max-width: 600px) {
    .links-grid { grid-template-columns: repeat(3, 1fr); }
    .clock { font-size: 1.5rem; }
    body { padding: 1rem; }
  }
</style>
</head>
<body>

<!-- Header -->
<div class="header">
  <div>
    <div class="logo">hub<span>.</span></div>
  </div>
  <div>
    <div class="clock" id="clock">--:--</div>
    <div class="date-str" id="dateStr">—</div>
  </div>
</div>

<!-- Search -->
<div class="search-wrap" id="searchWrap">
  <span class="search-icon">⌕</span>
  <input class="search-input" type="text" placeholder="Ara — site, araç, bilgi..." id="searchInput" autocomplete="off" />
  <div class="search-results" id="searchResults"></div>
</div>

<!-- Grid: Quick Links + Weather + Notes -->
<div class="grid">

  <!-- Quick Links -->
  <div class="card">
    <div class="card-title">Hızlı erişim</div>
    <div class="links-grid" id="linksGrid">
      <a class="link-item" href="https://github.com" target="_blank"><span class="link-icon">⌥</span><span class="link-label">GitHub</span></a>
      <a class="link-item" href="https://mail.google.com" target="_blank"><span class="link-icon">✉</span><span class="link-label">Mail</span></a>
      <a class="link-item" href="https://translate.google.com" target="_blank"><span class="link-icon">⌂</span><span class="link-label">Çeviri</span></a>
      <a class="link-item" href="https://calendar.google.com" target="_blank"><span class="link-icon">◫</span><span class="link-label">Takvim</span></a>
      <a class="link-item" href="https://youtube.com" target="_blank"><span class="link-icon">▷</span><span class="link-label">YouTube</span></a>
      <a class="link-item" href="https://wikipedia.org" target="_blank"><span class="link-icon">⊛</span><span class="link-label">Wiki</span></a>
      <a class="link-item" href="https://maps.google.com" target="_blank"><span class="link-icon">◉</span><span class="link-label">Harita</span></a>
      <a class="link-item" href="https://claude.ai" target="_blank"><span class="link-icon">◈</span><span class="link-label">Claude</span></a>
    </div>
  </div>

  <!-- Notes -->
  <div class="card">
    <div class="card-title">Hızlı not</div>
    <textarea class="note-area" id="noteArea" placeholder="Bir şeyler yaz..."></textarea>
    <div class="note-saved" id="noteSaved">kaydedildi</div>
  </div>

  <!-- Todo -->
  <div class="card">
    <div class="card-title">Yapılacaklar</div>
    <div class="todo-input-wrap">
      <input class="todo-input" id="todoInput" placeholder="Yeni görev..." type="text" />
      <button class="btn-sm" onclick="addTodo()">+</button>
    </div>
    <ul class="todo-list" id="todoList"></ul>
  </div>

</div>

<!-- Games Section -->
<div class="section-title">Oyunlar</div>

<div class="card">
  <div class="game-tabs">
    <button class="game-tab active" onclick="switchGame('snake', this)">Yılan</button>
    <button class="game-tab" onclick="switchGame('memory', this)">Hafıza</button>
    <button class="game-tab" onclick="switchGame('word', this)">Kelime</button>
  </div>

  <!-- Snake Game -->
  <div class="game-panel active" id="game-snake">
    <div class="game-info">
      <span class="score-badge">SKOR: <span id="snakeScore">0</span></span>
      <span class="game-hint">Tıkla veya yön tuşları ile başla</span>
    </div>
    <canvas id="snakeCanvas" width="280" height="280"></canvas>
  </div>

  <!-- Memory Game -->
  <div class="game-panel" id="game-memory">
    <div class="game-info">
      <span class="score-badge">HAMLE: <span id="memMoves">0</span></span>
      <button class="btn-restart" onclick="initMemory()">↺ Yenile</button>
    </div>
    <div class="memory-grid" id="memoryGrid"></div>
  </div>

  <!-- Word / Hangman -->
  <div class="game-panel" id="game-word">
    <div class="hangman-status" id="hangStatus">Kelimeyi bul!</div>
    <div class="hangman-lives" id="hangLives"></div>
    <div class="word-display" id="wordDisplay"></div>
    <div class="hangman-letters" id="hangLetters"></div>
    <div style="text-align:center; margin-top:0.5rem;">
      <button class="btn-restart" onclick="initHangman()">↺ Yeni kelime</button>
    </div>
  </div>
</div>

<script>
// ── Clock ──────────────────────────────────────────
function updateClock() {
  const now = new Date();
  const h = String(now.getHours()).padStart(2,'0');
  const m = String(now.getMinutes()).padStart(2,'0');
  document.getElementById('clock').textContent = h + ':' + m;
  const days = ['Pazar','Pazartesi','Salı','Çarşamba','Perşembe','Cuma','Cumartesi'];
  const months = ['Ocak','Şubat','Mart','Nisan','Mayıs','Haziran','Temmuz','Ağustos','Eylül','Ekim','Kasım','Aralık'];
  document.getElementById('dateStr').textContent = days[now.getDay()] + ', ' + now.getDate() + ' ' + months[now.getMonth()];
}
updateClock();
setInterval(updateClock, 10000);

// ── Search ─────────────────────────────────────────
const SEARCH_ITEMS = [
  { icon: '⌥', label: 'GitHub', sub: 'github.com', url: 'https://github.com' },
  { icon: '◈', label: 'Claude AI', sub: 'claude.ai', url: 'https://claude.ai' },
  { icon: '⊛', label: 'Wikipedia', sub: 'wikipedia.org', url: 'https://wikipedia.org' },
  { icon: '✉', label: 'Gmail', sub: 'mail.google.com', url: 'https://mail.google.com' },
  { icon: '▷', label: 'YouTube', sub: 'youtube.com', url: 'https://youtube.com' },
  { icon: '◉', label: 'Google Maps', sub: 'maps.google.com', url: 'https://maps.google.com' },
  { icon: '⌂', label: 'Google Çeviri', sub: 'translate.google.com', url: 'https://translate.google.com' },
  { icon: '◫', label: 'Google Takvim', sub: 'calendar.google.com', url: 'https://calendar.google.com' },
  { icon: '⌕', label: 'Google Arama', sub: 'google.com', url: 'https://google.com' },
  { icon: '⊞', label: 'Stack Overflow', sub: 'stackoverflow.com', url: 'https://stackoverflow.com' },
  { icon: '⊡', label: 'MDN Docs', sub: 'developer.mozilla.org', url: 'https://developer.mozilla.org' },
  { icon: '⊟', label: 'npm', sub: 'npmjs.com', url: 'https://npmjs.com' },
];

const searchInput = document.getElementById('searchInput');
const searchResults = document.getElementById('searchResults');

searchInput.addEventListener('input', function() {
  const q = this.value.trim().toLowerCase();
  if (!q) { searchResults.classList.remove('open'); return; }
  const matches = SEARCH_ITEMS.filter(i => i.label.toLowerCase().includes(q) || i.sub.toLowerCase().includes(q));
  const googleItem = { icon: '⌕', label: 'Google\'da ara: ' + this.value, sub: 'google.com/search', url: 'https://www.google.com/search?q=' + encodeURIComponent(this.value) };
  const items = [...matches, googleItem].slice(0, 6);
  searchResults.innerHTML = items.map(i =>
    `<a class="search-result-item" href="${i.url}" target="_blank">
      <span class="result-icon">${i.icon}</span>
      <div><div class="result-text">${i.label}</div><div class="result-sub">${i.sub}</div></div>
    </a>`
  ).join('');
  searchResults.classList.add('open');
});

document.addEventListener('click', function(e) {
  if (!document.getElementById('searchWrap').contains(e.target)) {
    searchResults.classList.remove('open');
  }
});

searchInput.addEventListener('keydown', function(e) {
  if (e.key === 'Enter' && this.value) {
    window.open('https://www.google.com/search?q=' + encodeURIComponent(this.value), '_blank');
    searchResults.classList.remove('open');
  }
});

// ── Notes ──────────────────────────────────────────
const noteArea = document.getElementById('noteArea');
noteArea.value = localStorage.getItem('hub_note') || '';
let noteSaveTimer;
noteArea.addEventListener('input', function() {
  clearTimeout(noteSaveTimer);
  noteSaveTimer = setTimeout(() => {
    localStorage.setItem('hub_note', noteArea.value);
    const el = document.getElementById('noteSaved');
    el.classList.add('show');
    setTimeout(() => el.classList.remove('show'), 1500);
  }, 800);
});

// ── Todo ───────────────────────────────────────────
let todos = JSON.parse(localStorage.getItem('hub_todos') || '[]');
function saveTodos() { localStorage.setItem('hub_todos', JSON.stringify(todos)); }
function renderTodos() {
  const list = document.getElementById('todoList');
  list.innerHTML = todos.map((t, i) => `
    <li class="todo-item">
      <input type="checkbox" class="todo-check" ${t.done ? 'checked' : ''} onchange="toggleTodo(${i})" />
      <span class="todo-text${t.done ? ' done' : ''}">${t.text}</span>
      <span class="todo-del" onclick="deleteTodo(${i})">✕</span>
    </li>`).join('');
}
function addTodo() {
  const input = document.getElementById('todoInput');
  const text = input.value.trim();
  if (!text) return;
  todos.unshift({ text, done: false });
  saveTodos(); renderTodos();
  input.value = '';
}
function toggleTodo(i) { todos[i].done = !todos[i].done; saveTodos(); renderTodos(); }
function deleteTodo(i) { todos.splice(i, 1); saveTodos(); renderTodos(); }
document.getElementById('todoInput').addEventListener('keydown', e => { if (e.key === 'Enter') addTodo(); });
renderTodos();

// ── Game Switch ────────────────────────────────────
function switchGame(name, btn) {
  document.querySelectorAll('.game-panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.game-tab').forEach(b => b.classList.remove('active'));
  document.getElementById('game-' + name).classList.add('active');
  btn.classList.add('active');
  if (name === 'snake') initSnake();
  if (name === 'memory') initMemory();
  if (name === 'word') initHangman();
}

// ── Snake Game ─────────────────────────────────────
const canvas = document.getElementById('snakeCanvas');
const ctx = canvas.getContext('2d');
const CELL = 20;
const COLS = canvas.width / CELL;
const ROWS = canvas.height / CELL;
let snake, dir, food, snakeScore, snakeRunning, snakeInterval;

function initSnake() {
  snake = [{x:6,y:10},{x:5,y:10},{x:4,y:10}];
  dir = {x:1,y:0};
  snakeScore = 0;
  snakeRunning = false;
  document.getElementById('snakeScore').textContent = '0';
  placeFood();
  drawSnake();
}

function placeFood() {
  do {
    food = {x: Math.floor(Math.random()*COLS), y: Math.floor(Math.random()*ROWS)};
  } while (snake.some(s => s.x===food.x && s.y===food.y));
}

function drawSnake() {
  const isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  ctx.fillStyle = isDark ? '#1c1e1b' : '#f0ede6';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  // food
  ctx.fillStyle = isDark ? '#52b788' : '#2d6a4f';
  ctx.beginPath();
  ctx.arc(food.x*CELL+CELL/2, food.y*CELL+CELL/2, CELL/2-2, 0, Math.PI*2);
  ctx.fill();

  // snake
  snake.forEach((s, i) => {
    const alpha = 1 - (i / snake.length) * 0.5;
    ctx.fillStyle = isDark ? `rgba(82,183,136,${alpha})` : `rgba(45,106,79,${alpha})`;
    ctx.beginPath();
    ctx.roundRect(s.x*CELL+1, s.y*CELL+1, CELL-2, CELL-2, 4);
    ctx.fill();
  });

  if (!snakeRunning) {
    ctx.fillStyle = isDark ? 'rgba(28,30,27,0.7)' : 'rgba(240,237,230,0.75)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = isDark ? '#52b788' : '#2d6a4f';
    ctx.font = '14px Courier New';
    ctx.textAlign = 'center';
    ctx.fillText('Başlamak için tıkla', canvas.width/2, canvas.height/2);
  }
}

function snakeStep() {
  const head = {x: snake[0].x + dir.x, y: snake[0].y + dir.y};
  if (head.x<0||head.x>=COLS||head.y<0||head.y>=ROWS||snake.some(s=>s.x===head.x&&s.y===head.y)) {
    clearInterval(snakeInterval);
    snakeRunning = false;
    ctx.fillStyle = 'rgba(192,57,43,0.15)';
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ctx.fillStyle = '#c0392b';
    ctx.font = '14px Courier New';
    ctx.textAlign = 'center';
    ctx.fillText('Oyun bitti! Tıkla →', canvas.width/2, canvas.height/2);
    return;
  }
  snake.unshift(head);
  if (head.x===food.x && head.y===food.y) {
    snakeScore++;
    document.getElementById('snakeScore').textContent = snakeScore;
    placeFood();
  } else {
    snake.pop();
  }
  drawSnake();
}

canvas.addEventListener('click', function() {
  if (snakeRunning) return;
  initSnake();
  snakeRunning = true;
  clearInterval(snakeInterval);
  snakeInterval = setInterval(snakeStep, 130);
});

document.addEventListener('keydown', function(e) {
  const panel = document.getElementById('game-snake');
  if (!panel.classList.contains('active')) return;
  const map = { ArrowUp:{x:0,y:-1}, ArrowDown:{x:0,y:1}, ArrowLeft:{x:-1,y:0}, ArrowRight:{x:1,y:0} };
  if (map[e.key]) {
    e.preventDefault();
    const nd = map[e.key];
    if (nd.x !== -dir.x || nd.y !== -dir.y) dir = nd;
    if (!snakeRunning) {
      snakeRunning = true;
      clearInterval(snakeInterval);
      snakeInterval = setInterval(snakeStep, 130);
    }
  }
});

initSnake();

// ── Memory Game ────────────────────────────────────
const EMOJIS = ['🍎','🍋','🍇','🍓','🌸','🎯','🦋','🌙'];
let memCards, memFlipped, memMatched, memMoves, memLock;

function initMemory() {
  const pairs = [...EMOJIS, ...EMOJIS].sort(() => Math.random()-0.5);
  memFlipped = []; memMatched = 0; memMoves = 0; memLock = false;
  document.getElementById('memMoves').textContent = '0';
  const grid = document.getElementById('memoryGrid');
  grid.innerHTML = '';
  memCards = pairs.map((emoji, i) => {
    const card = document.createElement('div');
    card.className = 'mem-card';
    card.innerHTML = `<span class="back">◈</span><span class="face">${emoji}</span>`;
    card.dataset.emoji = emoji;
    card.dataset.idx = i;
    card.addEventListener('click', () => flipCard(card));
    grid.appendChild(card);
    return card;
  });
}

function flipCard(card) {
  if (memLock || card.classList.contains('flip') || card.classList.contains('matched')) return;
  card.classList.add('flip');
  memFlipped.push(card);
  if (memFlipped.length === 2) {
    memLock = true;
    memMoves++;
    document.getElementById('memMoves').textContent = memMoves;
    const [a, b] = memFlipped;
    if (a.dataset.emoji === b.dataset.emoji) {
      a.classList.add('matched'); b.classList.add('matched');
      a.classList.remove('flip'); b.classList.remove('flip');
      memMatched++;
      memFlipped = []; memLock = false;
      if (memMatched === EMOJIS.length) {
        setTimeout(() => alert('Tebrikler! ' + memMoves + ' hamlede tamamladın 🎉'), 200);
      }
    } else {
      setTimeout(() => {
        a.classList.remove('flip'); b.classList.remove('flip');
        memFlipped = []; memLock = false;
      }, 800);
    }
  }
}

initMemory();

// ── Hangman ────────────────────────────────────────
const WORDS = [
  {w:'JAVASCRIPT', hint:'Programlama dili'},
  {w:'ISTANBUL', hint:'Şehir'},
  {w:'BILGISAYAR', hint:'Teknoloji'},
  {w:'UNIVERSITE', hint:'Kurum'},
  {w:'KLAVYE', hint:'Donanım'},
  {w:'ALGORITMA', hint:'Kavram'},
  {w:'YAZILIM', hint:'Teknoloji'},
  {w:'VERITABANI', hint:'Sistem'},
  {w:'ARAYUZ', hint:'Tasarım'},
  {w:'DONUSUM', hint:'Kavram'},
];
let hangWord, hangGuessed, hangWrong;

function initHangman() {
  const item = WORDS[Math.floor(Math.random() * WORDS.length)];
  hangWord = item.w;
  hangGuessed = new Set();
  hangWrong = 0;
  document.getElementById('hangStatus').textContent = 'İpucu: ' + item.hint;
  renderHangman();
}

function renderHangman() {
  const MAX = 6;
  const display = hangWord.split('').map(l => hangGuessed.has(l) ? l : '_').join(' ');
  document.getElementById('wordDisplay').textContent = display;

  const livesEl = document.getElementById('hangLives');
  livesEl.innerHTML = Array.from({length:MAX}, (_,i) =>
    `<div class="life-dot${i < hangWrong ? ' lost' : ''}"></div>`
  ).join('');

  const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZİÇŞĞÜÖ'.split('');
  const container = document.getElementById('hangLetters');
  container.innerHTML = letters.map(l => {
    const used = hangGuessed.has(l);
    const wrong = used && !hangWord.includes(l);
    return `<button class="letter-btn${wrong ? ' wrong' : ''}" ${used ? 'disabled' : ''} onclick="guessLetter('${l}')">${l}</button>`;
  }).join('');

  const won = hangWord.split('').every(l => hangGuessed.has(l));
  if (won) {
    document.getElementById('hangStatus').textContent = '🎉 Doğru! Tebrikler!';
  } else if (hangWrong >= MAX) {
    document.getElementById('wordDisplay').textContent = hangWord.split('').join(' ');
    document.getElementById('hangStatus').textContent = '😔 Kelime: ' + hangWord;
  }
}

function guessLetter(l) {
  if (hangGuessed.has(l)) return;
  hangGuessed.add(l);
  if (!hangWord.includes(l)) hangWrong++;
  renderHangman();
}

initHangman();
</script>
</body>
</html>
