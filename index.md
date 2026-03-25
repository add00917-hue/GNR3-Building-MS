<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GNR3 — ระบบจัดการอาคาร</title>
  <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600&family=IBM+Plex+Sans+Thai:wght@300;400;500;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-base:       #0a0e1a;
      --bg-card:       rgba(255,255,255,0.05);
      --bg-card-hover: rgba(255,255,255,0.09);
      --border:        rgba(255,255,255,0.08);
      --border-hover:  rgba(255,255,255,0.18);
      --text-primary:  #f0f4ff;
      --text-secondary:#8b9abf;
      --text-muted:    #3d4a66;
    }
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'IBM Plex Sans Thai', 'Kanit', sans-serif;
      background: var(--bg-base);
      min-height: 100vh;
      overflow-x: hidden;
      color: var(--text-primary);
    }

    /* BG */
    .bg { position: fixed; inset: 0; z-index: 0; pointer-events: none; overflow: hidden; }
    .bg-grid {
      position: absolute; inset: 0;
      background-image:
        linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
      background-size: 52px 52px;
    }
    .bg-orb1 {
      position: absolute; width: 700px; height: 700px;
      top: -250px; left: -200px; border-radius: 50%;
      background: radial-gradient(circle, rgba(30,64,120,0.6), transparent 65%);
      filter: blur(60px);
    }
    .bg-orb2 {
      position: absolute; width: 600px; height: 600px;
      bottom: -200px; right: -150px; border-radius: 50%;
      background: radial-gradient(circle, rgba(10,60,60,0.55), transparent 65%);
      filter: blur(60px);
    }

    /* PAGE */
    .page {
      position: relative; z-index: 1;
      max-width: 980px; margin: 0 auto;
      padding: 2.5rem 1.5rem 4rem;
    }

    /* HEADER */
    .header {
      display: flex; align-items: center;
      justify-content: space-between; gap: 12px;
      margin-bottom: 2rem;
      animation: slideDown .65s cubic-bezier(.22,1,.36,1) both;
    }
    .header-left { display: flex; align-items: center; gap: 14px; }

    .logo {
      width: 52px; height: 52px; border-radius: 14px; flex-shrink: 0;
      background: linear-gradient(145deg, #1a4fd8, #0891b2);
      display: flex; align-items: center; justify-content: center;
      box-shadow: 0 0 28px rgba(8,145,178,0.40), 0 4px 14px rgba(0,0,0,0.5);
    }
    .logo span {
      font-family: 'Kanit', sans-serif;
      font-size: 13px; font-weight: 600; color: #fff; letter-spacing: 1px;
    }
    .htitle h1 {
      font-family: 'Kanit', sans-serif;
      font-size: clamp(17px, 2.8vw, 23px); font-weight: 600; color: var(--text-primary);
    }
    .htitle p { font-size: 12px; color: var(--text-secondary); margin-top: 2px; }

    .clock { text-align: right; flex-shrink: 0; }
    .clock-t {
      font-family: 'Kanit', sans-serif;
      font-size: clamp(20px, 3.5vw, 30px); font-weight: 300;
      color: #22d3ee; letter-spacing: 2px; line-height: 1;
    }
    .clock-d { font-size: 10.5px; color: var(--text-muted); margin-top: 5px; }

    /* DIVIDER */
    .div {
      height: 1px;
      background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1) 40%, rgba(255,255,255,0.1) 60%, transparent);
      margin-bottom: 1.75rem;
      animation: fadeIn .7s ease .25s both;
    }

    .slabel {
      font-size: 10.5px; font-weight: 500; letter-spacing: 2.5px;
      text-transform: uppercase; color: var(--text-muted);
      margin-bottom: 1rem;
      animation: fadeIn .6s ease .35s both;
    }

    /* GRID */
    .grid {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 12px;
    }
    @media (max-width: 860px)  { .grid { grid-template-columns: repeat(4,1fr); } }
    @media (max-width: 620px)  { .grid { grid-template-columns: repeat(3,1fr); gap: 9px; } }
    @media (max-width: 360px)  { .grid { grid-template-columns: repeat(2,1fr); } }

    /* CARD */
    .card {
      display: flex; flex-direction: column; align-items: center; gap: 9px;
      padding: 1.35rem .7rem 1.05rem;
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: 18px; text-decoration: none;
      backdrop-filter: blur(14px); -webkit-backdrop-filter: blur(14px);
      position: relative; overflow: hidden;
      transition: transform .22s cubic-bezier(.22,1,.36,1),
                  border-color .2s, background .2s, box-shadow .2s;
      opacity: 0;
      animation: cardUp .55s cubic-bezier(.22,1,.36,1) both;
    }
    .card:nth-child(1)  { animation-delay:.12s }
    .card:nth-child(2)  { animation-delay:.17s }
    .card:nth-child(3)  { animation-delay:.22s }
    .card:nth-child(4)  { animation-delay:.27s }
    .card:nth-child(5)  { animation-delay:.32s }
    .card:nth-child(6)  { animation-delay:.37s }
    .card:nth-child(7)  { animation-delay:.42s }
    .card:nth-child(8)  { animation-delay:.47s }
    .card:nth-child(9)  { animation-delay:.52s }
    .card:nth-child(10) { animation-delay:.57s }

    .card::before {
      content:''; position:absolute; inset:0; border-radius:18px;
      background: var(--glow, transparent); opacity:0;
      transition: opacity .3s;
    }
    .card:hover {
      transform: translateY(-5px) scale(1.025);
      border-color: var(--ac, var(--border-hover));
      background: var(--bg-card-hover);
      box-shadow: 0 18px 44px rgba(0,0,0,0.45), 0 0 0 1px var(--ac,transparent);
    }
    .card:hover::before { opacity:1; }
    .card:active { transform: scale(.96); }

    .iw {
      width: 50px; height: 50px; border-radius: 14px;
      display: flex; align-items: center; justify-content: center;
      transition: transform .22s ease;
    }
    .card:hover .iw { transform: scale(1.12); }

    .ct {
      font-size: 11.5px; font-weight: 500;
      color: var(--text-primary); text-align: center; line-height: 1.5;
    }
    .cb {
      font-size: 9.5px; color: var(--text-muted);
      background: rgba(255,255,255,0.05);
      border: 1px solid rgba(255,255,255,0.06);
      padding: 2px 9px; border-radius: 20px;
    }

    /* live dot */
    .live .iw::after {
      content:''; position:absolute; top:10px; right:10px;
      width:9px; height:9px; border-radius:50%;
      background:#22c55e; border:2px solid var(--bg-base);
      animation: pulse 2.2s infinite;
    }
    @keyframes pulse {
      0%,100% { box-shadow:0 0 0 0 rgba(34,197,94,.5); }
      50%      { box-shadow:0 0 0 5px rgba(34,197,94,0); }
    }

    /* FOOTER */
    .ft {
      margin-top: 2.5rem; text-align: center;
      display: flex; align-items: center; justify-content: center; gap: 8px;
      opacity: 0; animation: fadeIn .6s ease 1.1s both;
    }
    .ft span { font-size: 11px; color: var(--text-muted); }
    .ft-dot { width:3px; height:3px; border-radius:50%; background:var(--text-muted); }

    @keyframes slideDown {
      from { opacity:0; transform:translateY(-18px); }
      to   { opacity:1; transform:translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity:0; } to { opacity:1; }
    }
    @keyframes cardUp {
      from { opacity:0; transform:translateY(22px) scale(.95); }
      to   { opacity:1; transform:translateY(0) scale(1); }
    }
  </style>
</head>
<body>

<div class="bg">
  <div class="bg-grid"></div>
  <div class="bg-orb1"></div>
  <div class="bg-orb2"></div>
</div>

<div class="page">

  <header class="header">
    <div class="header-left">
      <div class="logo"><span>GNR3</span></div>
      <div class="htitle">
        <h1>ระบบจัดการ GNR3</h1>
        <p>Building Management System</p>
      </div>
    </div>
    <div class="clock">
      <div class="clock-t" id="ct">00:00:00</div>
      <div class="clock-d" id="cd">—</div>
    </div>
  </header>

  <div class="div"></div>
  <p class="slabel">เมนูหลัก &nbsp;·&nbsp; 10 รายการ</p>

  <div class="grid">

    <a class="card live" href="https://add00917-hue.github.io/carpark/" target="_blank"
       style="--ac:#22d3ee;--glow:radial-gradient(ellipse at 50% -10%,rgba(34,211,238,.14),transparent 65%)">
      <div class="iw" style="background:rgba(34,211,238,.12)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#22d3ee" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <rect x="2" y="7" width="20" height="13" rx="2"/>
          <path d="M16 7V5a2 2 0 00-2-2h-4a2 2 0 00-2 2v2"/>
          <circle cx="7.5" cy="14" r="1.5" fill="#22d3ee" stroke="none"/>
          <circle cx="16.5" cy="14" r="1.5" fill="#22d3ee" stroke="none"/>
        </svg>
      </div>
      <span class="ct">จองจอดรถ</span>
      <span class="cb">01</span>
    </a>

    <a class="card" href="02_library.html"
       style="--ac:#4ade80;--glow:radial-gradient(ellipse at 50% -10%,rgba(74,222,128,.12),transparent 65%)">
      <div class="iw" style="background:rgba(74,222,128,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#4ade80" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M4 19.5A2.5 2.5 0 016.5 17H20"/>
          <path d="M6.5 2H20v20H6.5A2.5 2.5 0 014 19.5v-15A2.5 2.5 0 016.5 2z"/>
          <line x1="9" y1="8" x2="15" y2="8"/><line x1="9" y1="12" x2="13" y2="12"/>
        </svg>
      </div>
      <span class="ct">จองห้องสมุด</span>
      <span class="cb">02</span>
    </a>

    <a class="card" href="03_playground.html"
       style="--ac:#fb923c;--glow:radial-gradient(ellipse at 50% -10%,rgba(251,146,60,.12),transparent 65%)">
      <div class="iw" style="background:rgba(251,146,60,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#fb923c" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M12 2C8 2 4 5.5 4 9.5c0 5.5 7.5 12.5 8 12.5s8-7 8-12.5C20 5.5 16 2 12 2z"/>
          <circle cx="12" cy="9.5" r="2.5"/>
        </svg>
      </div>
      <span class="ct">จอง Playground</span>
      <span class="cb">03</span>
    </a>

    <a class="card" href="04_line_notify.html"
       style="--ac:#06d755;--glow:radial-gradient(ellipse at 50% -10%,rgba(6,215,85,.11),transparent 65%)">
      <div class="iw" style="background:rgba(6,215,85,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#06d755" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M21 15a2 2 0 01-2 2H7l-4 4V5a2 2 0 012-2h14a2 2 0 012 2z"/>
          <line x1="9" y1="10" x2="15" y2="10"/><line x1="9" y1="14" x2="12" y2="14"/>
        </svg>
      </div>
      <span class="ct">แจ้งกลุ่มไลน์</span>
      <span class="cb">04</span>
    </a>

    <a class="card" href="05_supplies.html"
       style="--ac:#a78bfa;--glow:radial-gradient(ellipse at 50% -10%,rgba(167,139,250,.12),transparent 65%)">
      <div class="iw" style="background:rgba(167,139,250,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#a78bfa" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M6 2L3 6v14a2 2 0 002 2h14a2 2 0 002-2V6l-3-4z"/>
          <line x1="3" y1="6" x2="21" y2="6"/>
          <path d="M16 10a4 4 0 01-8 0"/>
        </svg>
      </div>
      <span class="ct">อุปโภคบริโภค</span>
      <span class="cb">05</span>
    </a>

    <a class="card" href="06_stock.html"
       style="--ac:#fbbf24;--glow:radial-gradient(ellipse at 50% -10%,rgba(251,191,36,.11),transparent 65%)">
      <div class="iw" style="background:rgba(251,191,36,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#fbbf24" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <rect x="2" y="3" width="6" height="18" rx="1"/>
          <rect x="9" y="8" width="6" height="13" rx="1"/>
          <rect x="16" y="5" width="6" height="16" rx="1"/>
        </svg>
      </div>
      <span class="ct">Stock</span>
      <span class="cb">06</span>
    </a>

    <a class="card" href="07_housekeeping.html"
       style="--ac:#f472b6;--glow:radial-gradient(ellipse at 50% -10%,rgba(244,114,182,.12),transparent 65%)">
      <div class="iw" style="background:rgba(244,114,182,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#f472b6" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M3 22l4-4"/>
          <path d="M12.5 2.5s3.5 4 3 7.5c-.5 3-3 4-3 4s-2.5-1-3-4c-.5-3.5 3-7.5 3-7.5z"/>
          <path d="M7 19l10-10"/>
        </svg>
      </div>
      <span class="ct">งานแม่บ้าน</span>
      <span class="cb">07</span>
    </a>

    <a class="card" href="08_security.html"
       style="--ac:#94a3b8;--glow:radial-gradient(ellipse at 50% -10%,rgba(148,163,184,.10),transparent 65%)">
      <div class="iw" style="background:rgba(148,163,184,.09)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#94a3b8" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
          <polyline points="9,12 11,14 15,10"/>
        </svg>
      </div>
      <span class="ct">งาน รปภ.</span>
      <span class="cb">08</span>
    </a>

    <a class="card" href="09_inspection.html"
       style="--ac:#38bdf8;--glow:radial-gradient(ellipse at 50% -10%,rgba(56,189,248,.12),transparent 65%)">
      <div class="iw" style="background:rgba(56,189,248,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#38bdf8" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M9 11l3 3L22 4"/>
          <path d="M21 12v7a2 2 0 01-2 2H5a2 2 0 01-2-2V5a2 2 0 012-2h11"/>
        </svg>
      </div>
      <span class="ct">ตรวจสอบพื้นที่</span>
      <span class="cb">09</span>
    </a>

    <a class="card" href="10_utilities.html"
       style="--ac:#fb7185;--glow:radial-gradient(ellipse at 50% -10%,rgba(251,113,133,.12),transparent 65%)">
      <div class="iw" style="background:rgba(251,113,133,.10)">
        <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#fb7185" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>
        </svg>
      </div>
      <span class="ct">บันทึกค่าไฟ-ประปา</span>
      <span class="cb">10</span>
    </a>

  </div>

  <footer class="ft">
    <div class="ft-dot"></div>
    <span>GNR3 Building Management</span>
    <div class="ft-dot"></div>
    <span>v 1.0</span>
    <div class="ft-dot"></div>
  </footer>
</div>

<script>
  const M=['ม.ค.','ก.พ.','มี.ค.','เม.ย.','พ.ค.','มิ.ย.','ก.ค.','ส.ค.','ก.ย.','ต.ค.','พ.ย.','ธ.ค.'];
  const D=['อาทิตย์','จันทร์','อังคาร','พุธ','พฤหัสบดี','ศุกร์','เสาร์'];
  function tick(){
    const n=new Date();
    document.getElementById('ct').textContent=
      [n.getHours(),n.getMinutes(),n.getSeconds()].map(x=>String(x).padStart(2,'0')).join(':');
    document.getElementById('cd').textContent=
      `${D[n.getDay()]}  ${n.getDate()} ${M[n.getMonth()]} ${n.getFullYear()+543}`;
  }
  tick(); setInterval(tick,1000);
</script>

</body>
</html>
