<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Smart Crime Tracker — Prototype</title>
  <style>
    :root {
      --brand: #14532d;
      --brand-2: #166534;
      --ink: #0f172a;
      --muted: #475569;
      --bg: #f8fafc;
      --card: #ffffff;
    }
    body {
      margin: 0;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial;
      background: var(--bg);
      color: var(--ink);
    }
    header {
      background: linear-gradient(180deg, #0b3520, #14532d);
      padding: 20px;
      color: #fff;
      text-align: center;
    }
    header h1 {margin: 0; font-size: 1.8rem;}
    header p {margin: 5px 0 0; font-size: 1rem; opacity: .9;}
    nav {display: flex; gap: 8px; overflow-x: auto; padding: 10px; background: #0f291a;}
    nav a {
      color: #d1fae5; background: #134e4a;
      padding: 8px 12px; border-radius: 999px;
      font-size: .9rem; text-decoration: none; white-space: nowrap;
    }
    .wrap {max-width: 960px; margin: auto; padding: 15px;}
    .card {background: var(--card); padding: 15px; border-radius: 12px;
      margin: 10px 0; border: 1px solid #e2e8f0; box-shadow: 0 1px 6px rgba(0,0,0,0.05);}
    h2 {margin-top: 0; color: var(--brand-2);}
    .note {color: var(--muted); font-size: .9rem;}
    footer {text-align: center; padding: 20px; font-size: .9rem; color: var(--muted);}
    input, button {
      width: 100%; padding: 10px; margin: 6px 0;
      border-radius: 6px; border: 1px solid #ccc;
      font-size: 1rem;
    }
    button {background: var(--brand); color: #fff; border: none; cursor: pointer;}
  </style>
</head>
<body>

<header>
  <div style="background:#e6f4ea;color:#166534;padding:4px 10px;border-radius:999px;display:inline-block;font-size:.85rem;font-weight:600;margin-bottom:6px">
    শুরু করি আল্লাহর নামে
  </div>
  <h1>Smart Crime Tracker</h1>
  <p>অবহেলিত মানুষের পাশে — অপরাধ দমন ও ন্যায় প্রতিষ্ঠার জন্য</p>
</header>

<nav>
  <a href="#mission">Mission</a>
  <a href="#features">Features</a>
  <a href="#apk">APK</a>
  <a href="#islamic">Islamic</a>
  <a href="#admin">Admin</a>
</nav>

<div class="wrap">

  <section id="mission" class="card">
    <h2>Mission Statement</h2>
    <p>ন্যায় ও সত্যের পথে অপরাধ দমন—এটাই আমাদের লক্ষ্য। শুধুমাত্র অনুমোদিত কর্তৃপক্ষ এই সিস্টেম ব্যবহার করবে, অপব্যবহার সম্পূর্ণ নিষিদ্ধ।</p>
  </section>

  <section id="features" class="card">
    <h2>Features</h2>
    <ul>
      <li>CDR Tracker — কল রেকর্ড ভিজ্যুয়ালাইজেশন</li>
      <li>OSINT Tools — পাবলিক তথ্য সংগ্রহ</li>
      <li>Location Share — সম্মতির ভিত্তিতে</li>
      <li>Offline UI — মাঠ পর্যায়ে ব্যবহারযোগ্য</li>
    </ul>
    <div class="note">⚠️ এগুলো কেবল UI ডেমো। প্রকৃত ফাংশন পেতে সার্ভার সাইড প্রয়োজন।</div>
  </section>

  <section id="apk" class="card">
    <h2>Download APK</h2>
    <p>ডেমো APK ডাউনলোড করতে নিচের বাটনে ক্লিক করুন:</p>
    <button onclick="alert('Demo APK Placeholder — পরবর্তীতে আপনার লিংক বসান');">Download</button>
  </section>

  <section id="islamic" class="card">
    <h2>Islamic Reminder</h2>
    <blockquote style="background:#f1f5f9;padding:10px;border-radius:8px">
      “হাসবুনাল্লাহু ওয়া নি‘মাল ওয়াকীল” — আল্লাহ আমাদের জন্য যথেষ্ট, তিনিই উত্তম কারবারি।
    </blockquote>
  </section>

  <section id="admin" class="card">
    <h2>Admin Login (Demo)</h2>
    <p class="note">ডেমো লগইন, কোনো ডেটা সংরক্ষিত হবে না।</p>
    <input type="email" placeholder="Email">
    <input type="password" placeholder="Password">
    <button onclick="alert('Demo Login — কাজ করছে');">Login</button>
  </section>

</div>

<footer>
  © 2025 Smart Crime Tracker — আল্লাহ যেন এই উদ্যোগকে কবুল করেন।
</footer>

</body>
</html>
<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Smart Crime Tracker</title>
<style>
  :root{--brand:#14532d; --brand-2:#166534; --ink:#0f172a; --muted:#475569; --bg:#f8fafc; --card:#ffffff}
  *{box-sizing:border-box} body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial;background:var(--bg);color:var(--ink)}
  a{color:inherit}
  header.hero{background:linear-gradient(180deg,#0b3520,#14532d);color:#fff;padding:28px 16px;text-align:center}
  header.hero h1{margin:6px 0 6px;font-size:1.9rem} header.hero p{margin:0;opacity:.92}
  nav.top{display:flex;gap:10px;overflow:auto;padding:10px 12px;background:#0f291a;color:#d1fae5}
  nav.top a{white-space:nowrap;text-decoration:none;padding:8px 12px;border-radius:999px;background:#134e4a;font-size:.92rem}
  .wrap{max-width:1000px;margin:18px auto;padding:0 12px}
  .card{background:var(--card);border:1px solid #e2e8f0;border-radius:14px;padding:18px;margin:14px 0;box-shadow:0 1px 6px rgba(2,6,23,.04)}
  h2{margin:0 0 12px;color:var(--brand-2);font-size:1.26rem}
  .grid{display:grid;gap:12px} @media(min-width:900px){.grid{grid-template-columns:1fr 1fr}}
  ul.features{margin:10px 0 0 18px} ul.features li{margin:7px 0}
  .btn{display:inline-block;background:var(--brand);color:#fff;text-decoration:none;padding:10px 14px;border-radius:10px;font-weight:700;cursor:pointer;border:0}
  .btn.ghost{background:#e8f2ec;color:#14532d} .btn.red{background:#7a1111}
  .row{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
  .pill{display:inline-block;background:#e6f4ea;color:#166534;border:1px solid #ccead6;padding:4px 10px;border-radius:999px;font-size:.82rem;font-weight:700}
  footer{margin:26px 0 30px;text-align:center;color:var(--muted);font-size:.95rem}
  .note{font-size:.92rem;color:#64748b}
  .banner{border-radius:14px;overflow:hidden;border:1px solid #e2e8f0}
  .banner img{width:100%;max-height:300px;object-fit:cover;display:block}
  .brandRow{display:flex;align-items:center;gap:10px}
  .brandRow img{height:48px;width:48px;object-fit:contain;background:#fff;border-radius:10px;padding:4px;border:1px solid #e2e8f0}
  .input,.textarea,select{width:100%;padding:10px 12px;border:1px solid #cbd5e1;border-radius:10px;font-size:1rem;background:#fff}
  .textarea{min-height:110px;resize:vertical}
  .overlay{position:fixed;inset:0;background:rgba(15,23,42,.6);display:none;align-items:center;justify-content:center;padding:16px;z-index:50}
  .panel{width:100%;max-width:560px;background:#fff;border-radius:16px;padding:18px;box-shadow:0 20px 60px rgba(0,0,0,.25)}
  .panel h3{margin:0 0 10px;color:var(--brand-2)}
  .toolbar{display:flex;gap:8px;justify-content:space-between;margin-top:10px;align-items:center;flex-wrap:wrap}
  .left, .right{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
  .hide{display:none}
  pre.dua{white-space:pre-wrap;background:#f8fafc;border:1px solid #e2e8f0;border-radius:12px;padding:12px;line-height:1.7}
  .table{width:100%;border-collapse:collapse;border:1px solid #e2e8f0;border-radius:12px;overflow:hidden}
  .table th,.table td{padding:10px;border-bottom:1px solid #e2e8f0;font-size:.95rem;text-align:left}
  .table th{background:#f1f5f9}
  .kbd{font-family:ui-monospace,SFMono-Regular,Menlo,monospace;background:#eef2ff;border:1px solid #c7d2fe;border-radius:6px;padding:2px 6px}
  .badge{display:inline-block;padding:3px 8px;border-radius:999px;font-size:.78rem;border:1px solid #cbd5e1;background:#f8fafc}
  .ok{color:#166534;border-color:#bbf7d0;background:#ecfdf5}
  .rej{color:#7a1111;border-color:#fecaca;background:#fff1f2}
</style>
</head>
<body>

<header class="hero">
  <div class="pill">শুরু করি আল্লাহর নামে</div>
  <h1 class="brandRow">
    <img id="logoImg" alt="SCT Logo"
         src="data:image/svg+xml;utf8,<?xml version='1.0'?><svg xmlns='http://www.w3.org/2000/svg' width='120' height='120'><rect width='100%' height='100%' fill='%23ffffff'/><circle cx='60' cy='60' r='36' fill='%2314532d'/><text x='60' y='66' font-family='Arial' font-size='22' text-anchor='middle' fill='%23ffffff'>SCT</text></svg>">
    Smart Crime Tracker
  </h1>
  <p>অবহেলিত মানুষের পাশে — শুধু অপরাধ দমন ও ন্যায় প্রতিষ্ঠার জন্য</p>
</header>

<nav class="top">
  <a href="#mission">Mission</a>
  <a href="#features">Features</a>
  <a href="#islamic">Islamic</a>
  <a href="#adminLogin">Admin Login</a>
  <a href="#track">Live Track</a>
  <a href="#cdr">CDR</a>
  <a href="#rec">Recorder</a>
  <a href="#susp">Suspicious</a>
  <a href="#gold">GoldWatch</a>
  <a href="#panel">Admin Panel</a>
  <a href="#terms">Security & Terms</a>
</nav>

<div class="wrap">

  <!-- Banner / Mission / Features (your original) -->
  <section class="card banner">
    <img id="bannerImg" alt="Banner"
         src="data:image/svg+xml;utf8,<?xml version='1.0'?><svg xmlns='http://www.w3.org/2000/svg' width='1200' height='420'><defs><linearGradient id='g' x1='0' y1='0' x2='1' y2='1'><stop offset='0%' stop-color='%230b3520'/><stop offset='100%' stop-color='%2314532d'/></linearGradient></defs><rect width='100%' height='100%' fill='url(%23g)'/><text x='50%' y='55%' font-family='Arial' font-size='48' text-anchor='middle' fill='%23d1fae5'>Smart Crime Tracker</text></svg>">
  </section>

  <section id="mission" class="card">
    <h2>Mission Statement</h2>
    <p id="missionText">ন্যায় ও সত্যের পথে অপরাধ দমন—আমাদের লক্ষ্য। কেবল অনুমোদিত কর্তৃপক্ষ ব্যবহার করবে। অপব্যবহার সম্পূর্ণ নিষিদ্ধ।</p>
    <div class="chips" id="pillBox">
      <span class="chip">Ethical</span><span class="chip">Lawful</span><span class="chip">Secure-by-design</span>
    </div>
  </section>

  <section id="features" class="card">
    <h2>Features</h2>
    <div class="grid">
      <div>
        <ul class="features" id="featureList">
          <li>Live Mobile Tracking + Google Map (consent)</li>
          <li>CDR Log Viewer (WhatsApp/Imo/Call)</li>
          <li>Voice Recorder (Mic)</li>
          <li>Suspicious Handler (Rule-based AI)</li>
          <li>Gold Trader Surveillance</li>
          <li>Admin Approval Panel + Notes + Manual ID</li>
          <li>Islamic Start (Surah Ikhlas)</li>
          <li>Offline Mode (Service Worker)</li>
          <li>One Admin Lock — Only You</li>
        </ul>
      </div>
      <div>
        <div class="note">এখন সব ফিচার ক্লায়েন্ট-সাইডে কার্যকর। সার্ভার দিলে (API/Auth/DB) আরও শক্তিশালী হবে।</div>
      </div>
    </div>
  </section>

  <!-- Islamic -->
  <section id="islamic" class="card">
    <h2>Islamic Section</h2>
    <pre class="dua">
قُلْ هُوَ اللَّهُ أَحَدٌ
বলুন: তিনি আল্লাহ — এক। (সূরা আল-ইখলাস)
“হাসবুনাল্লাহু ওয়া নি‘মাল ওয়াকীল”
আল্লাহ আমাদের জন্য যথেষ্ট; তিনিই উত্তম কারবারি।
    </pre>
    <div class="row">
      <audio controls>
        <source src="" id="ikhlasAudio" type="audio/mpeg">
      </audio>
      <span class="note">নিজের mp3 লিংক Admin → Banner/Logo সেকশনের মত localStorage-এ যোগ করতে পারো (নিচে কোড দেওয়া আছে)।</span>
    </div>
  </section>

  <!-- Admin Login (One-admin lock) -->
  <section id="adminLogin" class="card">
    <h2>Admin Login (One Admin Lock)</h2>
    <div class="grid">
      <div>
        <label>Password</label>
        <input id="adminOnlyPass" class="input" type="password" placeholder="••••••••">
        <div class="row" style="margin-top:8px">
          <button class="btn" onclick="doAdminLogin()">Login</button>
          <button class="btn ghost" onclick="openAdmin(event)">Brand/Content Editor</button>
        </div>
        <div class="note">ডিফল্ট: <span class="kbd">Allah@1</span> — পরিবর্তন করতে নিচে “Security & Terms” সেকশনে দেয়া গাইড দেখো।</div>
        <div id="adminState" class="badge" style="margin-top:8px">Status: Locked</div>
      </div>
      <div>
        <div class="note">
          Admin login সফল হলে <b>GoldWatch</b> ও <b>Admin Panel</b> সেকশনগুলো আনলক হবে।
        </div>
      </div>
    </div>
  </section>

  <!-- Live Tracking -->
  <section id="track" class="card">
    <h2>Live Mobile Number Tracking (Consent)</h2>
    <div class="grid">
      <div>
        <label>Target Number (label only)</label>
        <input id="trackNumber" class="input" placeholder="01xxxxxxxxx">
        <div class="row" style="margin-top:8px">
          <button class="btn" onclick="startLive()">Start Live</button>
          <span class="note">এটি ডিভাইসের লোকেশন (consent) পড়ে—অপারেটর CDR নয়।</span>
        </div>
        <div class="row" style="margin-top:8px">
          Coords: <span id="coord" class="kbd">—</span>
        </div>
      </div>
      <div>
        <div id="map" style="height:320px;border:1px solid #e2e8f0;border-radius:12px;background:#f8fafc"></div>
        <div class="note">Google Maps API দিলে ম্যাপে মার্কার দেখাবে (ফাইলের নিচে স্ক্রিপ্ট নির্দেশনা আছে)।</div>
      </div>
    </div>
  </section>

  <!-- CDR -->
  <section id="cdr" class="card">
    <h2>CDR Style Log Viewer</h2>
    <div class="row">
      <button class="btn" onclick="addDemoCDR()">Add Demo</button>
      <button class="btn ghost" onclick="exportLogs()">Export</button>
      <input type="file" id="cdrImp" class="hide" accept="application/json" onchange="importLogs(event)">
      <button class="btn ghost" onclick="document.getElementById('cdrImp').click()">Import</button>
    </div>
    <div style="overflow:auto;margin-top:10px">
      <table class="table" id="cdrTable">
        <thead><tr><th>Name</th><th>Number</th><th>Type</th><th>Dir</th><th>Time</th><th>Dur</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <!-- Recorder -->
  <section id="rec" class="card">
    <h2>Voice Recorder (Mic)</h2>
    <div class="row">
      <button class="btn" onclick="startRec()">Start</button>
      <button class="btn red" onclick="stopRec()">Stop & Save</button>
      <span id="recState" class="note">Idle</span>
    </div>
    <div class="note">ব্রাউজারের মাইক্রোফোন অনুমতি লাগবে। ফোনকল রেকর্ডিং ব্রাউজারে সম্ভব নয়।</div>
  </section>

  <!-- Suspicious -->
  <section id="susp" class="card">
    <h2>Suspicious Handler (Rule-based AI)</h2>
    <div class="grid">
      <div><label>Number</label><input id="susNum" class="input" placeholder="01xxxxxxxxx"></div>
      <div><label>Context (চ্যাট/কল সারাংশ)</label><textarea id="susText" class="textarea"></textarea></div>
    </div>
    <div class="row" style="margin-top:8px"><button class="btn" onclick="evalSusp()">Evaluate</button></div>
    <div class="row" style="margin-top:8px">Score: <span id="susScore" class="kbd">0</span> <span id="susFlag" class="badge">—</span></div>
  </section>

  <!-- GoldWatch (admin-only) -->
  <section id="gold" class="card">
    <h2>Gold Trader Auto Surveillance <span class="badge" id="goldLock">Locked</span></h2>
    <div class="grid">
      <div><label>Market</label><input id="goldMarket" class="input" placeholder="Tati Bazar / Baitul Mukarram"></div>
      <div><label>Shop</label><input id="goldShop" class="input" placeholder="Shop name"></div>
      <div><label>Number</label><input id="goldNumber" class="input" placeholder="01xxxxxxxxx"></div>
    </div>
    <div class="row" style="margin-top:8px"><button class="btn" onclick="addGold()">Add</button></div>
    <div style="overflow:auto;margin-top:10px">
      <table class="table" id="goldTable">
        <thead><tr><th>Market</th><th>Shop</th><th>Number</th><th>Flag</th><th>Action</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <!-- Admin Panel (admin-only) -->
  <section id="panel" class="card">
    <h2>Admin Approval Panel <span class="badge" id="panelLock">Locked</span></h2>
    <div class="row">
      <button class="btn ghost" onclick="seedApplicants()">Seed 3 Applicants</button>
    </div>
    <div style="overflow:auto;margin-top:10px">
      <table class="table" id="queue">
        <thead><tr>
          <th>Name</th><th>Email</th><th>Org</th><th>Badge</th><th>Purpose</th><th>Status</th><th>Note</th><th>Action</th>
        </tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <section id="terms" class="card">
    <h2>Security & Terms</h2>
    <ul class="features">
      <li>শুধু অনুমোদিত কর্তৃপক্ষ ব্যবহার করতে পারবে</li>
      <li>গোপনীয়তা/আইন মান্য করা বাধ্যতামূলক</li>
      <li>অপব্যবহার শনাক্ত হলে অবিলম্বে অ্যাক্সেস স্থগিত</li>
      <li>সব কার্যক্রম সার্ভার-সাইড লগিং সুপারিশকৃত</li>
    </ul>
    <div class="note">
      <b>Admin Password পরিবর্তন:</b><br>
      DevTools Console-এ রান করো:
      <code class="kbd">localStorage.setItem('sct_admin_lock', JSON.stringify({pass:'NEWPASS'}))</code>
    </div>
  </section>

  <footer>© <span id="year"></span> Smart Crime Tracker — আল্লাহ যেন এই উদ্যোগকে কবুল করেন</footer>
</div>

<!-- Admin Overlay (branding/content) — kept from your version -->
<div id="adminOverlay" class="overlay" aria-modal="true" role="dialog">
  <div class="panel">
    <h3>Admin Panel (Local Branding)</h3>
    <div id="loginBlock" class="field">
      <label>Admin Password</label>
      <input id="adminPass" class="input" type="password" placeholder="••••••••">
      <div class="toolbar">
        <div class="left">
          <button class="btn" onclick="tryLogin()">Login</button>
          <button class="btn ghost" onclick="closeAdmin()">Close</button>
        </div>
        <span class="note">ডিফল্ট: <code>Allah@1</code></span>
      </div>
    </div>

    <div id="editorBlock" class="hide">
      <div class="row">
        <button class="btn ghost" onclick="closeAdmin()">Close</button>
        <button class="btn red" onclick="logoutAdmin()">Logout</button>
      </div>

      <div class="field">
        <label>Logo (PNG/JPG)</label>
        <input type="file" id="logoFile" accept="image/*">
        <div class="note">আপলোড দিলে সাথে সাথে পেইজে দেখা যাবে</div>
      </div>

      <div class="field">
        <label>Banner (JPEG/PNG)</label>
        <input type="file" id="bannerFile" accept="image/*">
      </div>

      <div class="field">
        <label>Islamic Audio (Surah Ikhlas MP3 URL)</label>
        <input id="ikhlasUrl" class="input" placeholder="https://…/ikhlas.mp3">
        <div class="row" style="margin-top:6px"><button class="btn ghost" onclick="saveIkhlas()">Save Audio</button></div>
      </div>

      <div class="field">
        <label>Mission Text</label>
        <textarea id="missionInput" class="textarea" placeholder="মিশন লিখুন…"></textarea>
      </div>

      <div class="field">
        <label>Features (প্রতি লাইনে একটি)</label>
        <textarea id="featuresInput" class="textarea" placeholder="উদাহরণ:
CDR visual
Public OSINT
Consent-based location"></textarea>
      </div>

      <div class="toolbar">
        <div class="left">
          <button class="btn" onclick="saveAll()">Save</button>
          <button class="btn ghost" onclick="resetDefaults()">Reset</button>
        </div>
        <div class="right">
          <button class="btn ghost" onclick="exportConfig()">Backup</button>
          <input type="file" id="importFile" accept="application/json" class="hide" onchange="importConfig(event)">
          <button class="btn ghost" onclick="document.getElementById('importFile').click()">Restore</button>
        </div>
      </div>
      <div class="note" style="margin-top:8px">সব পরিবর্তন <b>localStorage</b>-এ সেভ হয় (এই ডিভাইসে স্থায়ী)।</div>
    </div>
  </div>
</div>

<script>
  // ---------- Utilities / Defaults ----------
  const $ = (q)=>document.querySelector(q);
  const yearEl = document.getElementById('year'); yearEl.textContent = new Date().getFullYear();

  const LS_KEY      = 'sct_config_v1';
  const ADMIN_LOCK  = 'sct_admin_lock';
  const CDR_KEY     = 'sct_cdr_logs';
  const GOLD_KEY    = 'sct_gold_watch';
  const APP_KEY     = 'sct_applicants_v1';
  const AUDIO_KEY   = 'sct_ikhlas_mp3';

  // initialize one-admin lock
  if(!localStorage.getItem(ADMIN_LOCK)){
    localStorage.setItem(ADMIN_LOCK, JSON.stringify({pass:'Allah@1'}));
  }
  function _getAdminPass(){ try{ return (JSON.parse(localStorage.getItem(ADMIN_LOCK)||'{}').pass)||'Allah@1'; }catch(_){ return 'Allah@1'; } }

  // ---------- Branding defaults ----------
  const defaults = {
    mission: 'ন্যায় ও সত্যের পথে অপরাধ দমন—আমাদের লক্ষ্য। কেবল অনুমোদিত কর্তৃপক্ষ ব্যবহার করবে। অপব্যবহার নিষিদ্ধ।',
    features: [
      'Live Tracking — কনসেন্ট-ভিত্তিক',
      'CDR Viewer — Import/Export',
      'Voice Recorder — Mic',
      'Suspicious Handler — Rules',
      'Gold Watch — টগল ফ্ল্যাগ',
      'Admin Panel — Approve/Reject'
    ],
    logoData: null,
    bannerData: null
  };

  function getConfig(){
    try{
      const s = localStorage.getItem(LS_KEY);
      if(!s) return {...defaults};
      const obj = JSON.parse(s);
      return {...defaults, ...obj, features: Array.isArray(obj.features)&&obj.features.length? obj.features : defaults.features};
    }catch(_){ return {...defaults} }
  }
  function setConfig(cfg){ localStorage.setItem(LS_KEY, JSON.stringify(cfg)); }

  function renderBranding(){
    const cfg = getConfig();
    if(cfg.logoData)  $('#logoImg').src   = cfg.logoData;
    if(cfg.bannerData)$('#bannerImg').src = cfg.bannerData;
    $('#missionText').textContent = cfg.mission;
    const ul = $('#featureList'); ul.innerHTML='';
    cfg.features.forEach(f=>{ const li=document.createElement('li'); li.textContent=f; ul.appendChild(li); });

    const a = localStorage.getItem(AUDIO_KEY)||'';
    if(a) $('#ikhlasAudio').src = a;
  }

  // ---------- Admin Overlay (Branding) ----------
  function openAdmin(e){ if(e) e.preventDefault(); $('#adminOverlay').style.display='flex'; }
  function closeAdmin(){ $('#adminOverlay').style.display='none'; }

  function tryLogin(){
    const p = $('#adminPass').value.trim();
    if(p===_getAdminPass()){ $('#loginBlock').classList.add('hide'); $('#editorBlock').classList.remove('hide'); loadToEditor(); }
    else { alert('Wrong password'); }
  }
  function logoutAdmin(){ $('#editorBlock').classList.add('hide'); $('#loginBlock').classList.remove('hide'); $('#adminPass').value=''; }

  function loadToEditor(){
    const cfg = getConfig();
    $('#missionInput').value = cfg.mission;
    $('#featuresInput').value = (cfg.features||[]).join('\n');
    $('#ikhlasUrl').value = localStorage.getItem(AUDIO_KEY)||'';
  }

  function saveAll(){
    const cfg = getConfig();
    cfg.mission  = $('#missionInput').value.trim() || defaults.mission;
    const lines  = $('#featuresInput').value.split('\n').map(s=>s.trim()).filter(Boolean);
    cfg.features = lines.length? lines : defaults.features;
    setConfig(cfg); renderBranding(); alert('Saved');
  }

  function saveIkhlas(){
    const url = $('#ikhlasUrl').value.trim();
    if(!/^https?:\\/\\//i.test(url)){ alert('Valid MP3 URL দিন'); return; }
    localStorage.setItem(AUDIO_KEY, url);
    $('#ikhlasAudio').src = url;
    alert('Audio saved');
  }

  const toDataURL = (file)=> new Promise((res,rej)=>{ const r=new FileReader(); r.onload=()=>res(r.result); r.onerror=rej; r.readAsDataURL(file); });
  document.addEventListener('change', async (e)=>{
    if(e.target.id==='logoFile'){
      const f=e.target.files[0]; if(!f) return;
      const data=await toDataURL(f); const cfg=getConfig(); cfg.logoData=data; setConfig(cfg); $('#logoImg').src=data; alert('Logo updated');
    }
    if(e.target.id==='bannerFile'){
      const f=e.target.files[0]; if(!f) return;
      const data=await toDataURL(f); const cfg=getConfig(); cfg.bannerData=data; setConfig(cfg); $('#bannerImg').src=data; alert('Banner updated');
    }
  });

  function exportConfig(){
    const blob = new Blob([localStorage.getItem(LS_KEY) || JSON.stringify(defaults)], {type:'application/json'});
    const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download='sct-config-backup.json'; a.click();
    setTimeout(()=>URL.revokeObjectURL(a.href), 500);
  }
  function importConfig(ev){
    const f = ev.target.files[0]; if(!f) return;
    const r = new FileReader();
    r.onload = ()=>{
      try{ const obj = JSON.parse(r.result); setConfig(obj); renderBranding(); loadToEditor(); alert('Restored'); }
      catch(_){ alert('Invalid file'); }
    };
    r.readAsText(f);
  }
  function resetDefaults(){
    if(!confirm('সকল কাস্টম পরিবর্তন মুছে ডিফল্টে ফেরত যাবে—Proceed?')) return;
    setConfig({...defaults}); renderBranding(); loadToEditor(); alert('Reset done');
  }

  // ---------- One Admin Lock (gate for admin-only sections) ----------
  let ADMIN_OK=false;
  function doAdminLogin(){
    const p=$('#adminOnlyPass').value.trim();
    if(p===_getAdminPass()){
      ADMIN_OK=true;
      $('#adminState').textContent='Status: Unlocked';
      $('#adminState').className='badge ok';
      $('#goldLock').textContent='Unlocked';
      $('#panelLock').textContent='Unlocked';
      renderGold(); renderQueue();
      toast('Admin unlocked');
    } else {
      ADMIN_OK=false;
      $('#adminState').textContent='Status: Locked';
      $('#adminState').className='badge';
      alert('Wrong password');
    }
  }

  // ---------- Live Tracking (consent-based) ----------
  function initMap(lat,lng){
    const el=$('#map');
    if(window.google && google.maps){
      const map=new google.maps.Map(el,{center:{lat,lng},zoom:15});
      new google.maps.Marker({position:{lat,lng},map});
    }else{
      el.innerHTML='<div style="padding:12px;border:1px dashed #cbd5e1;border-radius:12px;background:#f8fafc">Coordinates:<br><b>'+lat.toFixed(6)+', '+lng.toFixed(6)+'</b><br><span class="note">Google Maps script not loaded.</span></div>';
    }
  }
  function startLive(){
    if(!navigator.geolocation){ alert('Geolocation not supported'); return;}
    navigator.geolocation.watchPosition(pos=>{
      const {latitude,longitude}=pos.coords;
      $('#coord').textContent = latitude.toFixed(6)+', '+longitude.toFixed(6);
      initMap(latitude, longitude);
    }, err=>alert('Location error: '+err.message), {enableHighAccuracy:true});
  }

  // ---------- CDR Viewer ----------
  function getCDR(){ try{return JSON.parse(localStorage.getItem(CDR_KEY)||'[]')}catch(_){return []} }
  function setCDR(a){ localStorage.setItem(CDR_KEY, JSON.stringify(a||[])) }
  function addDemoCDR(){
    const now=new Date().toLocaleString();
    const demo=[
      {name:'Rahim', number:'01711111111', type:'WhatsApp', dir:'OUT', time:now, dur:'02:14'},
      {name:'Karim', number:'01822222222', type:'Call',     dir:'IN',  time:now, dur:'00:45'},
      {name:'Imo User', number:'01933333333', type:'Imo',   dir:'OUT', time:now, dur:'05:20'}
    ];
    setCDR(getCDR().concat(demo)); renderCDR();
  }
  function renderCDR(){
    const tbody=$('#cdrTable tbody'); tbody.innerHTML='';
    getCDR().forEach(r=>{
      const tr=document.createElement('tr');
      tr.innerHTML=`<td>${r.name}</td><td>${r.number}</td><td>${r.type}</td><td>${r.dir}</td><td>${r.time}</td><td>${r.dur||'-'}</td>`;
      tbody.appendChild(tr);
    });
  }
  function exportLogs(){
    const blob=new Blob([localStorage.getItem(CDR_KEY)||'[]'],{type:'application/json'});
    const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='cdr-logs.json'; a.click();
    setTimeout(()=>URL.revokeObjectURL(a.href),500);
  }
  function importLogs(ev){
    const f=ev.target.files[0]; if(!f) return;
    const r=new FileReader(); r.onload=()=>{ try{ setCDR(JSON.parse(r.result)||[]); renderCDR(); alert('CDR imported'); }catch(_){ alert('Invalid JSON') } };
    r.readAsText(f);
  }

  // ---------- Voice Recorder (mic) ----------
  let mediaRecorder, chunks=[];
  async function startRec(){
    const s = await navigator.mediaDevices.getUserMedia({audio:true});
    mediaRecorder = new MediaRecorder(s);
    mediaRecorder.ondataavailable = e => chunks.push(e.data);
    mediaRecorder.onstop = e => {
      const blob=new Blob(chunks, {type:'audio/webm'}); chunks=[];
      const url=URL.createObjectURL(blob);
      const a=document.createElement('a'); a.href=url; a.download='voice-memo.webm'; a.click();
      setTimeout(()=>URL.revokeObjectURL(url), 1000);
    };
    mediaRecorder.start();
    $('#recState').textContent='Recording...';
  }
  function stopRec(){
    if(mediaRecorder && mediaRecorder.state!=='inactive'){ mediaRecorder.stop(); $('#recState').textContent='Stopped'; }
  }

  // ---------- Suspicious Handler ----------
  const WATCH=['01712345678','01898765432'];
  const KEYWORDS=['gold','btc','terror','fraud','hawala','smuggle','tati','baitul','dollar'];
  function score(text){
    const t=(text||'').toLowerCase();
    let s=0; KEYWORDS.forEach(k=>{ if(t.includes(k)) s+=2; });
    return s;
  }
  function checkSusp(number, text){
    const base = WATCH.includes((number||'').replace(/\\s|-/g,'')) ? 5 : 0;
    return base + score(text);
  }
  function evalSusp(){
    const n=$('#susNum').value.trim();
    const t=$('#susText').value;
    const s=checkSusp(n,t);
    $('#susScore').textContent=s;
    $('#susFlag').textContent = s>=5? '⚠️ High risk' : (s>=3? '⚠️ Medium' : '✅ Low');
    $('#susFlag').className = 'badge ' + (s>=5? 'rej' : (s>=3? '' : 'ok'));
  }

  // ---------- Gold Watch (admin-only UI) ----------
  function getGold(){ try{return JSON.parse(localStorage.getItem(GOLD_KEY)||'[]')}catch(_){return []} }
  function setGold(a){ localStorage.setItem(GOLD_KEY, JSON.stringify(a||[])) }
  function addGold(){
    if(!ADMIN_OK){ alert('Admin locked'); return;}
    const m=$('#goldMarket').value.trim();
    const s=$('#goldShop').value.trim();
    const n=$('#goldNumber').value.trim();
    if(!m||!s||!n){alert('Fill all');return;}
    const list=getGold(); list.push({id:Date.now(), market:m, shop:s, number:n, flagged:false}); setGold(list);
    renderGold();
  }
  function toggleFlag(id){
    const list=getGold().map(x=> x.id===id? {...x, flagged:!x.flagged}:x ); setGold(list); renderGold();
  }
  function renderGold(){
    const tbody=$('#goldTable tbody'); tbody.innerHTML='';
    getGold().forEach(x=>{
      const tr=document.createElement('tr');
      tr.innerHTML=`<td>${x.market}</td><td>${x.shop}</td><td>${x.number}</td><td>${x.flagged? '⚠️' : '—'}</td>
      <td><button class="btn" onclick="toggleFlag(${x.id})">Toggle</button></td>`;
      tbody.appendChild(tr);
    });
  }

  // ---------- Admin Approval Panel (admin-only) ----------
  function readApps(){ try{return JSON.parse(localStorage.getItem(APP_KEY)||'[]')}catch(_){return []} }
  function writeApps(list){ localStorage.setItem(APP_KEY, JSON.stringify(list||[])) }
  function renderQueue(){
    const tbody=$('#queue tbody'); tbody.innerHTML='';
    readApps().forEach(a=>{
      const tr=document.createElement('tr');
      tr.innerHTML=`<td>${a.name}</td><td>${a.email}</td><td>${a.org}</td><td>${a.badge}</td>
        <td>${a.purpose}</td><td>${a.status}</td><td>${a.note||'—'}</td>
        <td>
          <button class="btn" onclick="setStatus('${a.id}','APPROVED')">Approve</button>
          <button class="btn red" onclick="setStatus('${a.id}','REJECTED')">Reject</button>
          <button class="btn ghost" onclick="editNote('${a.id}')">Note</button>
        </td>`;
      tbody.appendChild(tr);
    });
  }
  function setStatus(id,st){ if(!ADMIN_OK){alert('Admin locked');return;} const arr=readApps(); const i=arr.findIndex(x=>x.id===id); if(i<0) return; arr[i].status=st; writeApps(arr); renderQueue(); }
  function editNote(id){ if(!ADMIN_OK){alert('Admin locked');return;} const arr=readApps(); const i=arr.findIndex(x=>x.id===id); if(i<0) return; const v=prompt('Note',arr[i].note||''); if(v===null) return; arr[i].note=v; writeApps(arr); renderQueue(); }
  function seedApplicants(){
    const a=readApps();
    if(a.length>=3){ alert('Already seeded/has data'); return; }
    const now = new Date().toLocaleString();
    const seed=[
      {id:'u1',name:'Officer A',email:'a@dept.gov',org:'CID',badge:'A-101',purpose:'Field ops',status:'PENDING',note:''},
      {id:'u2',name:'Officer B',email:'b@dept.gov',org:'DB', badge:'B-202',purpose:'Intel colln',status:'PENDING',note:''},
      {id:'u3',name:'Officer C',email:'c@dept.gov',org:'RAB',badge:'C-303',purpose:'Surveillance',status:'PENDING',note:''}
    ];
    writeApps(a.concat(seed)); renderQueue();
  }

  // ---------- Service Worker (offline) — Blob registration ----------
  (function registerSW(){
    if(!('serviceWorker' in navigator)) return;
    const swCode = `
      const CACHE='sct-cache-v2';
      const ASSETS = self.ASSETS || [
        '/', location.pathname, 
      ];
      self.addEventListener('install',e=>{e.waitUntil(caches.open(CACHE).then(c=>c.addAll(ASSETS)).then(()=>self.skipWaiting()))});
      self.addEventListener('activate',e=>{e.waitUntil(self.clients.claim())});
      self.addEventListener('fetch',e=>{e.respondWith(caches.match(e.request).then(r=>r||fetch(e.request)))});
    `;
    const blob = new Blob([swCode], {type:'text/javascript'});
    const url  = URL.createObjectURL(blob);
    navigator.serviceWorker.register(url).catch(console.warn);
  })();

  // ---------- Small toast ----------
  function toast(msg){ const t=document.createElement('div'); t.className='toast'; t.textContent=msg; document.body.appendChild(t); t.style.display='block'; setTimeout(()=>{t.remove()},1500); }

  // ---------- init ----------
  function renderCDROnLoad(){ renderCDR(); }
  window.addEventListener('load', ()=>{ renderBranding(); renderCDROnLoad(); renderGold(); renderQueue(); });
</script>

<!-- Optional: Google Maps (uncomment and add key) -->
<!-- <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script> -->
</body>
</html>
