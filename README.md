<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pulse — Haber</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a12;
    --bg2: #12121e;
    --bg3: #1a1a2e;
    --card: #16162a;
    --accent1: #ff4d6d;
    --accent2: #7b5ea7;
    --accent3: #00d4aa;
    --accent4: #ffa94d;
    --text: #f0eeff;
    --muted: #8884a8;
    --border: rgba(120, 100, 200, 0.18);
  }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  .bg-orb {
    position: fixed;
    border-radius: 50%;
    filter: blur(80px);
    pointer-events: none;
    z-index: 0;
  }
  .orb1 { width: 500px; height: 500px; background: rgba(123,94,167,0.15); top: -100px; right: -100px; }
  .orb2 { width: 400px; height: 400px; background: rgba(255,77,109,0.1); bottom: 0; left: -80px; }
  .orb3 { width: 300px; height: 300px; background: rgba(0,212,170,0.08); top: 50%; left: 40%; }

  .wrapper { position: relative; z-index: 1; max-width: 1200px; margin: 0 auto; padding: 0 24px 60px; }

  /* ── HEADER ── */
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 28px 0 20px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 32px;
  }
  .logo {
    font-family: 'Playfair Display', serif;
    font-size: 32px;
    font-weight: 900;
    letter-spacing: -1px;
    background: linear-gradient(135deg, var(--accent1), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    cursor: pointer;
  }
  .logo span { -webkit-text-fill-color: var(--accent3); }
  .nav { display: flex; gap: 28px; align-items: center; }
  .nav a {
    color: var(--muted);
    text-decoration: none;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    transition: color 0.2s;
    cursor: pointer;
  }
  .nav a:hover { color: var(--text); }
  .nav a.active { color: var(--accent3); }
  .date-chip {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 5px 14px;
    font-size: 12px;
    color: var(--muted);
    letter-spacing: 0.5px;
  }

  /* ── HAVA DURUMU ── */
  .weather-strip {
    background: linear-gradient(135deg, #1a1040 0%, #0e1f40 50%, #0a2a30 100%);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 24px 32px;
    margin-bottom: 36px;
    display: flex;
    align-items: center;
    gap: 0;
    overflow: hidden;
    position: relative;
  }
  .weather-strip::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at 20% 50%, rgba(0,150,255,0.08) 0%, transparent 60%);
    pointer-events: none;
  }
  .weather-main {
    display: flex;
    align-items: center;
    gap: 16px;
    flex: 0 0 auto;
    padding-right: 36px;
    border-right: 1px solid var(--border);
  }
  .weather-icon { font-size: 52px; line-height: 1; }
  .weather-city { font-size: 12px; text-transform: uppercase; letter-spacing: 1px; color: var(--muted); margin-bottom: 2px; }
  .weather-temp { font-size: 44px; font-weight: 300; line-height: 1; color: #e0f4ff; }
  .weather-desc { font-size: 13px; color: #7ab8d4; margin-top: 2px; }
  .weather-details {
    display: flex;
    gap: 0;
    flex: 1;
    padding: 0 36px;
  }
  .wd-item {
    flex: 1;
    text-align: center;
    padding: 8px 12px;
    border-right: 1px solid rgba(255,255,255,0.05);
  }
  .wd-item:last-child { border-right: none; }
  .wd-label { font-size: 11px; text-transform: uppercase; letter-spacing: 0.8px; color: var(--muted); margin-bottom: 6px; }
  .wd-value { font-size: 20px; font-weight: 600; color: var(--text); }
  .wd-unit { font-size: 11px; color: var(--muted); }
  .weather-forecast {
    display: flex;
    gap: 12px;
    padding-left: 36px;
    border-left: 1px solid var(--border);
  }
  .fc-day {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    padding: 8px 10px;
    border-radius: 12px;
    transition: background 0.2s;
    cursor: default;
  }
  .fc-day:hover { background: rgba(255,255,255,0.05); }
  .fc-day.today { background: rgba(0,212,170,0.1); border: 1px solid rgba(0,212,170,0.2); }
  .fc-name { font-size: 11px; text-transform: uppercase; letter-spacing: 0.5px; color: var(--muted); }
  .fc-icon { font-size: 20px; }
  .fc-temp { font-size: 14px; font-weight: 500; color: var(--text); }

  /* ── SON DAKİKA TICKER ── */
  .ticker-wrap {
    background: var(--accent1);
    border-radius: 10px;
    padding: 10px 0;
    margin-bottom: 36px;
    overflow: hidden;
    position: relative;
    display: flex;
    align-items: center;
  }
  .ticker-label {
    background: rgba(0,0,0,0.3);
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    padding: 0 16px;
    white-space: nowrap;
    flex-shrink: 0;
    border-right: 1px solid rgba(255,255,255,0.2);
    z-index: 2;
    height: 100%;
    display: flex;
    align-items: center;
  }
  .ticker-overflow { overflow: hidden; flex: 1; }
  .ticker-track {
    display: flex;
    animation: ticker 30s linear infinite;
    white-space: nowrap;
  }
  .ticker-item {
    font-size: 13px;
    font-weight: 500;
    padding: 0 40px;
    position: relative;
  }
  .ticker-item::after {
    content: '◆';
    position: absolute;
    right: 10px;
    font-size: 8px;
    opacity: 0.6;
    top: 50%;
    transform: translateY(-50%);
  }
  @keyframes ticker { from { transform: translateX(0); } to { transform: translateX(-50%); } }

  /* ── ANA IZGARA ── */
  .grid-main {
    display: grid;
    grid-template-columns: 2fr 1fr;
    grid-template-rows: auto auto;
    gap: 24px;
    margin-bottom: 36px;
  }

  /* Hero kart */
  .hero-card {
    grid-row: 1 / 3;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 20px;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.3s, box-shadow 0.3s;
    text-decoration: none;
    display: block;
  }
  .hero-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 24px 60px rgba(123,94,167,0.25);
  }
  .hero-img {
    width: 100%;
    height: 320px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  .hero-img-bg { position: absolute; inset: 0; background: linear-gradient(135deg, #1e1050 0%, #102050 40%, #082030 100%); }
  .hero-img-shape { position: absolute; border-radius: 50%; }
  .shape1 { width: 200px; height: 200px; background: rgba(123,94,167,0.3); top: -30px; right: 40px; filter: blur(40px); }
  .shape2 { width: 150px; height: 150px; background: rgba(0,212,170,0.2); bottom: 20px; left: 30px; filter: blur(30px); }
  .hero-emoji { font-size: 80px; position: relative; z-index: 1; }
  .hero-body { padding: 24px 28px; }
  .hero-cat {
    display: inline-block;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1.2px;
    text-transform: uppercase;
    color: var(--accent3);
    border: 1px solid var(--accent3);
    border-radius: 20px;
    padding: 3px 12px;
    margin-bottom: 14px;
  }
  .hero-title {
    font-family: 'Playfair Display', serif;
    font-size: 26px;
    font-weight: 700;
    line-height: 1.3;
    color: var(--text);
    margin-bottom: 12px;
  }
  .hero-summary { font-size: 14px; color: var(--muted); line-height: 1.7; margin-bottom: 18px; }
  .hero-meta { display: flex; gap: 16px; align-items: center; }
  .meta-time { font-size: 12px; color: var(--muted); }
  .meta-read { font-size: 12px; color: var(--accent2); font-weight: 500; }

  /* Yan kartlar */
  .side-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 20px;
    cursor: pointer;
    transition: transform 0.25s, border-color 0.25s;
    text-decoration: none;
    display: block;
  }
  .side-card:hover { transform: translateY(-3px); border-color: var(--accent2); }
  .side-cat {
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    padding: 2px 10px;
    border-radius: 20px;
    margin-bottom: 10px;
    display: inline-block;
  }
  .cat-tech { background: rgba(123,94,167,0.2); color: var(--accent2); }
  .cat-fin  { background: rgba(255,169,77,0.15); color: var(--accent4); }
  .cat-sci  { background: rgba(0,212,170,0.15); color: var(--accent3); }
  .cat-pol  { background: rgba(255,77,109,0.15); color: var(--accent1); }
  .cat-eco  { background: rgba(100,200,100,0.15); color: #7de87d; }
  .side-title {
    font-family: 'Playfair Display', serif;
    font-size: 16px;
    font-weight: 700;
    line-height: 1.4;
    color: var(--text);
    margin-bottom: 10px;
  }
  .side-meta { font-size: 11px; color: var(--muted); }
  .side-card-img {
    width: 100%;
    height: 100px;
    border-radius: 10px;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 36px;
  }
  .img-tech { background: linear-gradient(135deg, #1a0e30, #0e1040); }
  .img-fin  { background: linear-gradient(135deg, #1a1000, #201800); }

  /* ── BÖLÜM BAŞLIĞI ── */
  .section-title {
    font-family: 'Playfair Display', serif;
    font-size: 20px;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* ── CANLI KARTLAR ── */
  .live-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 18px;
    margin-bottom: 36px;
  }
  .live-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 18px 22px;
    display: flex;
    align-items: center;
    gap: 16px;
    cursor: pointer;
    transition: border-color 0.2s;
    text-decoration: none;
  }
  .live-card:hover { border-color: var(--accent1); }
  .live-badge {
    background: var(--accent1);
    color: white;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 1.5px;
    padding: 3px 8px;
    border-radius: 6px;
    animation: pulse-live 1.5s ease-in-out infinite;
    flex-shrink: 0;
  }
  @keyframes pulse-live { 0%,100% { opacity: 1; } 50% { opacity: 0.5; } }
  .live-content { flex: 1; }
  .live-title { font-size: 14px; font-weight: 600; color: var(--text); margin-bottom: 4px; }
  .live-time  { font-size: 11px; color: var(--muted); }
  .live-views { font-size: 12px; color: var(--accent4); font-weight: 500; }

  /* ── HABER KART IZGARA ── */
  .card-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 18px;
    margin-bottom: 36px;
  }
  .news-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.25s, box-shadow 0.25s;
    text-decoration: none;
    display: block;
  }
  .news-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 16px 40px rgba(0,0,0,0.4);
  }
  .news-card-thumb {
    height: 110px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 38px;
  }
  .thumb-sci { background: linear-gradient(135deg, #082028, #062018); }
  .thumb-pol { background: linear-gradient(135deg, #280810, #200612); }
  .thumb-eco { background: linear-gradient(135deg, #082008, #061810); }
  .thumb-spt { background: linear-gradient(135deg, #201008, #181006); }
  .news-card-body { padding: 14px 16px; }
  .news-card-title {
    font-size: 13px;
    font-weight: 600;
    line-height: 1.45;
    color: var(--text);
    margin-bottom: 8px;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .news-card-meta { font-size: 11px; color: var(--muted); }

  /* ── SAYFA SEKMELERİ ── */
  .page-nav {
    display: flex;
    gap: 8px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 32px;
  }
  .page-tab {
    padding: 10px 20px;
    font-size: 13px;
    font-weight: 500;
    color: var(--muted);
    cursor: pointer;
    border: none;
    border-bottom: 2px solid transparent;
    background: none;
    font-family: 'DM Sans', sans-serif;
    letter-spacing: 0.3px;
    transition: all 0.2s;
  }
  .page-tab:hover { color: var(--text); }
  .page-tab.active { color: var(--accent3); border-bottom-color: var(--accent3); }

  /* ── SAYFA GÖSTERİMİ ── */
  .page { display: none; }
  .page.active { display: block; }

  /* ── EKONOMİ GÖSTERGELERİ ── */
  .market-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 14px;
    margin-bottom: 28px;
  }
  .market-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 16px 18px;
  }
  .market-label { font-size: 11px; color: var(--muted); text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 8px; }
  .market-value { font-size: 24px; font-weight: 600; color: var(--text); margin-bottom: 4px; }
  .market-change-up   { font-size: 13px; color: #7de87d; }
  .market-change-down { font-size: 13px; color: var(--accent1); }

  /* ── MAKALE SAYFASI ── */
  .article-back {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    color: var(--muted);
    font-size: 13px;
    cursor: pointer;
    margin-bottom: 28px;
    background: none;
    border: none;
    padding: 0;
    transition: color 0.2s;
    font-family: 'DM Sans', sans-serif;
  }
  .article-back:hover { color: var(--text); }
  .article-hero {
    width: 100%;
    height: 340px;
    border-radius: 20px;
    margin-bottom: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 100px;
    position: relative;
    overflow: hidden;
  }
  .article-hero-bg { position: absolute; inset: 0; }
  .article-hero-emoji { position: relative; z-index: 1; }
  .article-content { max-width: 740px; }
  .article-cat {
    display: inline-block;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1.2px;
    text-transform: uppercase;
    border-radius: 20px;
    padding: 3px 12px;
    margin-bottom: 16px;
  }
  .article-title {
    font-family: 'Playfair Display', serif;
    font-size: 38px;
    font-weight: 700;
    line-height: 1.25;
    color: var(--text);
    margin-bottom: 20px;
  }
  .article-subtitle { font-size: 18px; color: var(--muted); line-height: 1.6; margin-bottom: 28px; }
  .article-meta {
    display: flex;
    gap: 20px;
    align-items: center;
    margin-bottom: 36px;
    padding-bottom: 28px;
    border-bottom: 1px solid var(--border);
  }
  .article-author { display: flex; align-items: center; gap: 10px; }
  .author-avatar {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent2), var(--accent1));
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 13px;
    font-weight: 700;
    color: white;
    flex-shrink: 0;
  }
  .author-name { font-size: 13px; font-weight: 500; color: var(--text); }
  .author-date { font-size: 11px; color: var(--muted); }
  .article-body { font-size: 16px; line-height: 1.85; color: rgba(240,238,255,0.85); }
  .article-body p { margin-bottom: 20px; }
  .article-body strong { color: var(--text); font-weight: 600; }
  .article-quote {
    border-left: 3px solid var(--accent2);
    padding: 16px 24px;
    margin: 28px 0;
    background: var(--bg3);
    border-radius: 0 12px 12px 0;
    font-size: 17px;
    font-style: italic;
    color: var(--text);
    line-height: 1.6;
  }

  /* ── FOOTER ── */
  footer {
    border-top: 1px solid var(--border);
    padding: 28px 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .footer-logo {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 900;
    color: var(--muted);
  }
  .footer-text { font-size: 12px; color: var(--muted); }
  .footer-links { font-size: 12px; color: var(--muted); }
</style>
</head>
<body>

<!-- Arka plan efektleri -->
<div class="bg-orb orb1"></div>
<div class="bg-orb orb2"></div>
<div class="bg-orb orb3"></div>

<div class="wrapper">

  <!-- ═══════════════════════════════════
       HEADER
  ═══════════════════════════════════ -->
  <header>
    <div class="logo" onclick="showPage('home')">Puls<span>e</span></div>
    <nav class="nav">
      <a id="nav-home"      onclick="showPage('home')"      class="active">Anasayfa</a>
      <a id="nav-gundem"    onclick="showPage('gundem')"            >Gündem</a>
      <a id="nav-teknoloji" onclick="showPage('teknoloji')"         >Teknoloji</a>
      <a id="nav-ekonomi"   onclick="showPage('ekonomi')"           >Ekonomi</a>
      <a id="nav-spor"      onclick="showPage('spor')"              >Spor</a>
    </nav>
    <div class="date-chip" id="live-date"></div>
  </header>

  <!-- ═══════════════════════════════════
       SAYFA: ANASAYFA
  ═══════════════════════════════════ -->
  <div id="page-home" class="page active">

    <!-- Hava Durumu -->
    <div class="weather-strip">
      <div class="weather-main">
        <div class="weather-icon">⛅</div>
        <div>
          <div class="weather-city">📍 Ankara</div>
          <div class="weather-temp">18°</div>
          <div class="weather-desc">Az Bulutlu</div>
        </div>
      </div>
      <div class="weather-details">
        <div class="wd-item">
          <div class="wd-label">Nem</div>
          <div class="wd-value">52<span class="wd-unit">%</span></div>
        </div>
        <div class="wd-item">
          <div class="wd-label">Rüzgar</div>
          <div class="wd-value">14<span class="wd-unit"> km/s</span></div>
        </div>
        <div class="wd-item">
          <div class="wd-label">UV İndeksi</div>
          <div class="wd-value">6<span class="wd-unit"> orta</span></div>
        </div>
        <div class="wd-item">
          <div class="wd-label">Görüş</div>
          <div class="wd-value">10<span class="wd-unit"> km</span></div>
        </div>
        <div class="wd-item">
          <div class="wd-label">Basınç</div>
          <div class="wd-value">1013<span class="wd-unit"> hPa</span></div>
        </div>
      </div>
      <div class="weather-forecast">
        <div class="fc-day today">
          <div class="fc-name">Bug.</div>
          <div class="fc-icon">⛅</div>
          <div class="fc-temp">18°</div>
        </div>
        <div class="fc-day">
          <div class="fc-name">Cmt</div>
          <div class="fc-icon">🌤️</div>
          <div class="fc-temp">21°</div>
        </div>
        <div class="fc-day">
          <div class="fc-name">Paz</div>
          <div class="fc-icon">☀️</div>
          <div class="fc-temp">24°</div>
        </div>
        <div class="fc-day">
          <div class="fc-name">Pzt</div>
          <div class="fc-icon">🌧️</div>
          <div class="fc-temp">15°</div>
        </div>
        <div class="fc-day">
          <div class="fc-name">Sal</div>
          <div class="fc-icon">⛈️</div>
          <div class="fc-temp">13°</div>
        </div>
      </div>
    </div>

    <!-- Son Dakika Ticker -->
    <div class="ticker-wrap">
      <div class="ticker-label">🔴 SON DAKİKA</div>
      <div class="ticker-overflow">
        <div class="ticker-track">
          <span class="ticker-item">Merkez Bankası faiz kararını açıkladı</span>
          <span class="ticker-item">Türkiye-AB zirvesi Brüksel'de başladı</span>
          <span class="ticker-item">İstanbul'da deprem tatbikatı tamamlandı</span>
          <span class="ticker-item">Borsa İstanbul rekor tazeledi</span>
          <span class="ticker-item">Yapay zeka yasası TBMM'de oylandı</span>
          <span class="ticker-item">Galatasaray şampiyonluk kupasını kaldırdı</span>
          <span class="ticker-item">Merkez Bankası faiz kararını açıkladı</span>
          <span class="ticker-item">Türkiye-AB zirvesi Brüksel'de başladı</span>
          <span class="ticker-item">İstanbul'da deprem tatbikatı tamamlandı</span>
          <span class="ticker-item">Borsa İstanbul rekor tazeledi</span>
          <span class="ticker-item">Yapay zeka yasası TBMM'de oylandı</span>
          <span class="ticker-item">Galatasaray şampiyonluk kupasını kaldırdı</span>
        </div>
      </div>
    </div>

    <!-- Ana Izgara: Hero + 2 yan kart -->
    <div class="grid-main">
      <a class="hero-card" href="#" onclick="showArticle('ai-yasa');return false;">
        <div class="hero-img">
          <div class="hero-img-bg"></div>
          <div class="hero-img-shape shape1"></div>
          <div class="hero-img-shape shape2"></div>
          <span class="hero-emoji">🤖</span>
        </div>
        <div class="hero-body">
          <span class="hero-cat">Teknoloji</span>
          <div class="hero-title">Yapay Zeka Yasası Meclisten Geçti: Türkiye'de Bir İlk</div>
          <div class="hero-summary">Uzun süredir tartışılan yapay zeka düzenlemesi nihayet yasalaştı. Peki bu yasa şirketleri ve bireyleri nasıl etkileyecek? Uzmanlar değerlendirdi.</div>
          <div class="hero-meta">
            <span class="meta-time">2 saat önce</span>
            <span class="meta-read">6 dk okuma</span>
          </div>
        </div>
      </a>

      <a class="side-card" href="#" onclick="showArticle('borsa-rekor');return false;">
        <div class="side-card-img img-fin">💹</div>
        <span class="side-cat cat-fin">Ekonomi</span>
        <div class="side-title">Borsa İstanbul Tarihi Zirveyi Gördü</div>
        <div class="side-meta">45 dk önce · 4 dk okuma</div>
      </a>

      <a class="side-card" href="#" onclick="showArticle('deprem-tatbikat');return false;">
        <div class="side-card-img img-tech">🏗️</div>
        <span class="side-cat cat-sci">Bilim</span>
        <div class="side-title">İstanbul Deprem Tatbikatında 3 Milyon Kişi Katıldı</div>
        <div class="side-meta">1 saat önce · 3 dk okuma</div>
      </a>
    </div>

    <!-- Canlı Takip -->
    <div class="section-title">Canlı Takip</div>
    <div class="live-row">
      <a class="live-card" href="#" onclick="showArticle('ab-zirve');return false;">
        <div class="live-badge">CANLI</div>
        <div class="live-content">
          <div class="live-title">Türkiye-AB Zirvesi: Müzakereler Sürüyor</div>
          <div class="live-time">Brüksel · 3 saattir yayında</div>
        </div>
        <div class="live-views">41.2K izleyici</div>
      </a>
      <a class="live-card" href="#" onclick="showArticle('gs-kupa');return false;">
        <div class="live-badge">CANLI</div>
        <div class="live-content">
          <div class="live-title">Galatasaray Şampiyonluk Törenini İzle</div>
          <div class="live-time">Ali Sami Yen · 1 saattir yayında</div>
        </div>
        <div class="live-views">128.7K izleyici</div>
      </a>
    </div>

    <!-- Günün Haberleri -->
    <div class="section-title">Günün Haberleri</div>
    <div class="card-row">
      <a class="news-card" href="#" onclick="showArticle('mars-kesfet');return false;">
        <div class="news-card-thumb thumb-sci">🔭</div>
        <div class="news-card-body">
          <span class="side-cat cat-sci" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Uzay</span>
          <div class="news-card-title">NASA Mars'ta Su İzleri Bulduğunu Duyurdu</div>
          <div class="news-card-meta">3 saat önce · 5 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('secim-anket');return false;">
        <div class="news-card-thumb thumb-pol">🗳️</div>
        <div class="news-card-body">
          <span class="side-cat cat-pol" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Siyaset</span>
          <div class="news-card-title">Son Seçim Anketi: Sürpriz Sonuçlar Açıklandı</div>
          <div class="news-card-meta">5 saat önce · 3 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('yesil-enerji');return false;">
        <div class="news-card-thumb thumb-eco">🌿</div>
        <div class="news-card-body">
          <span class="side-cat cat-eco" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Çevre</span>
          <div class="news-card-title">Türkiye Yeşil Enerji Üretiminde Rekor Kırdı</div>
          <div class="news-card-meta">6 saat önce · 4 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('galatasaray');return false;">
        <div class="news-card-thumb thumb-spt">⚽</div>
        <div class="news-card-body">
          <span class="side-cat cat-pol" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Spor</span>
          <div class="news-card-title">Galatasaray 5. Kez Şampiyon: Taraftarlar Sokaklara Döküldü</div>
          <div class="news-card-meta">7 saat önce · 2 dk</div>
        </div>
      </a>
    </div>

  </div><!-- /page-home -->

  <!-- ═══════════════════════════════════
       SAYFA: GÜNDEM
  ═══════════════════════════════════ -->
  <div id="page-gundem" class="page">
    <div class="page-nav">
      <button class="page-tab active">Tümü</button>
      <button class="page-tab">Türkiye</button>
      <button class="page-tab">Dünya</button>
      <button class="page-tab">Ekonomi</button>
    </div>
    <div class="card-row" style="grid-template-columns:repeat(3,1fr)">
      <a class="news-card" href="#" onclick="showArticle('ab-zirve');return false;">
        <div class="news-card-thumb thumb-pol">🌍</div>
        <div class="news-card-body">
          <span class="side-cat cat-pol" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Dış Politika</span>
          <div class="news-card-title">Türkiye-AB Zirvesinde Tarihi Adım</div>
          <div class="news-card-meta">2 saat önce · 5 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('deprem-tatbikat');return false;">
        <div class="news-card-thumb thumb-sci">🏙️</div>
        <div class="news-card-body">
          <span class="side-cat cat-sci" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">İstanbul</span>
          <div class="news-card-title">3 Milyonluk Deprem Tatbikatı Tamamlandı</div>
          <div class="news-card-meta">3 saat önce · 3 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('secim-anket');return false;">
        <div class="news-card-thumb thumb-pol">📊</div>
        <div class="news-card-body">
          <span class="side-cat cat-pol" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Siyaset</span>
          <div class="news-card-title">Muhalefet Bloğu Anketlerde Yükseliyor</div>
          <div class="news-card-meta">5 saat önce · 4 dk</div>
        </div>
      </a>
    </div>
  </div>

  <!-- ═══════════════════════════════════
       SAYFA: TEKNOLOJİ
  ═══════════════════════════════════ -->
  <div id="page-teknoloji" class="page">
    <div class="page-nav">
      <button class="page-tab active">Tümü</button>
      <button class="page-tab">Yapay Zeka</button>
      <button class="page-tab">Uzay</button>
      <button class="page-tab">Siber</button>
    </div>
    <div class="card-row" style="grid-template-columns:repeat(3,1fr)">
      <a class="news-card" href="#" onclick="showArticle('ai-yasa');return false;">
        <div class="news-card-thumb" style="background:linear-gradient(135deg,#1a0e30,#0e1040)">🤖</div>
        <div class="news-card-body">
          <span class="side-cat cat-tech" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Yapay Zeka</span>
          <div class="news-card-title">Yapay Zeka Yasası Meclisten Geçti</div>
          <div class="news-card-meta">2 saat önce · 6 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('mars-kesfet');return false;">
        <div class="news-card-thumb thumb-sci">🚀</div>
        <div class="news-card-body">
          <span class="side-cat cat-sci" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Uzay</span>
          <div class="news-card-title">Mars'ta Su İzleri Bulundu</div>
          <div class="news-card-meta">3 saat önce · 5 dk</div>
        </div>
      </a>
      <a class="news-card" href="#" onclick="showArticle('yesil-enerji');return false;">
        <div class="news-card-thumb thumb-eco">⚡</div>
        <div class="news-card-body">
          <span class="side-cat cat-eco" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Enerji</span>
          <div class="news-card-title">Güneş Enerjisinde Rekor Üretim</div>
          <div class="news-card-meta">6 saat önce · 4 dk</div>
        </div>
      </a>
    </div>
  </div>

  <!-- ═══════════════════════════════════
       SAYFA: EKONOMİ
  ═══════════════════════════════════ -->
  <div id="page-ekonomi" class="page">
    <div class="page-nav">
      <button class="page-tab active">Tümü</button>
      <button class="page-tab">Borsa</button>
      <button class="page-tab">Döviz</button>
      <button class="page-tab">Emtia</button>
    </div>
    <div class="market-grid">
      <div class="market-card">
        <div class="market-label">BIST 100</div>
        <div class="market-value">10.847</div>
        <div class="market-change-up">▲ +2.14%</div>
      </div>
      <div class="market-card">
        <div class="market-label">USD/TRY</div>
        <div class="market-value">32.41</div>
        <div class="market-change-down">▼ -0.38%</div>
      </div>
      <div class="market-card">
        <div class="market-label">Altın (gr)</div>
        <div class="market-value">3.218₺</div>
        <div class="market-change-up">▲ +0.91%</div>
      </div>
      <div class="market-card">
        <div class="market-label">Petrol (WTI)</div>
        <div class="market-value">$82.4</div>
        <div class="market-change-up">▲ +1.20%</div>
      </div>
    </div>
    <div class="card-row" style="grid-template-columns:repeat(3,1fr)">
      <a class="news-card" href="#" onclick="showArticle('borsa-rekor');return false;">
        <div class="news-card-thumb img-fin">💹</div>
        <div class="news-card-body">
          <span class="side-cat cat-fin" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Borsa</span>
          <div class="news-card-title">BIST Tarihi Zirveyi Test Ediyor</div>
          <div class="news-card-meta">45 dk önce · 4 dk</div>
        </div>
      </a>
      <a class="news-card" href="#">
        <div class="news-card-thumb" style="background:linear-gradient(135deg,#1a1000,#201800)">🏦</div>
        <div class="news-card-body">
          <span class="side-cat cat-fin" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Merkez Bankası</span>
          <div class="news-card-title">Faiz Sabit Kaldı, Enflasyon Beklentisi Revize Edildi</div>
          <div class="news-card-meta">1 saat önce · 5 dk</div>
        </div>
      </a>
      <a class="news-card" href="#">
        <div class="news-card-thumb" style="background:linear-gradient(135deg,#081820,#040c18)">📈</div>
        <div class="news-card-body">
          <span class="side-cat cat-fin" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Yatırım</span>
          <div class="news-card-title">Yabancı Yatırımcı Türk Piyasalarına Dönüyor</div>
          <div class="news-card-meta">2 saat önce · 3 dk</div>
        </div>
      </a>
    </div>
  </div>

  <!-- ═══════════════════════════════════
       SAYFA: SPOR
  ═══════════════════════════════════ -->
  <div id="page-spor" class="page">
    <div class="page-nav">
      <button class="page-tab active">Futbol</button>
      <button class="page-tab">Basketbol</button>
      <button class="page-tab">Tenis</button>
      <button class="page-tab">Diğer</button>
    </div>
    <div class="card-row" style="grid-template-columns:repeat(3,1fr)">
      <a class="news-card" href="#" onclick="showArticle('galatasaray');return false;">
        <div class="news-card-thumb thumb-spt">🏆</div>
        <div class="news-card-body">
          <span class="side-cat cat-pol" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Süper Lig</span>
          <div class="news-card-title">Galatasaray 5. Kez Şampiyon Oldu</div>
          <div class="news-card-meta">7 saat önce · 2 dk</div>
        </div>
      </a>
      <a class="news-card" href="#">
        <div class="news-card-thumb thumb-spt">⚽</div>
        <div class="news-card-body">
          <span class="side-cat cat-pol" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Milli Takım</span>
          <div class="news-card-title">A Milli Takım Kadrosu Açıklandı</div>
          <div class="news-card-meta">4 saat önce · 3 dk</div>
        </div>
      </a>
      <a class="news-card" href="#">
        <div class="news-card-thumb" style="background:linear-gradient(135deg,#181020,#100818)">🏀</div>
        <div class="news-card-body">
          <span class="side-cat cat-tech" style="font-size:10px;padding:2px 8px;margin-bottom:8px;">Euroleague</span>
          <div class="news-card-title">Efes Final Four'a Yükseldi</div>
          <div class="news-card-meta">5 saat önce · 4 dk</div>
        </div>
      </a>
    </div>
  </div>

  <!-- ═══════════════════════════════════
       SAYFA: MAKALE DETAY
  ═══════════════════════════════════ -->
  <div id="page-article" class="page">
    <button class="article-back" onclick="goBack()">← Geri Dön</button>
    <div id="article-content"></div>
  </div>

  <!-- FOOTER -->
  <footer>
    <div class="footer-logo">Pulse</div>
    <div class="footer-text">© 2026 Pulse Medya · Tüm hakları saklıdır</div>
    <div class="footer-links">Gizlilik · Künye · İletişim</div>
  </footer>

</div><!-- /wrapper -->

<!-- ═══════════════════════════════════════════════════════
     JAVASCRIPT
═══════════════════════════════════════════════════════ -->
<script>
/* ─── Makale veritabanı ─── */
const articles = {
  'ai-yasa': {
    emoji: '🤖',
    heroBg: 'linear-gradient(135deg, #1e1050 0%, #102050 40%, #082030 100%)',
    cat: 'Teknoloji', catClass: 'cat-tech',
    title: 'Yapay Zeka Yasası Meclisten Geçti: Türkiye\'de Bir İlk',
    subtitle: 'Uzun süredir tartışılan yapay zeka düzenlemesi nihayet yasalaştı.',
    author: 'Zeynep Arslan', authorInit: 'ZA',
    date: '25 Nisan 2026 · 14:32', readTime: '6 dk okuma',
    body: `
      <p>Türkiye Büyük Millet Meclisi, aylardır kamuoyunda tartışılan <strong>Yapay Zeka Düzenleme Yasası'nı</strong> bugün oybirliğiyle kabul etti. Yasa, Avrupa Birliği'nin YZ Yasası'ndan ilham almakla birlikte Türkiye'ye özgü yenilikler içeriyor.</p>
      <div class="article-quote">"Bu yasa, Türkiye'yi yapay zeka alanında küresel aktörler arasına taşıyacak ilk ciddi adımdır." — Sanayi Bakanı</div>
      <p>Yasanın en dikkat çekici maddesi, yüksek riskli YZ sistemlerinin kullanımı için <strong>zorunlu denetim mekanizması</strong>. Sağlık, eğitim ve güvenlik sektörlerinde faaliyet gösteren şirketlerin, sistemlerini bağımsız kuruluşlara denetlettirmesi gerekecek.</p>
      <p>Öte yandan muhalefet, yasanın inovasyon üzerindeki olası kısıtlayıcı etkilerini sorguladı. Teknoloji şirketleri temsilcileri uyum maliyetlerinin KOBİ'leri zorlayabileceğini dile getirdi.</p>
      <p>Yasa, Cumhurbaşkanı onayından sonra <strong>1 Ocak 2027</strong> itibarıyla yürürlüğe girecek.</p>`
  },
  'borsa-rekor': {
    emoji: '💹',
    heroBg: 'linear-gradient(135deg, #1a1000, #201400)',
    cat: 'Ekonomi', catClass: 'cat-fin',
    title: 'Borsa İstanbul Tarihi Zirveyi Gördü',
    subtitle: 'BIST 100 endeksi bugün 10.847 puanla rekor tazeledi.',
    author: 'Mert Kaya', authorInit: 'MK',
    date: '25 Nisan 2026 · 16:10', readTime: '4 dk okuma',
    body: `
      <p>Borsa İstanbul bugün tarihi bir eşiği aşarak <strong>10.847 puana</strong> ulaştı. Analistler bu yükselişi yabancı yatırımcıların geri dönüşü ve olumlu enflasyon verilerine bağlıyor.</p>
      <div class="article-quote">"Piyasa uzun süredir beklenen güven ortamını nihayet hissediyor." — Baş Ekonomist, Garanti BBVA</div>
      <p>En çok yükselen sektörler arasında bankacılık, enerji ve teknoloji şirketleri yer alıyor. Yabancı yatırımcı payının son üç ayda <strong>%8,4'ten %11,2'ye</strong> yükseldiği açıklandı.</p>
      <p>Merkez Bankası'nın dün faizi sabit bırakması ve önümüzdeki çeyrekte tek haneli enflasyon beklentisi piyasaları olumlu etkiledi.</p>`
  },
  'deprem-tatbikat': {
    emoji: '🏗️',
    heroBg: 'linear-gradient(135deg, #101830, #0a2020)',
    cat: 'Gündem', catClass: 'cat-sci',
    title: '3 Milyonluk Deprem Tatbikatı: İstanbul Hazır mı?',
    subtitle: 'Türkiye tarihinin en büyük deprem tatbikatı bugün İstanbul\'da gerçekleştirildi.',
    author: 'Ayşe Demir', authorInit: 'AD',
    date: '25 Nisan 2026 · 12:00', readTime: '3 dk okuma',
    body: `
      <p>İstanbul'da bugün gerçekleştirilen tatbikata <strong>3,2 milyon kişi</strong> katıldı. Sabah 10:41'de verilen alarm sinyaliyle birlikte milyonlarca İstanbullu depremi simüle eden tatbikatın protokollerini uygulamaya geçti.</p>
      <div class="article-quote">"İstanbul 2030 yılına kadar depreme hazır olacak." — İBB Başkanı</div>
      <p>Tatbikat kapsamında <strong>847 okul, 324 hastane</strong> ve onlarca alışveriş merkezi test edildi. İlk bulgulara göre toplanma alanlarına erişimde bazı sorunlar yaşandı.</p>
      <p>AFAD Başkanı, tatbikatın sonuçlarının önümüzdeki hafta kamuoyuyla paylaşılacağını açıkladı.</p>`
  },
  'mars-kesfet': {
    emoji: '🔭',
    heroBg: 'linear-gradient(135deg, #080820, #0a1810)',
    cat: 'Uzay', catClass: 'cat-sci',
    title: 'NASA Mars\'ta Su İzleri Bulduğunu Duyurdu',
    subtitle: 'Perseverance rover\'ı Mars yüzeyinin 2 metre altında tuzlu su birikim izleri tespit etti.',
    author: 'Dr. Emre Şen', authorInit: 'EŞ',
    date: '25 Nisan 2026 · 11:15', readTime: '5 dk okuma',
    body: `
      <p>NASA'nın Perseverance aracı, Mars'ın Jezero Krateri bölgesinde yüzeyin yaklaşık <strong>2 metre altında</strong> tuzlu su varlığına işaret eden güçlü izler tespit etti. Bulgular Nature dergisinde yayımlandı.</p>
      <div class="article-quote">"Bu bulgu, Mars'ta geçmiş ya da mevcut yaşam olasılığını ciddi biçimde güçlendiriyor." — NASA Bilim Direktörü</div>
      <p>Araştırmacılar, tespit edilen mineralojik yapının suyun dönemsel olarak yüzeye çıkıp tekrar donduğuna işaret ettiğini belirtti. Benzer yapılar daha önce yalnızca Dünya'nın kutup bölgelerinde gözlemlenmişti.</p>
      <p>Keşif, 2030 için planlanan insanlı Mars misyonunun güzergâh kararlarını doğrudan etkileyebilir.</p>`
  },
  'secim-anket': {
    emoji: '🗳️',
    heroBg: 'linear-gradient(135deg, #280810, #200612)',
    cat: 'Siyaset', catClass: 'cat-pol',
    title: 'Son Seçim Anketi: Sürpriz Sonuçlar Açıklandı',
    subtitle: 'MAK Danışmanlık\'ın son anketi beklenmedik bir tabloyu ortaya koyuyor.',
    author: 'Selin Yıldız', authorInit: 'SY',
    date: '25 Nisan 2026 · 09:30', readTime: '3 dk okuma',
    body: `
      <p>MAK Danışmanlık tarafından gerçekleştirilen ve <strong>5.200 kişiyle</strong> yüz yüze yapılan anket, muhalefet bloğunun birleşik bir liste ile seçimlere girdiği senaryoda ciddi bir yükseliş kaydettiğini gösteriyor.</p>
      <div class="article-quote">"Seçmen kitlesi net bir değişim mesajı veriyor; bu sinyali ciddiye almak gerekiyor." — Siyaset Bilimci Prof. Dr. Ahmet Çelik</div>
      <p>Anket bulgularına göre ekonomik endişeler seçmen gündeminin başında yer almaya devam ediyor. Katılımcıların <strong>%67'si</strong> geçim sıkıntısını birinci öncelikli sorun olarak tanımladı.</p>
      <p>Siyasi partiler ankete ilişkin değerlendirmelerini basın toplantılarıyla kamuoyuyla paylaştı.</p>`
  },
  'yesil-enerji': {
    emoji: '🌿',
    heroBg: 'linear-gradient(135deg, #082008, #061810)',
    cat: 'Çevre', catClass: 'cat-eco',
    title: 'Türkiye Yeşil Enerji Üretiminde Rekor Kırdı',
    subtitle: 'Nisan ayında yenilenebilir kaynaklar toplam elektrik üretiminin %61\'ini karşıladı.',
    author: 'Can Öztürk', authorInit: 'CÖ',
    date: '25 Nisan 2026 · 08:45', readTime: '4 dk okuma',
    body: `
      <p>Enerji ve Tabii Kaynaklar Bakanlığı verilerine göre Nisan 2026'da yenilenebilir enerji kaynakları, toplam elektrik üretiminin <strong>%61'ini</strong> karşılayarak tarihsel rekor kırdı.</p>
      <div class="article-quote">"Bu oran Türkiye'nin 2035 yeşil enerji hedefine çok daha hızlı ulaşabileceğine işaret ediyor." — Enerji Bakanı</div>
      <p>Rekorun kırılmasında güneş enerjisindeki patlama belirleyici rol oynadı. Konya Ovası'nda devreye giren <strong>3.200 MW'lık güneş tarlası</strong>, yalnızca nisan ayında 680.000 haneye yetecek enerji üretti.</p>
      <p>Rüzgar enerjisi de Ege ve Marmara kıyılarındaki yeni türbinlerle üretimine önemli katkı sağladı. Uzmanlar, bu trendin sanayi elektrik faturalarını da düşüreceğini öngörüyor.</p>`
  },
  'galatasaray': {
    emoji: '🏆',
    heroBg: 'linear-gradient(135deg, #201008, #181006)',
    cat: 'Spor', catClass: 'cat-pol',
    title: 'Galatasaray 5. Kez Şampiyon: Taraftarlar Sokaklara Döküldü',
    subtitle: 'Sarı-kırmızılılar, Beşiktaş karşısındaki galibiyetle şampiyonluğunu ilan etti.',
    author: 'Bora Atmaca', authorInit: 'BA',
    date: '25 Nisan 2026 · 22:15', readTime: '2 dk okuma',
    body: `
      <p>Galatasaray, Beşiktaş'ı <strong>3-1</strong> mağlup ederek Süper Lig'de art arda <strong>5. şampiyonluğunu</strong> kazandı. Maç sonrası binlerce taraftar İstanbul sokaklarını kırmızı-sarıya boyadı.</p>
      <div class="article-quote">"Bu kupayı başta taraftarlarımıza olmak üzere emeği geçen herkese armağan ediyorum." — Teknik Direktör</div>
      <p>Maçın golcüleri Icardi (2 gol) ve Zaha (1 gol) oldu. Beşiktaş'ın tek golü Rafa Silva'dan geldi. Maç boyunca üstün oynayan Galatasaray, 58. dakikada aldığı kırmızı karta rağmen skoru korumayı başardı.</p>
      <p>Şampiyonluk kutlamaları gece boyunca İstanbul'un farklı semtlerinde devam etti. Polis açıklamasına göre kutlamalara katılan kişi sayısı <strong>500 bini</strong> aştı.</p>`
  },
  'ab-zirve': {
    emoji: '🌍',
    heroBg: 'linear-gradient(135deg, #0a1020, #101830)',
    cat: 'Dış Politika', catClass: 'cat-pol',
    title: 'Türkiye-AB Zirvesinde Tarihi Adım',
    subtitle: 'Brüksel\'deki zirve, iki taraf arasında onlarca yıllık donukluğu çözebilir.',
    author: 'Nilüfer Başaran', authorInit: 'NB',
    date: '25 Nisan 2026 · 10:00', readTime: '5 dk okuma',
    body: `
      <p>Brüksel'de düzenlenen Türkiye-AB Zirvesi, Dışişleri Bakanlarının katılımıyla bugün başladı. Gündemde vize serbestisi, gümrük birliğinin güncellenmesi ve yeni işbirliği çerçevesi yer alıyor.</p>
      <div class="article-quote">"Bu zirve, on yıllardır beklediğimiz gerçek müzakere sürecinin fitilini ateşleyebilir." — AB Konsey Başkanı</div>
      <p>Türkiye, vize serbestisi konusunda somut bir takvim talep ederken AB tarafı, <strong>hukuk devleti reformlarında</strong> ölçülebilir ilerleme kaydedilmesi koşulunu masaya getirdi.</p>
      <p>Zirvenin sonunda imzalanması beklenen mutabakat metninin içeriği merak konusu olmaya devam ediyor. Diplomatik kaynaklar görüşmelerin olumlu bir atmosferde sürdüğünü aktarıyor.</p>`
  },
  'gs-kupa': {
    emoji: '🏟️',
    heroBg: 'linear-gradient(135deg, #1a0808, #201010)',
    cat: 'Spor', catClass: 'cat-pol',
    title: 'Galatasaray Şampiyonluk Kutlaması Canlı Yayında',
    subtitle: 'Ali Sami Yen Spor Kompleksi\'nde tarihi tören başladı.',
    author: 'Bora Atmaca', authorInit: 'BA',
    date: '25 Nisan 2026 · 23:00', readTime: '2 dk okuma',
    body: `
      <p>Galatasaray, şampiyonluk kupasını bugün gece Ali Sami Yen Stadyumu'nda gerçekleştirilen büyük törenle kaldırdı. <strong>52.000 taraftar</strong> tribünleri doldururken milyonlarca kişi canlı yayını takip etti.</p>
      <div class="article-quote">"Bu kupa bizim değil; her maç tribünleri dolduran taraftarlarımızın!" — Kaptan Seferovic</div>
      <p>Tören boyunca gökyüzünü aydınlatan havai fişekler ve lazer gösterisi İstanbul Boğazı'ndan bile görülebildi. Kutlamalar gece yarısı sona erdi.</p>`
  }
};

/* ─── Aktif sayfa takibi ─── */
let previousPage = 'home';
let currentPage  = 'home';

/* ─── Tarih/saat widget ─── */
function updateDate() {
  const now = new Date();
  const opts = { day:'numeric', month:'long', year:'numeric', hour:'2-digit', minute:'2-digit' };
  document.getElementById('live-date').textContent = now.toLocaleDateString('tr-TR', opts);
}
updateDate();
setInterval(updateDate, 60000);

/* ─── Sayfa geçişi ─── */
function showPage(name) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav a').forEach(a => a.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  const navEl = document.getElementById('nav-' + name);
  if (navEl) navEl.classList.add('active');
  previousPage = currentPage;
  currentPage  = name;
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

/* ─── Makale göster ─── */
function showArticle(id) {
  const a = articles[id];
  if (!a) return;
  document.getElementById('article-content').innerHTML = `
    <div class="article-hero" style="">
      <div class="article-hero-bg" style="background:${a.heroBg}"></div>
      <span class="article-hero-emoji">${a.emoji}</span>
    </div>
    <div class="article-content">
      <span class="article-cat side-cat ${a.catClass}">${a.cat}</span>
      <div class="article-title">${a.title}</div>
      <div class="article-subtitle">${a.subtitle}</div>
      <div class="article-meta">
        <div class="article-author">
          <div class="author-avatar">${a.authorInit}</div>
          <div>
            <div class="author-name">${a.author}</div>
            <div class="author-date">${a.date}</div>
          </div>
        </div>
        <div style="font-size:12px;color:var(--muted);">📖 ${a.readTime}</div>
      </div>
      <div class="article-body">${a.body}</div>
    </div>`;
  showPage('article');
}

/* ─── Geri dön ─── */
function goBack() {
  showPage(previousPage === 'article' ? 'home' : previousPage);
}

/* ─── Sekme geçişleri ─── */
document.querySelectorAll('.page-nav').forEach(nav => {
  nav.querySelectorAll('.page-tab').forEach(tab => {
    tab.addEventListener('click', () => {
      nav.querySelectorAll('.page-tab').forEach(t => t.classList.remove('active'));
      tab.classList.add('active');
    });
  });
});
</script>
</body>
</html>
