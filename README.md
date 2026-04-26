<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WordWise — Learn English with Riddles</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --cream: #FDF6EC;
    --gold: #C8963E;
    --gold-light: #F0C97A;
    --dark: #1A1508;
    --dark2: #2E2410;
    --green: #2D6A4F;
    --green-light: #74C69D;
    --red: #C0392B;
    --blue: #2472A4;
    --muted: #8A7A5A;
    --card-bg: #FFFDF7;
    --border: #E8D9BC;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--cream);
    color: var(--dark);
    min-height: 100vh;
  }

  /* HEADER */
  header {
    background: var(--dark);
    padding: 20px 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 3px solid var(--gold);
  }
  .logo {
    font-family: 'Playfair Display', serif;
    font-size: 28px;
    color: var(--gold);
    letter-spacing: -0.5px;
  }
  .logo span { color: #fff; }
  .score-bar {
    display: flex;
    align-items: center;
    gap: 20px;
    color: #fff;
    font-size: 14px;
  }
  .score-val {
    font-size: 22px;
    font-weight: 500;
    color: var(--gold-light);
    font-family: 'Playfair Display', serif;
  }
  .level-badge {
    background: var(--gold);
    color: var(--dark);
    font-size: 12px;
    font-weight: 500;
    padding: 4px 14px;
    border-radius: 20px;
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  /* NAV TABS */
  .tabs {
    display: flex;
    background: var(--dark2);
    padding: 0 40px;
    gap: 4px;
  }
  .tab {
    padding: 14px 22px;
    font-size: 14px;
    font-weight: 500;
    color: var(--muted);
    cursor: pointer;
    border-bottom: 3px solid transparent;
    transition: all 0.2s;
    user-select: none;
  }
  .tab:hover { color: #fff; }
  .tab.active { color: var(--gold-light); border-bottom-color: var(--gold); }

  /* MAIN LAYOUT */
  main { max-width: 900px; margin: 0 auto; padding: 40px 20px; }

  /* SECTION TITLE */
  .section-label {
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 6px;
  }
  .section-title {
    font-family: 'Playfair Display', serif;
    font-size: 34px;
    font-weight: 900;
    color: var(--dark);
    margin-bottom: 28px;
    line-height: 1.1;
  }
  .section-title em { color: var(--gold); font-style: italic; }

  /* RIDDLE CARD */
  .riddle-card {
    background: var(--card-bg);
    border: 1.5px solid var(--border);
    border-radius: 18px;
    padding: 36px 40px;
    margin-bottom: 28px;
    position: relative;
    overflow: hidden;
  }
  .riddle-card::before {
    content: '"';
    position: absolute;
    top: -10px;
    left: 24px;
    font-size: 120px;
    font-family: 'Playfair Display', serif;
    color: var(--gold-light);
    opacity: 0.3;
    line-height: 1;
  }
  .riddle-number {
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 10px;
  }
  .riddle-category {
    display: inline-block;
    background: var(--gold-light);
    color: var(--dark2);
    font-size: 11px;
    font-weight: 500;
    padding: 3px 12px;
    border-radius: 12px;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 18px;
  }
  .riddle-text {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--dark);
    line-height: 1.5;
    margin-bottom: 24px;
    position: relative;
    z-index: 1;
  }
  .riddle-hint {
    font-size: 13px;
    color: var(--muted);
    margin-bottom: 24px;
    padding: 10px 16px;
    background: #F5EDD8;
    border-radius: 8px;
    border-left: 3px solid var(--gold);
    display: none;
  }
  .riddle-hint.shown { display: block; }

  /* ANSWER INPUT */
  .answer-row {
    display: flex;
    gap: 12px;
    align-items: center;
    flex-wrap: wrap;
  }
  .answer-input {
    flex: 1;
    min-width: 200px;
    padding: 12px 20px;
    font-size: 16px;
    font-family: 'DM Sans', sans-serif;
    border: 2px solid var(--border);
    border-radius: 10px;
    background: #fff;
    color: var(--dark);
    outline: none;
    transition: border-color 0.2s;
  }
  .answer-input:focus { border-color: var(--gold); }
  .answer-input.correct { border-color: var(--green); background: #EAFAF1; }
  .answer-input.wrong { border-color: var(--red); background: #FDECEA; animation: shake 0.3s; }
  @keyframes shake {
    0%,100%{transform:translateX(0)}25%{transform:translateX(-6px)}75%{transform:translateX(6px)}
  }

  .btn {
    padding: 12px 24px;
    border-radius: 10px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    border: none;
    transition: all 0.18s;
  }
  .btn-primary {
    background: var(--gold);
    color: var(--dark);
  }
  .btn-primary:hover { background: var(--gold-light); }
  .btn-hint {
    background: transparent;
    border: 1.5px solid var(--border);
    color: var(--muted);
  }
  .btn-hint:hover { border-color: var(--gold); color: var(--gold); }

  /* FEEDBACK */
  .feedback {
    margin-top: 16px;
    font-size: 15px;
    font-weight: 500;
    display: none;
    align-items: center;
    gap: 8px;
  }
  .feedback.show { display: flex; }
  .feedback.correct-fb { color: var(--green); }
  .feedback.wrong-fb { color: var(--red); }
  .feedback-icon {
    width: 22px; height: 22px;
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 12px;
    flex-shrink: 0;
  }
  .feedback.correct-fb .feedback-icon { background: var(--green); color: #fff; }
  .feedback.wrong-fb .feedback-icon { background: var(--red); color: #fff; }
  .translation {
    font-size: 13px;
    color: var(--muted);
    margin-top: 6px;
    font-style: italic;
  }

  /* WORD MATCH GAME */
  .word-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 12px;
    margin-bottom: 24px;
  }
  .word-card {
    background: var(--card-bg);
    border: 2px solid var(--border);
    border-radius: 12px;
    padding: 14px 10px;
    text-align: center;
    cursor: pointer;
    transition: all 0.18s;
    user-select: none;
    font-size: 15px;
    font-weight: 500;
  }
  .word-card:hover { border-color: var(--gold); }
  .word-card.selected { border-color: var(--gold); background: #FFF8E8; color: var(--gold); }
  .word-card.matched { border-color: var(--green); background: #EAFAF1; color: var(--green); pointer-events: none; }
  .word-card.wrong-match { border-color: var(--red); background: #FDECEA; animation: shake 0.3s; }

  /* PROGRESS */
  .progress-row {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 32px;
  }
  .progress-track {
    flex: 1;
    height: 6px;
    background: var(--border);
    border-radius: 3px;
    overflow: hidden;
  }
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--gold), var(--gold-light));
    border-radius: 3px;
    transition: width 0.4s ease;
  }
  .progress-text { font-size: 13px; color: var(--muted); white-space: nowrap; }

  /* FILL IN BLANK */
  .fill-sentence {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--dark);
    line-height: 1.7;
    margin-bottom: 20px;
  }
  .blank-input {
    display: inline-block;
    border: none;
    border-bottom: 3px solid var(--gold);
    background: transparent;
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--blue);
    width: 140px;
    text-align: center;
    outline: none;
    padding: 0 4px;
  }
  .blank-input.correct-blank { border-bottom-color: var(--green); color: var(--green); }
  .blank-input.wrong-blank { border-bottom-color: var(--red); color: var(--red); animation: shake 0.3s; }
  .word-options {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 20px;
  }
  .word-option {
    padding: 8px 18px;
    border: 1.5px solid var(--border);
    border-radius: 8px;
    font-size: 15px;
    background: var(--card-bg);
    cursor: pointer;
    transition: all 0.18s;
    font-weight: 500;
  }
  .word-option:hover { border-color: var(--gold); background: #FFF8E8; }
  .word-option.used { opacity: 0.35; pointer-events: none; text-decoration: line-through; }

  /* FOOTER */
  .section-footer {
    text-align: center;
    color: var(--muted);
    font-size: 13px;
    margin-top: 40px;
    padding-top: 24px;
    border-top: 1px solid var(--border);
  }

  /* HIDDEN */
  .page { display: none; }
  .page.active { display: block; }

  /* NEXT BTN */
  .next-btn-row { text-align: right; margin-top: 12px; }
  .btn-next {
    background: var(--dark);
    color: var(--gold-light);
    padding: 11px 28px;
    border-radius: 10px;
    font-size: 14px;
    font-weight: 500;
    border: none;
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    transition: background 0.18s;
  }
  .btn-next:hover { background: var(--dark2); }

  /* CONGRATS */
  .congrats-box {
    background: var(--card-bg);
    border: 2px solid var(--gold);
    border-radius: 18px;
    padding: 48px 40px;
    text-align: center;
    display: none;
  }
  .congrats-box.show { display: block; }
  .congrats-icon { font-size: 56px; margin-bottom: 16px; }
  .congrats-title {
    font-family: 'Playfair Display', serif;
    font-size: 30px;
    font-weight: 900;
    color: var(--gold);
    margin-bottom: 10px;
  }
  .congrats-sub { font-size: 16px; color: var(--muted); margin-bottom: 28px; }
</style>
</head>
<body>

<header>
  <div class="logo">Word<span>Wise</span></div>
  <div class="score-bar">
    <div>
      <div style="font-size:11px;letter-spacing:1px;text-transform:uppercase;color:#888;margin-bottom:2px">Score</div>
      <div class="score-val" id="score-display">0</div>
    </div>
    <div class="level-badge" id="level-display">Beginner</div>
  </div>
</header>

<div class="tabs">
  <div class="tab active" onclick="switchTab('riddles')">🧩 Riddles</div>
  <div class="tab" onclick="switchTab('match')">🔗 Word Match</div>
  <div class="tab" onclick="switchTab('fill')">✏️ Fill in the Blank</div>
</div>

<main>

  <!-- ===== RIDDLES PAGE ===== -->
  <div class="page active" id="page-riddles">
    <div class="progress-row">
      <div class="progress-track"><div class="progress-fill" id="riddle-progress" style="width:0%"></div></div>
      <div class="progress-text" id="riddle-progress-text">0 / 6</div>
    </div>
    <div class="section-label">English Riddles</div>
    <div class="section-title">Think & <em>Guess</em> the Word</div>

    <div id="riddle-container"></div>
    <div class="congrats-box" id="riddle-congrats">
      <div class="congrats-icon">🎉</div>
      <div class="congrats-title">Brilliant!</div>
      <div class="congrats-sub">You solved all riddles. Try the Word Match next!</div>
      <button class="btn btn-primary" onclick="switchTab('match')">Go to Word Match →</button>
    </div>
  </div>

  <!-- ===== WORD MATCH PAGE ===== -->
  <div class="page" id="page-match">
    <div class="section-label">Word Match</div>
    <div class="section-title">Match English — <em>Turkish</em></div>
    <div id="match-container"></div>
    <div class="congrats-box" id="match-congrats">
      <div class="congrats-icon">⭐</div>
      <div class="congrats-title">Perfect Match!</div>
      <div class="congrats-sub">All pairs matched correctly!</div>
      <button class="btn btn-primary" onclick="switchTab('fill')">Try Fill in the Blank →</button>
    </div>
  </div>

  <!-- ===== FILL IN BLANK PAGE ===== -->
  <div class="page" id="page-fill">
    <div class="progress-row">
      <div class="progress-track"><div class="progress-fill" id="fill-progress" style="width:0%"></div></div>
      <div class="progress-text" id="fill-progress-text">0 / 5</div>
    </div>
    <div class="section-label">Fill in the Blank</div>
    <div class="section-title">Complete the <em>Sentence</em></div>
    <div id="fill-container"></div>
    <div class="congrats-box" id="fill-congrats">
      <div class="congrats-icon">🏆</div>
      <div class="congrats-title">You're amazing!</div>
      <div class="congrats-sub">All sentences completed. Keep practising!</div>
      <button class="btn btn-primary" onclick="restartAll()">Start Over 🔄</button>
    </div>
  </div>

  <div class="section-footer">WordWise · Learn English through Play · 🇬🇧</div>
</main>

<script>
let totalScore = 0;

function addScore(n) {
  totalScore += n;
  document.getElementById('score-display').textContent = totalScore;
  const lvls = [[0,'Beginner'],[50,'Elementary'],[120,'Intermediate'],[200,'Advanced']];
  let lv = 'Beginner';
  for (const [t,l] of lvls) { if (totalScore >= t) lv = l; }
  document.getElementById('level-display').textContent = lv;
}

function switchTab(t) {
  document.querySelectorAll('.tab').forEach((el,i)=>{ el.classList.toggle('active', ['riddles','match','fill'][i]===t); });
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page-'+t).classList.add('active');
}

/* ===== RIDDLES ===== */
const riddles = [
  { q:"I have hands but I cannot clap.\nI have a face but I cannot smile.\nWhat am I?", a:"clock", hint:"You look at me to know the time.", tr:"Ellerin var ama alkış tutamam, yüzüm var ama gülümseyemem. Neyim?", category:"Objects", translation:"Saat" },
  { q:"The more you take, the more you leave behind.\nWhat am I?", a:"footsteps", hint:"Think about walking on sand.", tr:"Ne kadar çok alırsan, o kadar çok geride bırakırsın.", category:"Nature", translation:"Ayak izleri" },
  { q:"I speak without a mouth and hear without ears.\nI have no body, but I come alive with wind.\nWhat am I?", a:"echo", hint:"Shout in a mountain and you'll hear me.", tr:"Ağzım yok konuşurum, kulağım yok duyarım.", category:"Nature", translation:"Yankı" },
  { q:"I am always in front of you but can't be seen.\nWhat am I?", a:"future", hint:"Yesterday is past, today is present, tomorrow is…", tr:"Her zaman önünde ama hiç göremezsin beni. Neyim?", category:"Abstract", translation:"Gelecek" },
  { q:"The more you have of it, the less you see.\nWhat am I?", a:"darkness", hint:"Turn off the lights at night.", tr:"Ne kadar çok olursa, o kadar az görürsün. Neyim?", category:"Nature", translation:"Karanlık" },
  { q:"I have cities, but no houses live there.\nI have mountains, but no trees grow there.\nI have water, but no fish swim.\nWhat am I?", a:"map", hint:"Explorers carry me on adventures.", tr:"Şehirlerim var ama ev yok, dağlarım var ama ağaç yok. Neyim?", category:"Objects", translation:"Harita" },
];

let riddleDone = 0;

function buildRiddles() {
  const c = document.getElementById('riddle-container');
  c.innerHTML = '';
  riddles.forEach((r, i) => {
    c.innerHTML += `
    <div class="riddle-card" id="rcard-${i}">
      <div class="riddle-number">Riddle ${i+1} of ${riddles.length}</div>
      <div class="riddle-category">${r.category}</div>
      <div class="riddle-text">${r.q.replace(/\n/g,'<br>')}</div>
      <div class="riddle-hint" id="rhint-${i}">💡 Hint: ${r.hint}<br><span style="font-size:12px;opacity:.7">Türkçe: ${r.tr}</span></div>
      <div class="answer-row">
        <input class="answer-input" id="rinput-${i}" placeholder="Type your answer..." onkeydown="if(event.key==='Enter')checkRiddle(${i})">
        <button class="btn btn-hint" onclick="showHint(${i})">Hint 💡</button>
        <button class="btn btn-primary" onclick="checkRiddle(${i})">Check ✓</button>
      </div>
      <div class="feedback" id="rfb-${i}">
        <div class="feedback-icon" id="rfbicon-${i}"></div>
        <div>
          <div id="rfbtext-${i}"></div>
          <div class="translation" id="rfbtr-${i}"></div>
        </div>
      </div>
    </div>`;
  });
  updateRiddleProgress();
}

function showHint(i) {
  document.getElementById('rhint-'+i).classList.add('shown');
}

function checkRiddle(i) {
  const inp = document.getElementById('rinput-'+i);
  const fb = document.getElementById('rfb-'+i);
  const val = inp.value.trim().toLowerCase();
  const correct = riddles[i].a.toLowerCase();
  const fbText = document.getElementById('rfbtext-'+i);
  const fbIcon = document.getElementById('rfbicon-'+i);
  const fbTr = document.getElementById('rfbtr-'+i);

  if (!val) return;

  if (val === correct || correct.split(' ').includes(val)) {
    inp.classList.add('correct');
    inp.disabled = true;
    fb.classList.add('show','correct-fb');
    fbIcon.textContent = '✓';
    fbText.textContent = 'Correct! The answer is "' + riddles[i].a + '"';
    fbTr.textContent = '🇹🇷 Türkçe: ' + riddles[i].translation;
    addScore(15);
    riddleDone++;
    updateRiddleProgress();
    if (riddleDone === riddles.length) {
      setTimeout(() => document.getElementById('riddle-congrats').classList.add('show'), 600);
    }
  } else {
    inp.classList.remove('wrong');
    void inp.offsetWidth;
    inp.classList.add('wrong');
    fb.classList.add('show','wrong-fb');
    fb.classList.remove('correct-fb');
    fbIcon.textContent = '✗';
    fbText.textContent = 'Not quite! Try again.';
    fbTr.textContent = '';
    setTimeout(() => { inp.classList.remove('wrong'); fb.classList.remove('show','wrong-fb'); }, 1200);
  }
}

function updateRiddleProgress() {
  const pct = (riddleDone / riddles.length) * 100;
  document.getElementById('riddle-progress').style.width = pct + '%';
  document.getElementById('riddle-progress-text').textContent = riddleDone + ' / ' + riddles.length;
}

/* ===== WORD MATCH ===== */
const matchData = [
  {en:'Apple', tr:'Elma'}, {en:'Sun', tr:'Güneş'}, {en:'Book', tr:'Kitap'},
  {en:'Water', tr:'Su'}, {en:'House', tr:'Ev'}, {en:'Tree', tr:'Ağaç'},
  {en:'Cat', tr:'Kedi'}, {en:'Happy', tr:'Mutlu'}
];
let matchSelected = null;
let matchedCount = 0;

function buildMatch() {
  const left = [...matchData].sort(()=>Math.random()-0.5);
  const right = [...matchData].sort(()=>Math.random()-0.5);
  const c = document.getElementById('match-container');
  c.innerHTML = `
    <p style="color:var(--muted);font-size:14px;margin-bottom:20px">Select an English word, then its Turkish meaning.</p>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px">
      <div>
        <div style="font-size:11px;letter-spacing:1.5px;text-transform:uppercase;color:var(--muted);margin-bottom:10px">English 🇬🇧</div>
        <div id="match-left" class="word-grid" style="grid-template-columns:1fr"></div>
      </div>
      <div>
        <div style="font-size:11px;letter-spacing:1.5px;text-transform:uppercase;color:var(--muted);margin-bottom:10px">Turkish 🇹🇷</div>
        <div id="match-right" class="word-grid" style="grid-template-columns:1fr"></div>
      </div>
    </div>
    <div id="match-fb" style="margin-top:14px;font-size:14px;font-weight:500;color:var(--muted)"></div>`;

  left.forEach(w => {
    const el = document.createElement('div');
    el.className = 'word-card';
    el.textContent = w.en;
    el.dataset.word = w.en;
    el.dataset.side = 'left';
    el.onclick = () => selectMatchCard(el);
    document.getElementById('match-left').appendChild(el);
  });
  right.forEach(w => {
    const el = document.createElement('div');
    el.className = 'word-card';
    el.textContent = w.tr;
    el.dataset.word = w.tr;
    el.dataset.side = 'right';
    el.onclick = () => selectMatchCard(el);
    document.getElementById('match-right').appendChild(el);
  });
}

function selectMatchCard(el) {
  if (el.classList.contains('matched')) return;
  if (!matchSelected) {
    matchSelected = el;
    el.classList.add('selected');
  } else {
    if (matchSelected.dataset.side === el.dataset.side) {
      matchSelected.classList.remove('selected');
      matchSelected = el;
      el.classList.add('selected');
      return;
    }
    const enWord = matchSelected.dataset.side==='left' ? matchSelected.dataset.word : el.dataset.word;
    const trWord = matchSelected.dataset.side==='right' ? matchSelected.dataset.word : el.dataset.word;
    const pair = matchData.find(d=>d.en===enWord);
    const isMatch = pair && pair.tr === trWord;

    if (isMatch) {
      matchSelected.classList.remove('selected'); matchSelected.classList.add('matched');
      el.classList.add('matched');
      document.getElementById('match-fb').textContent = '✅ Correct! ' + enWord + ' = ' + trWord;
      document.getElementById('match-fb').style.color = 'var(--green)';
      addScore(10);
      matchedCount++;
      if (matchedCount === matchData.length) {
        setTimeout(()=>document.getElementById('match-congrats').classList.add('show'),600);
      }
    } else {
      matchSelected.classList.remove('selected');
      matchSelected.classList.add('wrong-match');
      el.classList.add('wrong-match');
      document.getElementById('match-fb').textContent = '❌ Wrong pair. Try again!';
      document.getElementById('match-fb').style.color = 'var(--red)';
      setTimeout(()=>{ matchSelected.classList.remove('wrong-match'); el.classList.remove('wrong-match'); },700);
    }
    matchSelected = null;
  }
}

/* ===== FILL IN BLANK ===== */
const fillData = [
  { sentence: "The ___ shines brightly every morning.", answer: "sun", options: ["sun","rain","book","cat"], tr: "Her sabah ___ parlak parlar." },
  { sentence: "She loves to read ___ before bed.", answer: "books", options: ["books","apples","clouds","shoes"], tr: "Yatmadan önce ___ okumayı sever." },
  { sentence: "We drink ___ every day to stay healthy.", answer: "water", options: ["water","music","time","stone"], tr: "Sağlıklı kalmak için her gün ___ içeriz." },
  { sentence: "The ___ barked loudly at the stranger.", answer: "dog", options: ["dog","tree","moon","cup"], tr: "___ yabancıya yüksek sesle havladı." },
  { sentence: "In spring, the ___ are full of colorful flowers.", answer: "gardens", options: ["gardens","skies","rivers","clouds"], tr: "İlkbaharda ___ renkli çiçeklerle dolar." },
];

let fillDone = 0;

function buildFill() {
  const c = document.getElementById('fill-container');
  c.innerHTML = '';
  fillData.forEach((d,i) => {
    const parts = d.sentence.split('___');
    c.innerHTML += `
    <div class="riddle-card" id="fcard-${i}">
      <div class="riddle-number">Sentence ${i+1} of ${fillData.length}</div>
      <div class="fill-sentence">
        ${parts[0]}<input class="blank-input" id="finput-${i}" placeholder="???" autocomplete="off" onkeydown="if(event.key==='Enter')checkFill(${i})">${parts[1]}
      </div>
      <div class="translation" style="display:block;margin-bottom:16px">🇹🇷 ${d.tr.replace('___','___')}</div>
      <div class="word-options" id="foptions-${i}"></div>
      <div style="display:flex;gap:10px;flex-wrap:wrap">
        <button class="btn btn-primary" onclick="checkFill(${i})">Check ✓</button>
      </div>
      <div class="feedback" id="ffb-${i}">
        <div class="feedback-icon" id="ffbicon-${i}"></div>
        <div id="ffbtext-${i}"></div>
      </div>
    </div>`;
  });

  fillData.forEach((d,i)=>{
    const opts = [...d.options].sort(()=>Math.random()-0.5);
    const cont = document.getElementById('foptions-'+i);
    opts.forEach(o=>{
      const btn = document.createElement('div');
      btn.className='word-option';
      btn.textContent=o;
      btn.onclick=()=>{
        if(btn.classList.contains('used'))return;
        document.getElementById('finput-'+i).value=o;
        cont.querySelectorAll('.word-option').forEach(b=>b.classList.remove('selected-opt'));
        btn.style.borderColor='var(--gold)';
      };
      cont.appendChild(btn);
    });
  });
  updateFillProgress();
}

function checkFill(i) {
  const inp = document.getElementById('finput-'+i);
  const fb = document.getElementById('ffb-'+i);
  const val = inp.value.trim().toLowerCase();
  const correct = fillData[i].answer.toLowerCase();
  const fbText = document.getElementById('ffbtext-'+i);
  const fbIcon = document.getElementById('ffbicon-'+i);

  if (!val) return;

  if (val === correct) {
    inp.classList.add('correct-blank');
    inp.disabled = true;
    fb.classList.add('show','correct-fb');
    fb.classList.remove('wrong-fb');
    fbIcon.textContent = '✓';
    fbText.textContent = 'Correct! "' + fillData[i].answer + '" is the right word.';
    addScore(12);
    fillDone++;
    updateFillProgress();
    document.getElementById('foptions-'+i).querySelectorAll('.word-option').forEach(b=>{
      if(b.textContent===fillData[i].answer){ b.classList.add('used'); b.style.borderColor='var(--green)'; b.style.color='var(--green)'; }
    });
    if (fillDone === fillData.length) {
      setTimeout(()=>document.getElementById('fill-congrats').classList.add('show'),600);
    }
  } else {
    inp.classList.remove('wrong-blank');
    void inp.offsetWidth;
    inp.classList.add('wrong-blank');
    fb.classList.add('show','wrong-fb');
    fb.classList.remove('correct-fb');
    fbIcon.textContent = '✗';
    fbText.textContent = 'Not quite! Try another word.';
    setTimeout(()=>{ inp.classList.remove('wrong-blank'); fb.classList.remove('show','wrong-fb'); inp.value=''; },1200);
  }
}

function updateFillProgress() {
  const pct = (fillDone / fillData.length) * 100;
  document.getElementById('fill-progress').style.width = pct + '%';
  document.getElementById('fill-progress-text').textContent = fillDone + ' / ' + fillData.length;
}

function restartAll() {
  totalScore = 0; riddleDone = 0; matchedCount = 0; fillDone = 0; matchSelected = null;
  document.getElementById('score-display').textContent = '0';
  document.getElementById('level-display').textContent = 'Beginner';
  document.getElementById('riddle-congrats').classList.remove('show');
  document.getElementById('match-congrats').classList.remove('show');
  document.getElementById('fill-congrats').classList.remove('show');
  buildRiddles(); buildMatch(); buildFill();
  switchTab('riddles');
}

buildRiddles();
buildMatch();
buildFill();
</script>
</body>
</html>
