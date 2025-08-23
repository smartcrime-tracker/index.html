<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Smart Crime Tracker — Demo (Single File)</title>

<!-- Leaflet (Map demo) -->
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<style>
  :root{
    --bg:#0e1b22; --panel:#0f2732; --card:#122f3d; --accent:#31d0c6;
    --text:#dff7f6; --muted:#8fb5b2; --warn:#ffca61; --danger:#ff6b6b;
    --ok:#2bc480;
  }
  *{box-sizing:border-box} body{margin:0;font-family:ui-sans-serif,system-ui,Segoe UI,Roboto;
    color:var(--text); background:radial-gradient(#0a1620,#0a1218) fixed;}
  header{position:sticky;top:0;background:rgba(10,20,28,.85);backdrop-filter:blur(6px);
    border-bottom:1px solid #123; padding:10px 14px; z-index:20}
  h1{margin:0;font-size:18px; letter-spacing:.4px}
  .wrap{max-width:1100px;margin:20px auto;padding:0 12px}
  .tabs{display:flex; gap:8px; flex-wrap:wrap; margin:14px 0}
  .tabs button{background:#163241;border:1px solid #1d4859;color:#cfe; padding:10px 14px;
    border-radius:10px; cursor:pointer}
  .tabs button.active{background:var(--accent); color:#042b2b; border-color:transparent; font-weight:700}
  .panel{display:none;background:var(--panel); border:1px solid #173746; border-radius:14px;
    padding:14px; box-shadow:0 10px 30px rgba(0,0,0,.25)}
  .panel.show{display:block}
  .card{background:var(--card); border:1px solid #1a4050; border-radius:12px; padding:12px; margin:10px 0}
  .row{display:flex; gap:12px; flex-wrap:wrap}
  .grow{flex:1 1 280px}
  label{display:block; color:var(--muted); font-size:13px; margin-bottom:6px}
  input,select,textarea{width:100%; padding:10px 12px; border-radius:10px; border:1px solid #255162;
    background:#0e2430;color:#eaffff; outline:none}
  textarea{min-height:90px}
  .btn{display:inline-flex; align-items:center; gap:8px; background:var(--accent); color:#041e1e;
    border:none; padding:10px 14px; border-radius:10px; font-weight:700; cursor:pointer}
  .btn.secondary{background:#214655; color:#cfe}
  .btn.warn{background:var(--warn); color:#281e00}
  .btn.ok{background:var(--ok); color:#041e1e}
  .btn.danger{background:var(--danger); color:#2b0505}
  pre{white-space:pre-wrap;background:#0a1f29;border:1px dashed #204758;border-radius:12px;padding:10px}
  .hint{color:var(--muted); font-size:12px}
  .flex-between{display:flex; align-items:center; justify-content:space-between}
  .right{display:flex; gap:8px; align-items:center}
  .badge{display:inline-block;padding:4px 8px;border-radius:999px;background:#08222b;border:1px solid #1c4b5b;color:#9ad}
  .switch{display:flex;gap:8px;align-items:center}
  .switch input{width:auto}
  #map{height:360px;border-radius:12px;border:1px solid #1a4050}
  .splash{background:linear-gradient(180deg,#0b1721,transparent), url('https://images.unsplash.com/photo-1444703686981-a3abbc4d4fe3?q=80&w=1400&auto=format&fit=crop') center/cover no-repeat;
    padding:28px;border-radius:12px}
  .dua{font-size:15px;line-height:1.8}
  .no-skip{user-select:none}
  .footer{color:#7aa; font-size:12px; text-align:center; padding:20px}
</style>
</head>
<body>

<header class="flex-between">
  <h1>🚔 Smart Crime Tracker — Demo</h1>
  <div class="right">
    <span id="loginInfo" class="badge">Guest</span>
    <label class="switch"><input type="checkbox" id="adminToggle" onchange="toggleAdmin()"/> <span>Admin mode</span></label>
  </div>
</header>

<div class="wrap">

  <!-- Splash / Bismillah (no-skip) -->
  <section id="tab-splash" class="panel show">
    <div class="splash no-skip">
      <h2>بِسْمِ اللّٰهِ الرَّحْمٰنِ الرَّحِيمِ</h2>
      <div class="dua card">
        <div>أعوذُ باللهِ منَ الشَّيطانِ الرَّجيمِ — <b>আমি আল্লাহর আশ্রয় প্রার্থনা করছি ধিকৃত শয়তানের কাছ থেকে।</b></div>
        <hr/>
        <div><b>قُلْ هُوَ اللَّهُ أَحَدٌ ...</b> (সূরা ইখলাস – ১-৪)</div>
      </div>
      <button class="btn ok" onclick="go('tab-login')">Proceed</button>
      <div class="hint">এই স্ক্রিনটি স্কিপ করা যাবে না (ডেমো)।</div>
    </div>
  </section>

  <!-- Login -->
  <section id="tab-login" class="panel">
    <div class="card">
      <h3>🔐 Login</h3>
      <div class="row">
        <div class="grow">
          <label>Username / Email</label>
          <input id="lgUser" placeholder="e.g., analyst@agency.gov">
        </div>
        <div class="grow">
          <label>Password</label>
          <input id="lgPass" type="password" placeholder="••••••••">
        </div>
      </div>
      <button class="btn" onclick="demoLogin()">Login</button>
      <div class="hint">ডেমো: user <code>admin@local</code> / pass <code>admin12345</code></div>
    </div>
    <div class="row">
      <div class="card grow">
        <h4>Session</h4>
        <pre id="loginState">Not logged in</pre>
      </div>
      <div class="card grow">
        <h4>Quick Links</h4>
        <button class="btn secondary" onclick="go('tab-number')">Number Search</button>
        <button class="btn secondary" onclick="go('tab-cdr')">CDR</button>
        <button class="btn secondary" onclick="go('tab-map')">Location</button>
        <button class="btn secondary" onclick="go('tab-profile')">Profile</button>
      </div>
    </div>
  </section>

  <!-- Number Search -->
  <section id="tab-number" class="panel">
    <div class="card">
      <h3>📱 Mobile Number Search</h3>
      <div class="row">
        <div class="grow">
          <label>MSISDN (8801XXXXXXXXX)</label>
          <input id="msisdn" placeholder="8801XXXXXXXXX">
        </div>
        <div style="align-self:end">
          <button class="btn" onclick="searchNumber()">Search</button>
        </div>
      </div>
      <pre id="numberOut">Ready.</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/number?msisdn=...</code></div>
    </div>
  </section>

  <!-- CDR -->
  <section id="tab-cdr" class="panel">
    <div class="card">
      <h3>📞 CDR / Call Detail Records</h3>
      <div class="row">
        <div class="grow">
          <label>MSISDN</label>
          <input id="cdrMsisdn" placeholder="8801XXXXXXXXX">
        </div>
        <div class="grow">
          <label>Days</label>
          <select id="cdrDays">
            <option>7</option><option>15</option><option selected>30</option><option>90</option>
          </select>
        </div>
        <div style="align-self:end">
          <button class="btn" onclick="fetchCDR()">Fetch</button>
        </div>
      </div>
      <pre id="cdrOut">Waiting…</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/cdr?msisdn=...&days=...</code></div>
    </div>
  </section>

  <!-- Live Location -->
  <section id="tab-map" class="panel">
    <div class="card">
      <h3>🗺️ Live / Last Location</h3>
      <div class="row">
        <div class="grow">
          <label>MSISDN</label>
          <input id="locMsisdn" placeholder="8801XXXXXXXXX" />
        </div>
        <div style="align-self:end">
          <button class="btn" onclick="loadLocation()">Locate</button>
        </div>
      </div>
      <div id="map" class="card"></div>
      <pre id="mapOut">No location loaded.</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/location?msisdn=...</code></div>
    </div>
  </section>

  <!-- Profile -->
  <section id="tab-profile" class="panel">
    <div class="card">
      <h3>👤 Suspect Profile</h3>
      <div class="row">
        <div class="grow">
          <label>Search by Name / NID / ID</label>
          <input id="profKey" placeholder="Name / NID / Internal ID">
        </div>
        <div style="align-self:end">
          <button class="btn" onclick="fetchProfile()">Fetch</button>
        </div>
      </div>
      <pre id="profOut">Waiting…</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/profile?q=...</code></div>
    </div>
  </section>

  <!-- Alerts -->
  <section id="tab-alerts" class="panel">
    <div class="card">
      <h3>⚠️ Alerts (Demo)</h3>
      <div class="row">
        <div class="grow">
          <label>Create alert (keyword / msisdn)</label>
          <input id="alertKey" placeholder="e.g., gold smuggling / 8801XXXX" />
        </div>
        <div style="align-self:end">
          <button class="btn warn" onclick="addAlert()">Add Alert</button>
        </div>
      </div>
      <pre id="alertOut">No alerts.</pre>
      <div class="hint">API placeholder: <code>POST {API_BASE}/alerts</code> & <code>GET {API_BASE}/alerts</code></div>
    </div>
  </section>

  <!-- Audit Log -->
  <section id="tab-audit" class="panel">
    <div class="card">
      <h3>🧾 Audit Log (Demo)</h3>
      <pre id="auditOut">Empty.</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/audit</code></div>
    </div>
  </section>

  <!-- Admin (optional; hidden unless toggle ON) -->
  <section id="tab-admin" class="panel">
    <div class="card">
      <h3>🛠️ Admin Panel (Demo)</h3>
      <div class="row">
        <div class="grow">
          <label>Pending Users</label>
          <pre id="adminUsers">Loading…</pre>
        </div>
        <div class="grow">
          <label>Requests</label>
          <pre id="adminReqs">Loading…</pre>
        </div>
      </div>
      <div class="hint">API placeholders: <code>/admin/users</code>, <code>/admin/approve</code>, <code>/requests</code></div>
    </div>
  </section>

  <div class="footer">Demo build. Replace placeholders with your secured APIs to go live.</div>
</div>

<script>
/* ==============================
   CONFIG (এক জায়গায় সব)
============================== */
const USE_MOCK = true;          // 🔁 ডেমো চালাতে true রাখুন; লাইভে false
const API_BASE = "https://your-secure-api.example.com"; // 🔗 আপনার API base URL
const API_KEY  = "YOUR_TOKEN";  // 🔐 হেডারে যাবে (লাইভে)
const AUDIT = [];               // সহজ audit buffer

/* ==============================
   TAB / NAV
============================== */
function go(id){
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('show'));
  document.getElementById(id).classList.add('show');
  if(id==='tab-map'){ ensureMap(); }
}
function toggleAdmin(){
  const on = document.getElementById('adminToggle').checked;
  if(on){ go('tab-admin'); loadAdmin(); }
  else{ go('tab-login'); }
}

/* ==============================
   LOGIN (Demo only)
============================== */
let SESSION = { logged:false, user:null, role:"guest" };
function demoLogin(){
  const u = document.getElementById('lgUser').value.trim();
  const p = document.getElementById('lgPass').value.trim();
  if(u==='admin@local' && p==='admin12345'){
    SESSION = {logged:true, user:'Admin (Demo)', role:'admin'};
  }else{
    SESSION = {logged:true, user:u||'Analyst (Demo)', role:'analyst'};
  }
  document.getElementById('loginState').textContent = JSON.stringify(SESSION,null,2);
  document.getElementById('loginInfo').textContent = SESSION.user;
  AUDIT.push({t:new Date().toISOString(), a:'login', by:SESSION.user});
  go('tab-number');
  renderAudit();
}

/* ==============================
   Helpers: fetch wrapper
============================== */
async function apiGet(path){
  if(USE_MOCK) return mockGet(path);
  const res = await fetch(`${API_BASE}${path}`, {headers:{Authorization:`Bearer ${API_KEY}`}});
  if(!res.ok) throw new Error('API error');
  return res.json();
}
async function apiPost(path, body){
  if(USE_MOCK) return mockPost(path, body);
  const res = await fetch(`${API_BASE}${path}`, {
    method:'POST',
    headers:{'Content-Type':'application/json', Authorization:`Bearer ${API_KEY}`},
    body:JSON.stringify(body)
  });
  if(!res.ok) throw new Error('API error');
  return res.json();
}

/* ==============================
   Number Search
============================== */
async function searchNumber(){
  const msisdn = document.getElementById('msisdn').value.trim();
  const out = document.getElementById('numberOut');
  if(!msisdn){ out.textContent='⚠️ Enter MSISDN'; return; }
  out.textContent = '⏳ Searching…';
  const data = await apiGet(`/number?msisdn=${encodeURIComponent(msisdn)}`);
  out.textContent = JSON.stringify(data,null,2);
  AUDIT.push({t:new Date().toISOString(), a:'number-search', msisdn});
  renderAudit();
}

/* ==============================
   CDR
============================== */
async function fetchCDR(){
  const msisdn = document.getElementById('cdrMsisdn').value.trim();
  const days = document.getElementById('cdrDays').value;
  const out = document.getElementById('cdrOut');
  if(!msisdn){ out.textContent='⚠️ Enter MSISDN'; return; }
  out.textContent='⏳ Loading CDR…';
  const data = await apiGet(`/cdr?msisdn=${encodeURIComponent(msisdn)}&days=${days}`);
  out.textContent = JSON.stringify(data,null,2);
  AUDIT.push({t:new Date().toISOString(), a:'cdr', msisdn, days});
  renderAudit();
}

/* ==============================
   Location (Leaflet)
============================== */
let MAP, MARKER;
function ensureMap(){
  if(MAP) return;
  MAP = L.map('map').setView([23.8103,90.4125], 11);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19, attribution:'© OSM'
  }).addTo(MAP);
}
async function loadLocation(){
  ensureMap();
  const msisdn = document.getElementById('locMsisdn').value.trim();
  const out = document.getElementById('mapOut');
  if(!msisdn){ out.textContent='⚠️ Enter MSISDN'; return; }
  out.textContent='⏳ Fetching location…';
  const data = await apiGet(`/location?msisdn=${encodeURIComponent(msisdn)}`);
  const {lat,lng,accuracy,ts} = data;
  if(MARKER) MAP.removeLayer(MARKER);
  MARKER = L.marker([lat,lng]).addTo(MAP).bindPopup(`📍 ${lat.toFixed(5)}, ${lng.toFixed(5)}<br/>±${accuracy}m<br/>${ts}`).openPopup();
  MAP.setView([lat,lng], 13);
  out.textContent = JSON.stringify(data,null,2);
  AUDIT.push({t:new Date().toISOString(), a:'location', msisdn});
  renderAudit();
}

/* ==============================
   Profile
============================== */
async function fetchProfile(){
  const q = document.getElementById('profKey').value.trim();
  const out = document.getElementById('profOut');
  if(!q){ out.textContent='⚠️ Enter query'; return; }
  out.textContent='⏳ Fetching…';
  const data = await apiGet(`/profile?q=${encodeURIComponent(q)}`);
  out.textContent = JSON.stringify(data,null,2);
  AUDIT.push({t:new Date().toISOString(), a:'profile', q});
  renderAudit();
}

/* ==============================
   Alerts
============================== */
const ALERTS = [];
function addAlert(){
  const k = document.getElementById('alertKey').value.trim();
  if(!k) return;
  ALERTS.push({t:new Date().toISOString(), key:k});
  document.getElementById('alertOut').textContent = JSON.stringify(ALERTS,null,2);
  AUDIT.push({t:new Date().toISOString(), a:'alert-add', key:k});
  renderAudit();
}

/* ==============================
   Admin (optional)
============================== */
async function loadAdmin(){
  const users = await apiGet('/admin/users');
  const reqs  = await apiGet('/requests');
  document.getElementById('adminUsers').textContent = JSON.stringify(users,null,2);
  document.getElementById('adminReqs').textContent  = JSON.stringify(reqs,null,2);
}

/* ==============================
   Audit render
============================== */
function renderAudit(){
  document.getElementById('auditOut').textContent = JSON.stringify(AUDIT.slice(-20),null,2);
}

/* ==============================
   MOCK DATA (Demo runs anywhere)
============================== */
function mockDelay(ms=400){ return new Promise(r=>setTimeout(r,ms)); }

async function mockGet(path){
  await mockDelay();
  if(path.startsWith('/number')){
    const url = new URL('https://x'+path); const msisdn = url.searchParams.get('msisdn');
    return { msisdn, status:'ok', carrier:'DemoTel', imsi:'470-01-123456789', circle:'Sylhet',
      tags:['watchlist','gold-smuggling'], lastSeen:'2025-08-20T18:35:00Z' };
  }
  if(path.startsWith('/cdr')){
    const url = new URL('https://x'+path);
    const msisdn = url.searchParams.get('msisdn'), days=url.searchParams.get('days');
    const rows = Array.from({length:10},(_,i)=>({
      date:`2025-08-${(i+10)}`, time:`${10+i}:2${i}`, type:i%3? 'OUT':'IN',
      to:'+88017'+(10000000+i), duration: 45+i*7, cell:`LAC-12 CID-${200+i}`
    }));
    return {msisdn, days, total:rows.length, rows};
  }
  if(path.startsWith('/location')){
    const url = new URL('https://x'+path); const msisdn = url.searchParams.get('msisdn');
    const pts = [[24.8949,91.8687],[23.8103,90.4125],[22.3569,91.7832]];
    const p = pts[Math.floor(Math.random()*pts.length)];
    return { msisdn, lat:p[0], lng:p[1], accuracy:35, ts:new Date().toISOString() };
  }
  if(path.startsWith('/profile')){
    const url = new URL('https://x'+path); const q = url.searchParams.get('q');
    return { q, name:'Md X Demo', nid:'1234567890', address:'Sylhet Sadar', risk:'HIGH',
             associates:['88017xxxxxxx','88018xxxxxxx'], notes:'Watchlist: bullion route' };
  }
  if(path.startsWith('/admin/users')){
    return [{id:'u-101', name:'Analyst A', status:'pending'},{id:'u-102', name:'Field B', status:'pending'}];
  }
  if(path.startsWith('/requests')){
    return [{id:'r-1', type:'CDR', msisdn:'8801xxxxxx', by:'Analyst A', status:'open'}];
  }
  return {ok:true};
}
async function mockPost(path, body){ await mockDelay(); return {ok:true, path, body}; }

/* ==============================
   SIMPLE NAV BUTTONS (top tabs)
============================== */
(function mountTabs(){
  const tabs=[
    ['tab-splash','Splash'],
    ['tab-login','Login'],
    ['tab-number','Number'],
    ['tab-cdr','CDR'],
    ['tab-map','Location'],
    ['tab-profile','Profile'],
    ['tab-alerts','Alerts'],
    ['tab-audit','Audit'],
    ['tab-admin','Admin']
  ];
  const bar=document.createElement('div'); bar.className='tabs';
  tabs.forEach(([id,label],i)=>{
    const b=document.createElement('button');
    b.textContent=label; b.onclick=()=>{document.querySelectorAll('.tabs button').forEach(x=>x.classList.remove('active')); b.classList.add('active'); go(id);};
    if(i===0) b.classList.add('active'); bar.appendChild(b);
  });
  document.querySelector('.wrap').insertBefore(bar, document.querySelector('.wrap').firstChild.nextSibling);
})();
</script>
</body>
</html>
