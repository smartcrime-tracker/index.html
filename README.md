<!doctype html>
<html lang="bn">
<head>
  <meta charset="utf-8" />
  <title>Smart Crime Tracker — All-in-One (Lite Simulation)</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <!-- Leaflet for Map -->
  <link rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="">
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
    integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

  <style>
    :root{
      --bg:#0b1020; --card:#121a2d; --accent:#4fd1c5; --text:#e6f0ff;
      --muted:#9bb0c9; --danger:#ee6666; --ok:#50fa7b; --warn:#f7c948;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:system-ui,Segoe UI,Inter,Roboto,Arial}
    a{color:inherit;text-decoration:none}

    .hide{display:none !important}
    .btn{background:var(--accent);color:#062020;border:none;padding:10px 14px;border-radius:12px;font-weight:700;cursor:pointer}
    .btn.small{padding:6px 10px;border-radius:10px;font-size:14px}
    .btn.ghost{background:#132042;color:#cfe7ff}
    .btn.danger{background:var(--danger);color:#fff}
    .btn.ok{background:var(--ok);color:#062020}
    .pill{display:inline-block;padding:4px 8px;border-radius:999px;font-size:12px;background:#132042;color:#b7c9ff}
    .tag{display:inline-block;padding:2px 8px;border-radius:8px;font-size:12px;margin-left:6px}
    .tag.high{background:#3a0e1a;color:#ff9bb0;border:1px solid #6e1f33}
    .tag.med{background:#372a05;color:#ffe59a;border:1px solid #6d5710}
    .tag.low{background:#0d2b15;color:#9bf9c1;border:1px solid #154c27}

    /* Opening */
    #opening{
      min-height:100vh;color:var(--text);
      background:
        radial-gradient(1000px 400px at 20% 20%, #1b2a5b, #0b1020),
        radial-gradient(800px 300px at 80% 30%, #0f1a3b, #0b1020),
        radial-gradient(600px 200px at 40% 80%, #12204a, #0b1020);
      display:flex;align-items:center;justify-content:center;text-align:center;padding:24px;
    }
    #opening .wrap{max-width:820px}
    .bism{font-size:34px;margin:0 0 6px}
    .title{font-size:22px;margin:0 0 8px;color:var(--accent)}
    .sub{color:var(--muted);margin-bottom:16px}
    audio{margin:10px auto 14px;display:block}

    /* Auth */
    #auth{min-height:100vh;background:linear-gradient(180deg,#0b1020,#0d1427);display:flex;align-items:center;justify-content:center;color:var(--text)}
    .card{background:var(--card);padding:28px;border-radius:16px;width:min(480px,92%);box-shadow:0 8px 32px rgba(0,0,0,.35)}
    .card h2{margin:0 0 12px}
    label{display:block;font-size:14px;margin:10px 0 6px;color:var(--muted)}
    input,select,textarea{width:100%;padding:12px;border-radius:10px;border:1px solid #25314f;background:#0e1627;color:var(--text)}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    .hint{font-size:12px;color:var(--muted);margin-top:8px}
    .error{color:#ff7676;font-size:13px;margin-top:8px}

    /* App */
    #app{min-height:100vh;background:#0b1020;color:var(--text)}
    .topbar{display:flex;align-items:center;justify-content:space-between;padding:12px 16px;background:#0c1529;position:sticky;top:0;z-index:50}
    .brand{font-weight:800;color:var(--accent)}
    .nav a{color:var(--muted);margin-right:12px}
    .nav a.active{color:#fff;font-weight:700}
    main{padding:16px}
    .grid{display:grid;gap:12px}
    @media(min-width:1000px){ .grid-2{grid-template-columns:1fr 1fr} .grid-3{grid-template-columns:1fr 1fr 1fr} }
    .panel{padding:12px;background:#0e172b;border:1px solid #1e2a46;border-radius:12px}
    table{width:100%;border-collapse:collapse;margin-top:8px}
    th,td{border-bottom:1px solid #1e2a46;padding:8px;text-align:left;font-size:14px}
    .map{height:340px;border-radius:12px;overflow:hidden;border:1px solid #1e2a46}
    .toolbar{display:flex;gap:8px;flex-wrap:wrap;margin:6px 0}
    .status{font-size:13px;color:var(--muted)}
    .log{max-height:220px;overflow:auto;background:#0e172b;border:1px solid #1e2a46;border-radius:12px;padding:8px}
    .log p{margin:0 0 6px;font-size:13px}
    .oktxt{color:#9bf9c1}
    .warntxt{color:#ffe59a}
    .dangertxt{color:#ff9bb0}
  </style>
</head>
<body>

  <!-- Opening -->
  <section id="opening">
    <div class="wrap">
      <h1 class="bism">بِسْمِ اللّٰهِ الرَّحْمٰنِ الرَّحِيْمِ</h1>
      <h2 class="title">Smart Crime Tracker — All-in-One (Lite Simulation)</h2>
      <p class="sub">আল্লাহর নামে শুরু করি — ন্যায়ের পথে প্রযুক্তির হাতিয়ার</p>

      <!-- Optional audio file path in your repo: assets/sounds/surah_ikhlas.mp3 -->
      <audio id="recitation" controls>
        <source src="assets/sounds/surah_ikhlas.mp3" type="audio/mpeg">
      </audio>

      <button class="btn" id="startBtn">চালিয়ে যান</button>
      <p class="hint">মোবাইলে টাচ করলে অডিও চালু হতে পারে।</p>
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
        <div class="row">
          <div><label>Region</label>
            <select id="region">
              <option value="bd">Bangladesh</option>
              <option value="global">Global</option>
            </select>
          </div>
          <div><label>Mode</label>
            <select id="mode">
              <option value="analyst">Analyst</option>
              <option value="field">Field</option>
              <option value="admin">Admin</option>
            </select>
          </div>
        </div>
        <button type="submit" class="btn" style="margin-top:12px;width:100%">Login</button>
        <div id="loginError" class="error"></div>
        <p class="hint">ডেমো ভেরিফিকেশন। প্রোডাকশনে সার্ভার-সাইড auth দরকার।</p>
      </form>
    </div>
  </section>

  <!-- App -->
  <section id="app" class="hide">
    <header class="topbar">
      <div class="brand">Smart Crime Tracker</div>
      <nav class="nav">
        <a href="#" data-tab="search" class="active">Number Search</a>
        <a href="#" data-tab="map">Location</a>
        <a href="#" data-tab="cdr">CDR</a>
        <a href="#" data-tab="profiles">Profiles</a>
        <a href="#" data-tab="admin">Admin</a>
        <a href="#" data-tab="alerts">Alerts</a>
        <a href="#" data-tab="audit">Audit Log</a>
      </nav>
      <div>
        <span class="pill" id="userTag">Analyst</span>
        <button class="btn small danger" id="logoutBtn">Logout</button>
      </div>
    </header>

    <main>
      <!-- Number Search -->
      <section id="tab-search" class="panel">
        <h3>🔍 Number Search</h3>
        <div class="toolbar">
          <input type="tel" id="msisdn" placeholder="01XXXXXXXXX বা +8801XXXXXXXXX" style="padding:10px;border-radius:10px;border:1px solid #25314f;background:#0e1627;color:#e6f0ff" />
          <button class="btn" id="btnSearch">Search</button>
          <button class="btn ghost" id="btnClear">Clear</button>
        </div>
        <div id="searchResult" class="panel" style="margin-top:10px">
          Ready.
        </div>
      </section>

      <div class="grid grid-2" style="margin-top:12px">
        <!-- Map -->
        <section id="tab-map" class="panel">
          <h3>🗺️ Live Location (Placeholder)</h3>
          <p class="status">Leaflet মানচিত্র — ডেমো ভিউ। সার্ভার/API ছাড়া রিয়েল ট্র্যাকিং হয় না।</p>
          <div id="map" class="map"></div>
          <div class="toolbar">
            <button class="btn small" id="btnCenterDhaka">Center: Dhaka</button>
            <button class="btn small" id="btnSimMove">Simulate Move</button>
          </div>
        </section>

        <!-- CDR -->
        <section id="tab-cdr" class="panel">
          <h3>📞 CDR (Call Detail Records) — Demo</h3>
          <div class="toolbar">
            <button class="btn small ghost" id="btnLoadCDR">Load CDR</button>
            <button class="btn small" id="btnFilterHigh">Filter: High Risk</button>
            <button class="btn small" id="btnResetCDR">Reset</button>
          </div>
          <table id="cdrTable">
            <thead>
              <tr><th>Time</th><th>From</th><th>To</th><th>Duration</th><th>Type</th><th>Risk</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </section>
      </div>

      <!-- Profiles -->
      <section id="tab-profiles" class="panel" style="margin-top:12px">
        <h3>🧾 Criminal Profiles — Demo</h3>
        <div class="toolbar">
          <input id="profileQuery" placeholder="Name/Number/Tag" style="padding:10px;border-radius:10px;border:1px solid #25314f;background:#0e1627;color:#e6f0ff" />
          <button class="btn small" id="btnProfileSearch">Search</button>
          <button class="btn small ghost" id="btnProfileReset">Reset</button>
        </div>
        <table id="profileTable">
          <thead>
            <tr><th>Name</th><th>Number</th><th>Tags</th><th>Risk</th><th>Notes</th></tr>
          </thead>
          <tbody></tbody>
        </table>
      </section>

      <!-- Admin -->
      <section id="tab-admin" class="panel" style="margin-top:12px">
        <h3>🛡️ Admin Panel — Demo</h3>
        <div class="grid grid-2">
          <div class="panel">
            <h4>New Access Requests</h4>
            <table id="reqTable">
              <thead><tr><th>Org</th><th>User</th><th>Role</th><th>Action</th></tr></thead>
              <tbody></tbody>
            </table>
          </div>
          <div class="panel">
            <h4>Assign User ID</h4>
            <div class="row">
              <input id="assignName" placeholder="Name" />
              <input id="assignId" placeholder="User ID" />
            </div>
            <textarea id="assignNote" placeholder="Private note" style="margin-top:8px"></textarea>
            <div class="toolbar">
              <button class="btn ok" id="btnAssign">Assign</button>
              <span class="status" id="assignStatus"></span>
            </div>
          </div>
        </div>
      </section>

      <!-- Alerts -->
      <section id="tab-alerts" class="panel" style="margin-top:12px">
        <h3>⚠️ Suspicious Handler — Alerts (Demo)</h3>
        <div class="toolbar">
          <button class="btn small" id="btnSimAlert">Simulate Alert</button>
          <button class="btn small ghost" id="btnClearAlerts">Clear</button>
        </div>
        <div id="alertBox" class="log"></div>
      </section>

      <!-- Audit -->
      <section id="tab-audit" class="panel" style="margin-top:12px">
        <h3>🧾 Audit Log</h3>
        <div class="toolbar">
          <button class="btn small" id="btnExportAudit">Export JSON</button>
          <span class="status">সব action লোকালি লগ হচ্ছে (client-side)।</span>
        </div>
        <div id="auditLog" class="log"></div>
      </section>
    </main>
  </section>

  <script>
    // ====== Demo Config ======
    const ADMIN_EMAIL = "smartcrimetracker@gmail.com";
    const ADMIN_PASS  = "AlhamDulillah01";

    // ====== Sections ======
    const opening = document.getElementById('opening');
    const auth = document.getElementById('auth');
    const app = document.getElementById('app');

    document.getElementById('startBtn').addEventListener('click', ()=>{
      opening.classList.add('hide');
      auth.classList.remove('hide');
      const a=document.getElementById('recitation'); try{a && a.pause();}catch(e){}
    });

    // mobile autoplay assist
    document.addEventListener('touchstart', ()=>{
      const a=document.getElementById('recitation'); if(a&&a.paused) a.play().catch(()=>{});
    }, {once:true});

    // ====== Auth ======
    const userTag = document.getElementById('userTag');
    const loginForm = document.getElementById('loginForm');
    const loginError = document.getElementById('loginError');

    function isAuthed(){ return sessionStorage.getItem('sct_auth')==='1'; }
    function logout(){
      sessionStorage.removeItem('sct_auth');
      app.classList.add('hide');
      auth.classList.remove('hide');
      logAudit("Logout");
    }
    document.getElementById('logoutBtn').addEventListener('click', logout);

    loginForm.addEventListener('submit', (e)=>{
      e.preventDefault();
      const email = (document.getElementById('email').value||'').trim();
      const pass = (document.getElementById('password').value||'').trim();
      const mode = document.getElementById('mode').value;
      if(email===ADMIN_EMAIL && pass===ADMIN_PASS){
        sessionStorage.setItem('sct_auth','1');
        userTag.textContent = mode[0].toUpperCase()+mode.slice(1);
        loginError.textContent = "";
        auth.classList.add('hide');
        app.classList.remove('hide');
        initApp();
        logAudit("Login success ("+mode+")");
      }else{
        loginError.textContent = "❌ ভুল ইমেইল বা পাসওয়ার্ড!";
        logAudit("Login failed for "+email);
      }
    });

    // Deep link restore
    window.addEventListener('load', ()=>{
      if(isAuthed()){
        opening.classList.add('hide'); auth.classList.add('hide'); app.classList.remove('hide');
        initApp();
      }
    });

    // ====== Tabs ======
    document.querySelectorAll('.nav a').forEach(a=>{
      a.addEventListener('click', (e)=>{
        e.preventDefault();
        const tab = a.getAttribute('data-tab');
        document.querySelectorAll('.nav a').forEach(x=>x.classList.remove('active'));
        a.classList.add('active');
        document.querySelectorAll('main > section, .grid > section').forEach(x=>x.classList.remove('active'));
        // activate chosen
        if(tab==='map' || tab==='cdr'){
          // these are inside grid row
          document.getElementById('tab-map').classList.add('active');
          if(tab==='cdr') document.getElementById('tab-cdr').classList.add('active');
          else document.getElementById('tab-cdr').classList.remove('active');
        }else{
          document.getElementById('tab-map').classList.remove('active');
          document.getElementById('tab-cdr').classList.remove('active');
          document.getElementById('tab-'+tab).classList.add('active');
        }
        if(tab==='map'){ setTimeout(()=> mapInvalidate(), 80); }
      });
    });

    // ====== Demo DB ======
    const peopleDB = [
      { number:"+8801712345678", name:"Unknown Handler", risk:"HIGH", tags:["border","night"], notes:"Frequent cross-border calls" },
      { number:"+8801811223344", name:"Trader A", risk:"MEDIUM", tags:["market-x"], notes:"Linked to market X" },
      { number:"+8801999888877", name:"Courier Z", risk:"LOW", tags:["courier"], notes:"Pattern under watch" },
      { number:"+8801755512345", name:"Receiver K", risk:"MEDIUM", tags:["warehouse"], notes:"Short bursts near port" },
      { number:"+8801711002200", name:"Handler M", risk:"HIGH", tags:["airport"], notes:"Night-time switches" }
    ];

    // ====== Search ======
    const searchResult = document.getElementById('searchResult');
    document.getElementById('btnSearch').addEventListener('click', ()=>{
      const msisdn = (document.getElementById('msisdn').value||'').trim();
      if(!msisdn){ searchResult.textContent="Enter a number."; return; }
      const match = peopleDB.find(x=> x.number.endsWith(msisdn.slice(-6)) || x.number===msisdn);
      if(match){
        searchResult.innerHTML = `
          <b>Match found</b> <span class="tag ${match.risk==='HIGH'?'high':match.risk==='MEDIUM'?'med':'low'}">${match.risk}</span><br>
          Name: ${match.name}<br>
          Number: ${match.number}<br>
          Tags: ${match.tags.join(", ")}<br>
          Notes: ${match.notes}
        `;
        logAudit("Search hit: "+match.number+" ("+match.risk+")");
        // highlight on map
        flashMarker();
      }else{
        searchResult.textContent = "No match in local demo DB. (Full version will query secure backend)";
        logAudit("Search miss for "+msisdn);
      }
    });
    document.getElementById('btnClear').addEventListener('click', ()=>{
      document.getElementById('msisdn').value="";
      searchResult.textContent="Ready.";
    });

    // ====== Map ======
    let map, marker, polyline;
    function bootMap(){
      map = L.map('map').setView([23.777, 90.399], 11); // Dhaka
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; OpenStreetMap'
      }).addTo(map);
      marker = L.marker([23.777, 90.399]).addTo(map).bindPopup('Default: Dhaka').openPopup();
      polyline = L.polyline([[23.777,90.399]], {color:'#4fd1c5'}).addTo(map);

      if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition((pos)=>{
          const {latitude, longitude} = pos.coords;
          map.setView([latitude, longitude], 13);
          marker.setLatLng([latitude, longitude]).bindPopup('Your device location').openPopup();
          polyline.addLatLng([latitude, longitude]);
        }, ()=>{ /* ignore */ });
      }
    }
    function mapInvalidate(){ if(map){ map.invalidateSize(); } }
    function simulateMove(){
      if(!map || !marker) return;
      const latlng = marker.getLatLng();
      const dlat = (Math.random()-0.5)/500;
      const dlng = (Math.random()-0.5)/500;
      const next = [latlng.lat+dlat, latlng.lng+dlng];
      marker.setLatLng(next).bindPopup('Simulated move').openPopup();
      polyline.addLatLng(next);
    }
    function flashMarker(){
      if(!marker) return;
      marker.bindPopup('<b>Target focus</b>').openPopup();
      setTimeout(()=> marker.closePopup(), 1000);
    }
    document.getElementById('btnCenterDhaka').addEventListener('click', ()=>{
      if(map){ map.setView([23.777,90.399], 12); marker.setLatLng([23.777,90.399]).bindPopup('Centered: Dhaka').openPopup(); polyline.setLatLngs([[23.777,90.399]]); }
    });
    document.getElementById('btnSimMove').addEventListener('click', ()=> simulateMove());

    // ====== CDR ======
    const cdrDataBase = [
      {time:"2025-08-16 02:14", from:"+8801712345678", to:"+8801999888877", dur:"02:16", type:"OUT", risk:"HIGH"},
      {time:"2025-08-16 02:45", from:"+8801711002200", to:"+8801811223344", dur:"00:46", type:"IN",  risk:"MEDIUM"},
      {time:"2025-08-16 03:05", from:"+8801755512345", to:"+8801811223344", dur:"03:10", type:"OUT", risk:"LOW"},
      {time:"2025-08-16 03:40", from:"+8801811223344", to:"+8801712345678", dur:"00:32", type:"OUT", risk:"HIGH"},
      {time:"2025-08-16 04:01", from:"+8801999888877", to:"+8801712345678", dur:"01:02", type:"IN",  risk:"LOW"}
    ];
    const cdrBody = document.querySelector('#cdrTable tbody');
    function renderCDR(rows){
      cdrBody.innerHTML = "";
      rows.forEach(r=>{
        const tr = document.createElement('tr');
        tr.innerHTML = `<td>${r.time}</td><td>${r.from}</td><td>${r.to}</td><td>${r.dur}</td><td>${r.type}</td><td>${r.risk}</td>`;
        cdrBody.appendChild(tr);
      });
    }
    document.getElementById('btnLoadCDR').addEventListener('click', ()=>{
      renderCDR(cdrDataBase);
      logAudit("CDR loaded ("+cdrDataBase.length+")");
    });
    document.getElementById('btnFilterHigh').addEventListener('click', ()=>{
      renderCDR(cdrDataBase.filter(x=>x.risk==="HIGH"));
      logAudit("CDR filter: HIGH");
    });
    document.getElementById('btnResetCDR').addEventListener('click', ()=>{
      cdrBody.innerHTML=""; logAudit("CDR reset");
    });

    // ====== Profiles ======
    const profileBody = document.querySelector('#profileTable tbody');
    function renderProfiles(rows){
      profileBody.innerHTML = "";
      rows.forEach(p=>{
        const tr = document.createElement('tr');
        const tagHtml = p.tags.map(t=>`<span class="pill" style="margin-right:6px">${t}</span>`).join("");
        tr.innerHTML = `<td>${p.name}</td><td>${p.number}</td><td>${tagHtml}</td><td>${p.risk}</td><td>${p.notes}</td>`;
        profileBody.appendChild(tr);
      });
    }
    function profileSearch(){
      const q = (document.getElementById('profileQuery').value||'').toLowerCase();
      if(!q) return renderProfiles(peopleDB);
      const rows = peopleDB.filter(p =>
        p.name.toLowerCase().includes(q) ||
        p.number.includes(q) ||
        p.tags.some(t=>t.includes(q)) ||
        (p.notes||'').toLowerCase().includes(q)
      );
      renderProfiles(rows);
      logAudit("Profiles search: '"+q+"' ("+rows.length+")");
    }
    document.getElementById('btnProfileSearch').addEventListener('click', profileSearch);
    document.getElementById('btnProfileReset').addEventListener('click', ()=>{ document.getElementById('profileQuery').value=""; renderProfiles(peopleDB); });

    // ====== Admin ======
    const reqBody = document.querySelector('#reqTable tbody');
    let reqQueue = [
      {org:"Metro Police", user:"Officer A", role:"Analyst"},
      {org:"Border Unit", user:"Agent B", role:"Field"},
      {org:"Airport Sec", user:"Ops C", role:"Analyst"}
    ];
    function renderReqs(){
      reqBody.innerHTML="";
      reqQueue.forEach((r,idx)=>{
        const tr = document.createElement('tr');
        tr.innerHTML = `<td>${r.org}</td><td>${r.user}</td><td>${r.role}</td>
                        <td>
                          <button class="btn small ok" onclick="approveReq(${idx})">Approve</button>
                          <button class="btn small danger" onclick="rejectReq(${idx})">Reject</button>
                        </td>`;
        reqBody.appendChild(tr);
      });
    }
    window.approveReq = (i)=>{ const r=reqQueue.splice(i,1)[0]; renderReqs(); logAudit("Approved: "+r.user+" @ "+r.org); };
    window.rejectReq  = (i)=>{ const r=reqQueue.splice(i,1)[0]; renderReqs(); logAudit("Rejected: "+r.user+" @ "+r.org); };

    document.getElementById('btnAssign').addEventListener('click', ()=>{
      const name=document.getElementById('assignName').value.trim();
      const id=document.getElementById('assignId').value.trim();
      const note=document.getElementById('assignNote').value.trim();
      const status=document.getElementById('assignStatus');
      if(!name||!id){ status.textContent="Name/ID required"; status.style.color="#ff9bb0"; return; }
      status.textContent="Assigned to "+name+" ("+id+")"; status.style.color="#9bf9c1";
      logAudit("Assigned ID "+id+" to "+name+"; note="+(note||"-"));
      document.getElementById('assignName').value=""; document.getElementById('assignId').value=""; document.getElementById('assignNote').value="";
    });

    // ====== Alerts (Suspicious Handler) ======
    const alertBox = document.getElementById('alertBox');
    function pushAlert(msg, level="warn"){
      const p=document.createElement('p');
      p.innerHTML = (level==="danger"?"<span class='dangertxt'>[HIGH]</span>":
                     level==="warn"  ?"<span class='warntxt'>[MED]</span>":
                                       "<span class='oktxt'>[LOW]</span>") + " " + msg;
      alertBox.prepend(p);
      logAudit("ALERT "+level.toUpperCase()+": "+msg);
    }
    document.getElementById('btnSimAlert').addEventListener('click', ()=>{
      const pick = Math.random();
      if(pick<0.33) pushAlert("Cross-border burst detected for +8801712345678", "danger");
      else if(pick<0.66) pushAlert("Night-time switching near airport (Handler M)", "warn");
      else pushAlert("Low activity courier ping (Courier Z)", "low");
    });
    document.getElementById('btnClearAlerts').addEventListener('click', ()=> alertBox.innerHTML="");

    // ====== Audit ======
    const auditBox = document.getElementById('auditLog');
    const audit = [];
    function logAudit(line){
      const ts = new Date().toISOString();
      const row = {ts, line};
      audit.unshift(row);
      const p=document.createElement('p'); p.innerHTML = `<span class="status">${ts}</span> — ${line}`;
      auditBox.prepend(p);
    }
    document.getElementById('btnExportAudit').addEventListener('click', ()=>{
      const blob = new Blob([JSON.stringify(audit, null, 2)], {type:"application/json"});
      const url = URL.createObjectURL(blob);
      const a=document.createElement('a');
      a.href=url; a.download="sct_audit_log.json"; a.click();
      URL.revokeObjectURL(url);
    });

    // ====== Initialize after login ======
    function initApp(){
      renderReqs();
      renderProfiles(peopleDB);
      if(!map) bootMap();
      logAudit("Dashboard ready");
    }
  </script>
</body>
</html>
<!doctype html>
<html lang="bn">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Smart Crime Tracker</title>
  <link rel="preconnect" href="https://cdn.jsdelivr.net" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/modern-normalize/modern-normalize.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.css"/>
  <style>
    :root { --bg:#0b1220; --card:#121a2a; --muted:#8aa0bf; --fg:#e9f0ff; --accent:#4f9cff; }
    body{background:var(--bg);color:var(--fg);font-family:ui-sans-serif,system-ui,Segoe UI,Roboto,Helvetica,Arial}
    .wrap{max-width:1200px;margin:auto;padding:16px}
    .grid{display:grid;gap:12px}
    .g2{grid-template-columns:repeat(2,minmax(0,1fr))}
    .g3{grid-template-columns:repeat(3,minmax(0,1fr))}
    .card{background:var(--card);border:1px solid #1e2a40;border-radius:16px;padding:16px;box-shadow:0 6px 24px rgba(0,0,0,.25)}
    input,select,textarea{width:100%;padding:10px 12px;border-radius:10px;border:1px solid #243555;background:#0c1526;color:var(--fg)}
    label{font-size:12px;color:var(--muted)}
    button{background:var(--accent);border:none;color:white;padding:10px 14px;border-radius:12px;cursor:pointer;font-weight:600}
    .ghost{background:#22314e}
    .row{display:flex;gap:8px;align-items:center}
    .right{margin-left:auto}
    .muted{color:var(--muted)}
    .pill{display:inline-block;padding:4px 10px;border-radius:999px;background:#1b2a45;color:#b5cffd;border:1px solid #2a406a}
    .table{width:100%;border-collapse:collapse}
    .table th,.table td{border-bottom:1px solid #22314e;padding:8px 6px;text-align:left}
    .ok{color:#67f09b}.bad{color:#ff7b7b}.warn{color:#ffd479}
    #map{height:320px;border-radius:16px;border:1px solid #22314e}
    .hide{display:none}
    .tag{padding:2px 8px;border-radius:999px;border:1px solid #39507a}
    .footer{text-align:center;margin:18px 0;color:#7e93b8;font-size:12px}
    .logo{font-weight:900;letter-spacing:.5px}
  </style>
</head>
<body>
<div class="wrap">
  <div class="row" style="margin-bottom:12px">
    <div class="logo">🛡️ Smart Crime Tracker</div>
    <div class="right muted" id="envNote">Disconnected</div>
  </div>

  <!-- AUTH -->
  <div id="authView" class="grid g2">
    <div class="card">
      <h3>🔐 সাইন ইন</h3>
      <label>Email</label>
      <input id="loginEmail" placeholder="you@agency.gov.bd" />
      <label>Password</label>
      <input id="loginPass" type="password" placeholder="••••••••" />
      <div class="row" style="margin-top:8px">
        <button id="btnLogin">Sign In</button>
        <button class="ghost" id="btnMagic">Send Magic Link</button>
      </div>
      <div class="muted" style="margin-top:8px" id="authMsg"></div>
    </div>
    <div class="card">
      <h3>🆕 রেজিস্ট্রেশন (এডমিন অনুমোদনসাপেক্ষ)</h3>
      <label>Official Email</label>
      <input id="regEmail" placeholder="you@agency.gov.bd" />
      <label>Password</label>
      <input id="regPass" type="password" />
      <button id="btnRegister">Create Account</button>
      <p class="muted" style="margin-top:8px">সাইনআপ হলে আপনি <span class="tag">PENDING</span> থাকবেন—এডমিন অনুমোদনের পরই লগইন একটিভ হবে।</p>
    </div>
  </div>

  <!-- APP -->
  <div id="appView" class="hide">
    <div class="card row">
      <div>👤 <span id="meEmail" class="pill"></span></div>
      <div class="right row">
        <span id="myRole" class="tag">ROLE</span>
        <button id="btnSignOut">Sign Out</button>
      </div>
    </div>

    <div class="grid g3">
      <div class="card">
        <h3>➕ নতুন কেস</h3>
        <div class="grid">
          <div>
            <label>Case Title</label>
            <input id="caseTitle" placeholder="Gold smuggling ring near border" />
          </div>
          <div>
            <label>Category</label>
            <select id="caseCategory">
              <option>Smuggling</option><option>Corruption</option><option>Fraud</option>
              <option>Narcotics</option><option>Human Trafficking</option>
            </select>
          </div>
          <div>
            <label>Suspect Phone (optional, lawful use only)</label>
            <input id="casePhone" placeholder="+8801XXXXXXXXX" />
          </div>
          <div>
            <label>Geo (lat,lon)</label>
            <input id="caseGeo" placeholder="23.7809,90.2792" />
          </div>
          <div class="grid">
            <label>Details</label>
            <textarea id="caseDetails" rows="4" placeholder="Summary, agencies involved, lawful sources, reference memo no."></textarea>
          </div>
          <div>
            <label>Attach File (image/pdf)</label>
            <input id="fileInput" type="file" accept=".png,.jpg,.jpeg,.pdf" />
          </div>
        </div>
        <div class="row" style="margin-top:8px">
          <button id="btnCreate">Create Case</button>
          <div id="createMsg" class="muted"></div>
        </div>
      </div>

      <div class="card">
        <h3>🔎 সার্চ / ফিল্টার</h3>
        <div class="grid">
          <input id="qText" placeholder="title/phone/category" />
          <select id="qRisk">
            <option value="">Any Risk</option>
            <option>LOW</option><option>MEDIUM</option><option>HIGH</option>
          </select>
        </div>
        <div class="row" style="margin-top:8px">
          <button id="btnSearch">Search</button>
          <button class="ghost" id="btnExport">Export CSV</button>
        </div>
        <div class="muted" id="searchMsg" style="margin-top:8px"></div>
      </div>

      <div class="card">
        <h3>🗺️ ম্যাপ</h3>
        <div id="map"></div>
      </div>
    </div>

    <div class="card">
      <h3>📂 কেস তালিকা</h3>
      <table class="table" id="casesTbl">
        <thead>
          <tr><th>Case</th><th>Category</th><th>Risk</th><th>Geo</th><th>Created</th><th>Actions</th></tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>

    <div class="card">
      <h3>🧾 অডিট লগ</h3>
      <table class="table" id="auditTbl">
        <thead><tr><th>Time</th><th>User</th><th>Action</th><th>Case</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </div>

  <div class="footer">Built lawfully for investigative support. No unauthorized surveillance.</div>
</div>

<script type="module">
  // ====== CONFIG (EDIT THESE) ======================================
  const SUPABASE_URL = "https://YOUR-PROJECT.ref.supabase.co";
  const SUPABASE_ANON_KEY = "YOUR-ANON-KEY";
  // ================================================================

  import { createClient } from "https://esm.sh/@supabase/supabase-js@2.45.4";

  const sb = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {
    auth: { persistSession: true, autoRefreshToken: true }
  });

  const authView = document.getElementById('authView');
  const appView  = document.getElementById('appView');
  const envNote  = document.getElementById('envNote');
  envNote.textContent = (SUPABASE_URL.includes("YOUR-PROJECT")?"⚠️ Configure Supabase":"Connected");

  const meEmail = document.getElementById('meEmail');
  const myRole  = document.getElementById('myRole');

  // Leaflet map
  let map, markersLayer;
  const ensureMap = () => {
    if (map) return;
    map = L.map('map').setView([23.685, 90.3563], 6);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{maxZoom:19}).addTo(map);
    markersLayer = L.layerGroup().addTo(map);
  };

  // Session
  const session = await sb.auth.getSession();
  updateUI(!!session.data.session);
  sb.auth.onAuthStateChange((_e, _s) => updateUI(!!_s));

  function updateUI(loggedIn){
    authView.classList.toggle('hide', loggedIn);
    appView.classList.toggle('hide', !loggedIn);
    if (loggedIn) initApp();
  }

  // Auth handlers
  document.getElementById('btnLogin').onclick = async ()=>{
    const email = document.getElementById('loginEmail').value.trim();
    const pass  = document.getElementById('loginPass').value.trim();
    const { data, error } = await sb.auth.signInWithPassword({ email, password: pass });
    const am = document.getElementById('authMsg');
    if (error) { am.textContent = error.message; return; }
    // enforce approved users only
    const { data: prof } = await sb.from('profiles').select('role,status').eq('user_id', data.user.id).single();
    if (!prof || prof.status!=='APPROVED') {
      await sb.auth.signOut();
      am.textContent = "Account pending approval by admin.";
      return;
    }
  };

  document.getElementById('btnMagic').onclick = async ()=>{
    const email = document.getElementById('loginEmail').value.trim();
    const { error } = await sb.auth.signInWithOtp({ email, options:{ shouldCreateUser:true }});
    document.getElementById('authMsg').textContent = error? error.message : "Magic link sent (check email).";
  };

  document.getElementById('btnRegister').onclick = async ()=>{
    const email = document.getElementById('regEmail').value.trim();
    const pass  = document.getElementById('regPass').value.trim();
    const { data, error } = await sb.auth.signUp({ email, password: pass });
    const am = document.getElementById('authMsg');
    if (error) { am.textContent = error.message; return; }
    // create profile with PENDING
    await sb.from('profiles').insert({ user_id: data.user.id, email, role:'USER', status:'PENDING' });
    am.textContent = "Registered. Wait for admin approval.";
  };

  document.getElementById('btnSignOut').onclick = ()=> sb.auth.signOut();

  // App init
  async function initApp(){
    const u = (await sb.auth.getUser()).data.user;
    meEmail.textContent = u?.email || "unknown";

    const { data: prof } = await sb.from('profiles').select('role,status').eq('user_id', u.id).single();
    myRole.textContent = `${prof?.role||'USER'} | ${prof?.status||'UNKNOWN'}`;

    ensureMap();
    await listCases();
    await listAudit();
  }

  // Create case
  document.getElementById('btnCreate').onclick = async ()=>{
    const title = document.getElementById('caseTitle').value.trim();
    const cat   = document.getElementById('caseCategory').value;
    const phone = document.getElementById('casePhone').value.trim();
    const geo   = document.getElementById('caseGeo').value.trim();
    const details = document.getElementById('caseDetails').value.trim();
    const file = document.getElementById('fileInput').files[0];

    if (!title) { msg('createMsg',"Title required"); return; }

    let lat=null, lon=null;
    if (geo){
      const p=geo.split(',').map(x=>x.trim());
      if (p.length===2){ lat=parseFloat(p[0]); lon=parseFloat(p[1]); }
    }

    // naive risk scoring (lawful heuristic)
    const risk = (cat==='Smuggling'||cat==='Human Trafficking')?'HIGH':(cat==='Corruption'?'MEDIUM':'LOW');

    // upload file if any
    let file_url = null;
    if (file){
      const safeName = `${Date.now()}-${file.name}`.replace(/[^a-zA-Z0-9._-]/g,'_');
      const up = await sb.storage.from('attachments').upload(safeName, file, { upsert:false });
      if (up.error){ msg('createMsg', up.error.message); return; }
      const pub = sb.storage.from('attachments').getPublicUrl(safeName);
      file_url = pub.data.publicUrl;
    }

    const { data: row, error } = await sb.from('cases').insert({
      title, category:cat, phone, lat, lon, details, risk, status:'OPEN'
    }).select().single();
    if (error){ msg('createMsg', error.message); return; }

    await sb.from('audit_logs').insert({ action:'CREATE_CASE', case_id: row.id });

    msg('createMsg', "Case created ✅");
    clearForm();
    await listCases();
    await listAudit();
  };

  function clearForm(){
    ['caseTitle','casePhone','caseGeo','caseDetails','fileInput'].forEach(id=>{
      const el=document.getElementById(id);
      if (el.type==='file') el.value=null; else el.value='';
    });
  }

  // Search / Export
  document.getElementById('btnSearch').onclick = ()=> listCases();
  document.getElementById('btnExport').onclick = async ()=>{
    const data = await fetchCases();
    const csv = toCSV(data);
    const blob = new Blob([csv],{type:'text/csv'});
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = `cases-${Date.now()}.csv`;
    a.click();
  };

  async function fetchCases(){
    const q = document.getElementById('qText').value.trim();
    const risk = document.getElementById('qRisk').value;
    let req = sb.from('cases').select('*').order('created_at', { ascending:false }).limit(200);
    if (q){
      // simple OR filters
      req = req.or(`title.ilike.%${q}%,phone.ilike.%${q}%,category.ilike.%${q}%`);
    }
    if (risk) req = req.eq('risk', risk);
    const { data } = await req;
    return data||[];
  }

  async function listCases(){
    const data = await fetchCases();
    const tbody = document.querySelector('#casesTbl tbody');
    tbody.innerHTML = '';
    markersLayer.clearLayers();

    for (const c of data){
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td><strong>${escapeHtml(c.title)}</strong><div class="muted">${escapeHtml(c.details||'')}</div></td>
        <td>${c.category||''}</td>
        <td><span class="${c.risk==='HIGH'?'bad':(c.risk==='MEDIUM'?'warn':'ok')}">${c.risk}</span></td>
        <td>${c.lat!=null?c.lat.toFixed(4):''}, ${c.lon!=null?c.lon.toFixed(4):''}</td>
        <td>${new Date(c.created_at).toLocaleString()}</td>
        <td>
          <button data-id="${c.id}" class="actClose">Close</button>
          <button data-id="${c.id}" class="ghost actNote">Add Note</button>
        </td>
      `;
      tbody.appendChild(tr);

      if (c.lat!=null && c.lon!=null){
        const m = L.marker([c.lat,c.lon]).bindPopup(`<b>${escapeHtml(c.title)}</b><br/>${c.category} | ${c.risk}`);
        markersLayer.addLayer(m);
      }
    }

    // actions
    document.querySelectorAll('.actClose').forEach(btn=>{
      btn.onclick = async ()=>{
        const id = btn.getAttribute('data-id');
        await sb.from('cases').update({ status:'CLOSED' }).eq('id', id);
        await sb.from('audit_logs').insert({ action:'CLOSE_CASE', case_id: id });
        await listCases(); await listAudit();
      };
    });
    document.querySelectorAll('.actNote').forEach(btn=>{
      btn.onclick = async ()=>{
        const id = btn.getAttribute('data-id');
        const note = prompt("Add note (lawful info only)");
        if (!note) return;
        await sb.from('notes').insert({ case_id:id, note });
        await sb.from('audit_logs').insert({ action:'ADD_NOTE', case_id: id });
        alert('Note added');
      };
    });
  }

  async function listAudit(){
    const { data } = await sb.from('audit_logs_view').select('*').order('created_at',{ascending:false}).limit(200);
    const tbody = document.querySelector('#auditTbl tbody');
    tbody.innerHTML='';
    for (const a of (data||[])){
      const tr = document.createElement('tr');
      tr.innerHTML = `<td>${new Date(a.created_at).toLocaleString()}</td>
                      <td>${escapeHtml(a.email||'')}</td>
                      <td>${a.action}</td>
                      <td>${a.case_title||''}</td>`;
      tbody.appendChild(tr);
    }
  }

  // utils
  function msg(id, text){ document.getElementById(id).textContent = text; }
  function toCSV(rows){
    if (!rows.length) return '';
    const cols = Object.keys(rows[0]);
    const esc = v => `"${String(v??'').replace(/"/g,'""')}"`;
    return [cols.join(','), ...rows.map(r=>cols.map(k=>esc(r[k])).join(','))].join('\n');
  }
  function escapeHtml(s){ return String(s).replace(/[&<>"']/g, m=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[m])); }

</script>
<script src="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.js"></script>
</body>
</html>
-- Profiles (role/status)
create table if not exists public.profiles (
  user_id uuid primary key references auth.users(id) on delete cascade,
  email text unique,
  role text not null default 'USER', -- USER | ADMIN
  status text not null default 'PENDING', -- PENDING | APPROVED | SUSPENDED
  created_at timestamp with time zone default now()
);

-- Cases
create table if not exists public.cases (
  id bigserial primary key,
  title text not null,
  category text not null,
  phone text,
  lat double precision,
  lon double precision,
  details text,
  risk text not null default 'LOW', -- LOW | MEDIUM | HIGH
  status text not null default 'OPEN', -- OPEN | CLOSED
  created_by uuid references auth.users(id) default auth.uid(),
  created_at timestamp with time zone default now()
);

-- Notes
create table if not exists public.notes (
  id bigserial primary key,
  case_id bigint references public.cases(id) on delete cascade,
  note text not null,
  created_by uuid references auth.users(id) default auth.uid(),
  created_at timestamp with time zone default now()
);

-- Audit logs
create table if not exists public.audit_logs (
  id bigserial primary key,
  action text not null,
  case_id bigint references public.cases(id),
  actor uuid references auth.users(id) default auth.uid(),
  created_at timestamp with time zone default now()
);

-- Storage bucket for attachments
insert into storage.buckets (id, name, public) values ('attachments','attachments', true)
on conflict (id) do nothing;

-- Helpful view for audit table (join emails & case titles)
create or replace view public.audit_logs_view as
select a.created_at, a.action, p.email, c.title as case_title
from public.audit_logs a
left join public.cases c on c.id = a.case_id
left join public.profiles p on p.user_id = a.actor
order by a.created_at desc;

-- Enable RLS
alter table public.profiles enable row level security;
alter table public.cases enable row level security;
alter table public.notes enable row level security;
alter table public.audit_logs enable row level security;

-- Policies
-- profiles: user can read own, admins can read all; only admins update status/role
create policy "profiles_self_read" on public.profiles
for select using ( auth.uid() = user_id or exists (select 1 from public.profiles ap where ap.user_id=auth.uid() and ap.role='ADMIN') );

create policy "profiles_admin_update" on public.profiles
for update using ( exists (select 1 from public.profiles ap where ap.user_id=auth.uid() and ap.role='ADMIN') );

create policy "profiles_insert_on_signup" on public.profiles
for insert with check ( auth.role() = 'authenticated' );

-- cases: approved users may select/insert/update; anonymous blocked
create policy "cases_select_approved" on public.cases
for select using ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );

create policy "cases_insert_approved" on public.cases
for insert with check ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );

create policy "cases_update_approved" on public.cases
for update using ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );

-- notes similar
create policy "notes_select_approved" on public.notes
for select using ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );

create policy "notes_insert_approved" on public.notes
for insert with check ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );

-- audit logs: read-only for approved; insert by any approved action
create policy "audit_select_approved" on public.audit_logs
for select using ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );

create policy "audit_insert_approved" on public.audit_logs
for insert with check ( exists (select 1 from public.profiles p where p.user_id = auth.uid() and p.status='APPROVED') );
update public.profiles set role='ADMIN', status='APPROVED' where email='your.email@agency.gov.bd';
<!-- কোথাও ভিজিবল জায়গায় বসাও -->
<a class="btn" href="https://github.com/smartcrime-tracker/smartcrime-tracker.github.io/releases/latest/download/SmartCrimeTracker.apk">
  ⬇️ Download Smart Crime Tracker (APK)
</a>
smartcrime-tracker.github.io/
├─ index.html              ← ওয়েব ডেমো/ডাউনলোড বাটন
└─ app/                    ← Flutter Android অ্যাপ
   ├─ lib/
   ├─ android/
   ├─ ios/
   ├─ pubspec.yaml
   └─ ...
   name: Build Android APK

on:
  workflow_dispatch:
  push:
    branches: [ main ]
    paths:
      - 'app/**'
      - '.github/workflows/build-android.yml'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.22.0'

      - name: Flutter pub get
        working-directory: app
        run: flutter pub get

      - name: Build APK (release)
        working-directory: app
        run: flutter build apk --release

      - name: Upload APK to Release
        uses: softprops/action-gh-release@v2
        with:
          tag_name: v${{ github.run_number }}
          name: Smart Crime Tracker Build ${{ github.run_number }}
          files: app/build/app/outputs/flutter-apk/app-release.apk
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          files: app/build/app/outputs/flutter-apk/app-release.apk#SmartCrimeTracker.apk
          https://github.com/smartcrime-tracker/smartcrime-tracker.github.io/releases/latest/download/SmartCrimeTracker.apk
          import 'package:flutter/material.dart';

void main() => runApp(const SCTApp());

class SCTApp extends StatelessWidget {
  const SCTApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Smart Crime Tracker',
      theme: ThemeData.dark(useMaterial3: true),
      home: const LoginScreen(),
    );
  }
}

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});
  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final email = TextEditingController();
  final pass = TextEditingController();
  String? error;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ConstrainedBox(
          constraints: const BoxConstraints(maxWidth: 420),
          child: Card(
            margin: const EdgeInsets.all(16),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: Column(mainAxisSize: MainAxisSize.min, children: [
                const Text('Admin Login', style: TextStyle(fontSize: 20)),
                const SizedBox(height: 8),
                TextField(controller: email, decoration: const InputDecoration(labelText: 'Email')),
                TextField(controller: pass, decoration: const InputDecoration(labelText: 'Password'), obscureText: true),
                const SizedBox(height: 12),
                ElevatedButton(
                  onPressed: () {
                    if (email.text.trim() == 'smartcrimetracker@gmail.com' &&
                        pass.text.trim() == 'AlhamDulillah01') {
                      Navigator.pushReplacement(context, MaterialPageRoute(builder: (_) => const Home()));
                    } else {
                      setState(() => error = 'Wrong email or password');
                    }
                  },
                  child: const Text('Login'),
                ),
                if (error != null) Padding(
                  padding: const EdgeInsets.only(top: 8),
                  child: Text(error!, style: const TextStyle(color: Colors.redAccent)),
                )
              ]),
            ),
          ),
        ),
      ),
    );
  }
}

class Home extends StatefulWidget {
  const Home({super.key});
  @override
  State<Home> createState() => _HomeState();
}

class _HomeState extends State<Home> {
  final msisdn = TextEditingController();
  final demo = const [
    {'number': '+8801712345678', 'name': 'Unknown Handler', 'risk': 'HIGH'},
    {'number': '+8801811223344', 'name': 'Trader A', 'risk': 'MEDIUM'},
    {'number': '+8801999888877', 'name': 'Courier Z', 'risk': 'LOW'},
  ];
  List<Map<String,String>> result = const [];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Smart Crime Tracker')),
      body: Padding(
        padding: const EdgeInsets.all(12),
        child: Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
          const Text('Number Search (Demo)'),
          Row(children: [
            Expanded(child: TextField(controller: msisdn, decoration: const InputDecoration(hintText: '01XXXXXXXXX'))),
            const SizedBox(width: 8),
            ElevatedButton(
              onPressed: () {
                final q = msisdn.text.trim();
                final r = demo.where((x) => x['number']!.endsWith(q.isEmpty ? '' : q.substring(q.length - (q.length>=6?6:q.length)))).toList();
                setState(() => result = r.map((e)=> e.map((k,v)=> MapEntry(k, v))).toList());
              },
              child: const Text('Search'),
            )
          ]),
          const SizedBox(height: 12),
          Expanded(
            child: ListView.builder(
              itemCount: result.length,
              itemBuilder: (_, i) => ListTile(
                title: Text('${result[i]['name']} — ${result[i]['number']}'),
                subtitle: Text('Risk: ${result[i]['risk']}'),
              ),
            ),
          )
        ]),
      ),
    );
  }
}
name: smart_crime_tracker
description: Minimal lawful demo
publish_to: "none"
environment:
  sdk: ">=3.4.0 <4.0.0"
dependencies:
  flutter:
    sdk: flutter
flutter:
  uses-material-design: true
  
