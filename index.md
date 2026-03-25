<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GNR3 — ระบบบริหารจัดการอาคาร</title>

  <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600&family=IBM+Plex+Sans+Thai:wght@300;400;500;600&display=swap" rel="stylesheet">

  <style>
    :root {
      --bg-base:#0a0e1a;
      --bg-card:rgba(255,255,255,0.05);
      --bg-card-hover:rgba(255,255,255,0.09);
      --border:rgba(255,255,255,0.08);
      --border-hover:rgba(255,255,255,0.18);
      --text-primary:#f0f4ff;
      --text-muted:#3d4a66;
    }

    *{margin:0;padding:0;box-sizing:border-box;}

    body{
      font-family:'IBM Plex Sans Thai','Kanit',sans-serif;
      background:var(--bg-base);
      min-height:100vh;
      color:var(--text-primary);
    }

    /* BG */
    .bg{position:fixed;inset:0;z-index:0;pointer-events:none;}
    .bg-grid{
      position:absolute;inset:0;
      background-image:
        linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
      background-size:52px 52px;
    }

    /* PAGE */
    .page{
      position:relative;
      z-index:1;
      max-width:980px;
      margin:0 auto;
      padding:4rem 1.5rem; /* ปรับใหม่ */
    }

    /* GRID */
    .grid{
      display:grid;
      grid-template-columns:repeat(5,1fr);
      gap:12px;
      margin-top:1rem;
    }

    @media(max-width:860px){.grid{grid-template-columns:repeat(4,1fr);}}
    @media(max-width:620px){.grid{grid-template-columns:repeat(3,1fr);gap:9px;}}
    @media(max-width:360px){.grid{grid-template-columns:repeat(2,1fr);}}

    /* CARD */
    .card{
      display:flex;
      flex-direction:column;
      align-items:center;
      gap:9px;
      padding:1.3rem .7rem;
      background:var(--bg-card);
      border:1px solid var(--border);
      border-radius:18px;
      text-decoration:none;
      backdrop-filter:blur(14px);
      transition:.25s;
    }

    .card:hover{
      transform:translateY(-5px) scale(1.03);
      border-color:var(--border-hover);
      background:var(--bg-card-hover);
    }

    .iw{
      width:50px;height:50px;
      border-radius:14px;
      display:flex;
      align-items:center;
      justify-content:center;
    }

    .ct{
      font-size:12px;
      text-align:center;
    }

    .cb{
      font-size:10px;
      color:var(--text-muted);
    }

  </style>
</head>

<body>

<div class="bg">
  <div class="bg-grid"></div>
</div>

<div class="page">

  <div class="grid">

    <a class="card" href="https://add00917-hue.github.io/carpark/" target="_blank">
      <div class="iw">🚗</div>
      <span class="ct">จองจอดรถ</span>
      <span class="cb">01</span>
    </a>

    <a class="card" href="02_library.html">
      <div class="iw">📚</div>
      <span class="ct">จองห้องสมุด</span>
      <span class="cb">02</span>
    </a>

    <a class="card" href="03_playground.html">
      <div class="iw">📍</div>
      <span class="ct">จอง Playground</span>
      <span class="cb">03</span>
    </a>

    <a class="card" href="04_line_notify.html">
      <div class="iw">💬</div>
      <span class="ct">แจ้งกลุ่มไลน์</span>
      <span class="cb">04</span>
    </a>

    <a class="card" href="05_supplies.html">
      <div class="iw">📦</div>
      <span class="ct">อุปโภคบริโภค</span>
      <span class="cb">05</span>
    </a>

    <a class="card" href="06_stock.html">
      <div class="iw">📊</div>
      <span class="ct">Stock</span>
      <span class="cb">06</span>
    </a>

    <a class="card" href="07_housekeeping.html">
      <div class="iw">🧹</div>
      <span class="ct">งานแม่บ้าน</span>
      <span class="cb">07</span>
    </a>

    <a class="card" href="08_security.html">
      <div class="iw">🛡️</div>
      <span class="ct">งาน รปภ.</span>
      <span class="cb">08</span>
    </a>

    <a class="card" href="09_inspection.html">
      <div class="iw">✅</div>
      <span class="ct">ตรวจสอบพื้นที่</span>
      <span class="cb">09</span>
    </a>

    <a class="card" href="10_utilities.html">
      <div class="iw">⚡</div>
      <span class="ct">บันทึกค่าไฟ-ประปา</span>
      <span class="cb">10</span>
    </a>

  </div>

</div>

</body>
</html>
