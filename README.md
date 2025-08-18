<!doctype html>
<html lang="bn">
<head>
  <meta charset="utf-8">
  <title>Smart Crime Tracker — Lite (Single File)</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <!-- Leaflet (no API key required) -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
        integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="">
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
          integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

  <style>
    :root{
      --bg:#0b1020; --card:#121a2d; --accent:#4fd1c5; --text:#e6f0ff; --muted:#9bb0c9; --danger:#ee6666;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:system-ui,Segoe UI,Roboto,Inter,Arial}
    a{color:inherit}
    .hide{display:none !important}

    /* Opening */
    #opening{
      height:100vh; color:var(--text);
      background:
        radial-gradient(1000px 400px at 20% 20%, #1b2a5b, #0b1020),
        radial-gradient(800px 300px at 80% 30%, #0f1a3b, #0b1020),
        radial-gradient(600px 200px at 40% 80%, #12204a, #0b1020);
      display:flex; align-items:center; justify-content:center; text-align:center; padding:24px;
    }
    #opening .wrap{max-width:720px}
    .bism{font-size:34px; margin:0 0 6px}
    .title{font-size:22px; margin:0 0 8px; color:var(--accent)}
    .sub{color:var(--muted); margin-bottom:16px}
    .btn{background:var(--accent); color:#062020; border:none; padding:12px 18px; border-radius:12px; font-weight:700; cursor:pointer}
    .btn.small{padding:8px 12px; border-radius:10px; font-size:14px}
    .btn.danger{background:var(--danger); color:#fff}

    /* Auth card */
    #auth{min-height:100vh; background:linear-gradient(180deg,#0b1020,#0d1427); display:flex; align-items:center; justify-content:center; color:var(--text)}
    .card{background:var(--card); padding:28px; border-radius:16px; width:min(420px,92%); box-shadow:0 8px 32px rgba(0,0,0,.35)}
    .card h2{margin:0 0 12px}
    label{display:block; font-size:14px; margin:10px 0 6px; color:var(--muted)}
    input{width:100%; padding:12px; border-radius:10px; border:1px solid #25314f; background:#0e1627; color:var(--text)}
    .hint{font-size:12px; color:var(--muted); margin-top:8px}
    .error{color:#ff7676; font-size:13px; margin-top:8px}

    /* Dashboard */
    #app{min-height:100vh; background:#0b1020; color:var(--text)}
    .topbar{display:flex; align-items:center; justify-content:space-between; padding:12px 16px; background:#0c1529; position:sticky; top:0; z-index:50}
    .brand{font-weight:800; color:var(--accent)}
    .nav a{color:var(--muted); text-decoration:none; margin-right:12px}
    .nav a.active{color:#fff; font-weight:700}
    main{padding:16px}
    .tab{display:none}
    .tab.active{display:block}
    .panel{margin-top:12px; padding:12px; background:#0e172b; border:1px solid #1e2a46; border-radius:12px}
    .list{margin:8px 0; padding-left:18px}
    .map{height:320px; border-radius:12px; overflow:hidden; border:1px solid #1e2a46}
    .grid{display:grid; gap:12px}
    @media(min-width:900px){ .grid-2{grid-template-columns:1fr 1fr} }
    .pill{display:inline-block; padding:4px 8px; border-radius:999px; font-size:12px; background:#132042; color:#b7c9ff}
  </style>
</head>
<body>

  <!-- Opening -->
  <section id="opening">
    <div class="wrap">
      <h1 class="bism">بِسْمِ اللّٰهِ الرَّحْمٰنِ الرَّحِيْمِ</h1>
      <h2 class="title">Smart Crime Tracker — Lite</h2>
      <p class="sub">আল্লাহর নামে শুরু করি — ন্যায়ের পথে প্রযুক্তির হাতিয়ার</p>

      <!-- Optional: place your audio at assets/sounds/surah_ikhlas.mp3 in repo root -->
      <audio id="recitation" controls>
        <source src="assets/sounds/surah_ikhlas.mp3" type="audio/mpeg">
      </audio>

      <div style="margin-top:14px">
        <button class="btn" id="enterBtn">চালিয়ে যান</button>
      </div>
      <p class="hint" style="margin-top:10px;color:#9bb0c9">টাচ করলে অডিও চালু হতে পারে (মোবাইলে).</p>
    </div>
  </section>

  <!-- Auth -->
  <section id="auth" class="hide">
    <div class="card">
      <h2>Admin Login</h2>
      <form id="loginForm">
        <label>Email</label>
        <input type="email" id="email" placeholder="smartcrimetracker@gmail.com" required>
        <label>Password</label>
        <input type="password" id="password" placeholder="********" required>
        <button type="submit" class="btn" style="margin-top:12px;width:100%">Login</button>
        <div id="loginError" class="error"></div>
        <p class="hint">ডেমো লগইন (client-side)। প্রোডাকশনে server-side auth লাগবে।</p>
      </form>
    </div>
  </section>

  <!-- App -->
  <section id="app" class="hide">
    <header class="topbar">
      <div class="brand">Smart Crime Tracker — Lite</div>
      <nav class="nav">
        <a href="#" data-tab="search" class="active">Number Search</a>
        <a href="#" data-tab="map">Location</a>
        <a href="#" data-tab="db">Criminal DB</a>
      </nav>
      <div>
        <span class="pill" id="userTag">Admin</span>
        <button class="btn small danger" id="logoutBtn">Logout</button>
      </div>
    </header>

    <main>
      <!-- Search -->
      <section id="tab-search" class="tab active">
        <h3>🔍 Number Search (Demo)</h3>
        <form id="searchForm">
          <input type="tel" id="msisdn" placeholder="01XXXXXXXXX" required
                 style="padding:10px;border-radius:10px;border:1px solid #25314f;background:#0e1627;color:#e6f0ff">
          <button class="btn" type="submit" style="margin-left:8px">Search</button>
        </form>
        <div id="searchResult" class="panel">Ready.</div>
      </section>

      <!-- Map -->
      <section id="tab-map" class="tab">
        <h3>🗺️ Live Location (Placeholder)</h3>
        <p class="hint">এখানে Leaflet map দেখানো হচ্ছে। পূর্ণ সংস্করণে secured backend + device signals যুক্ত হবে।</p>
        <div id="map" class="map"></div>
      </section>

      <!-- DB -->
      <section id="tab-db" class="tab">
        <h3>📁 Criminal DB (Demo)</h3>
        <p class="hint">ডেমো ডেটা — পূর্ণ সংস্করণে সার্ভার DB থাকবে।</p>
        <ul id="dbList" class="list"></ul>
      </section>
    </main>
  </section>

  <script>
    // ====== Config (DEMO) ======
    const ADMIN_EMAIL = "smartcrimetracker@gmail.com";
    const ADMIN_PASS  = "AlhamDulillah01";

    // ====== Opening -> Auth ======
    const opening = document.getElementById('opening');
    const auth = document.getElementById('auth');
    const app = document.getElementById('app');
    document.getElementById('enterBtn').addEventListener('click', () => {
      opening.classList.add('hide');
      auth.classList.remove('hide');
      const a = document.getElementById('recitation');
      try { a && a.pause(); } catch(e){}
    });
    // try autoplay on first touch (mobile)
    document.addEventListener('touchstart', function(){
      const a = document.getElementById('recitation');
      if(a && a.paused) a.play().catch(()=>{});
    }, {once:true});

    // ====== Auth flow (client-side demo) ======
    const form = document.getElementById('loginForm');
    form.addEventListener('submit', (e)=>{
      e.preventDefault();
      const email = (document.getElementById('email').value || '').trim();
      const pass  = (document.getElementById('password').value || '').trim();
      const err = document.getElementById('loginError');

      if(email===ADMIN_EMAIL && pass===ADMIN_PASS){
        sessionStorage.setItem('sct_auth','1');
        err.textContent = "";
        auth.classList.add('hide');
        app.classList.remove('hide');
        initApp();
      }else{
        err.textContent = "❌ ভুল ইমেইল বা পাসওয়ার্ড!";
      }
    });

    function isAuthed(){ return sessionStorage.getItem('sct_auth')==='1'; }
    function logout(){
      sessionStorage.removeItem('sct_auth');
      app.classList.add('hide');
      auth.classList.remove('hide');
    }
    document.getElementById('logoutBtn').addEventListener('click', logout);

    // ====== Tabs ======
    document.querySelectorAll('.nav a').forEach(a=>{
      a.addEventListener('click', (e)=>{
        e.preventDefault();
        const tab = a.getAttribute('data-tab');
        document.querySelectorAll('.nav a').forEach(x=>x.classList.remove('active'));
        a.classList.add('active');
        document.querySelectorAll('.tab').forEach(x=>x.classList.remove('active'));
        document.getElementById('tab-'+tab).classList.add('active');
        if(tab==='map'){ setTimeout(()=> mapInvalidate(), 50); }
      });
    });

    // ====== Demo DB & Search ======
    const demoDB = [
      { number:"+8801712345678", name:"Unknown Handler", risk:"HIGH",   notes:"Frequent cross-border calls"},
      { number:"+8801811223344", name:"Trader A",       risk:"MEDIUM", notes:"Linked to market X"},
      { number:"+8801999888877", name:"Courier Z",      risk:"LOW",    notes:"Pattern under watch"}
    ];
    function bootDB(){
      const ul = document.getElementById('dbList');
      ul.innerHTML = "";
      demoDB.forEach(x=>{
        const li = document.createElement('li');
        li.textContent = `${x.name} — ${x.number} — Risk: ${x.risk} — ${x.notes}`;
        ul.appendChild(li);
      });
    }
    document.getElementById('searchForm').addEventListener('submit',(e)=>{
      e.preventDefault();
      const msisdn = (document.getElementById('msisdn').value || '').trim();
      const div = document.getElementById('searchResult');
      if(!msisdn){ div.textContent="Enter a number."; return; }
      const match = demoDB.find(x=>x.number.endsWith(msisdn.slice(-6)));
      if(match){
        div.innerHTML = `<strong>Match found</strong><br>Name: ${match.name}<br>Number: ${match.number}<br>Risk: ${match.risk}<br>Notes: ${match.notes}`;
      }else{
        div.textContent = "No local match. (Full version will query secure backend)";
      }
    });

    // ====== Map (Leaflet) ======
    let map, marker;
    function bootMap(){
      map = L.map('map').setView([23.777, 90.399], 11); // Dhaka
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; OpenStreetMap'
      }).addTo(map);
      marker = L.marker([23.777,90.399]).addTo(map).bindPopup('Default: Dhaka').openPopup();

      // Try browser geolocation (user consent)
      if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition((pos)=>{
          const {latitude, longitude} = pos.coords;
          map.setView([latitude, longitude], 13);
          marker.setLatLng([latitude, longitude]).bindPopup('Your device location').openPopup();
        }, ()=>{/* ignore errors for demo */});
      }
    }
    function mapInvalidate(){ if(map){ map.invalidateSize(); } }

    // ====== App init after login ======
    function initApp(){
      if(!isAuthed()){
        auth.classList.remove('hide'); app.classList.add('hide'); return;
      }
      bootDB();
      if(!map){ bootMap(); }
    }

    // Deep link: if already authed (refresh case)
    window.addEventListener('load', ()=>{
      if(isAuthed()){
        opening.classList.add('hide');
        auth.classList.add('hide');
        app.classList.remove('hide');
        initApp();
      }
    });
  </script>
</body>
</html>
