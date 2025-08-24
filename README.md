<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>SMART CRIME TRACKER — Full Demo</title>

<!-- Leaflet for map -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<style>
  :root{
    --bg:#070e16; --panel:#0c1c27; --card:#102735; --line:#1a3d4f;
    --text:#e9feff; --muted:#96bec4; --accent:#31d0c6; --ok:#27c084; --warn:#ffd166; --danger:#ff6b6b;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;
    font-family: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    color:var(--text);
    background: radial-gradient(circle at 10% 10%, rgba(255,255,255,0.02), transparent 10%),
                radial-gradient(circle at 80% 30%, rgba(255,255,255,0.02), transparent 10%),
                linear-gradient(180deg,#071119 0%, #071019 50%, #041018 100%);
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
    overflow-y: auto;
  }

  /* star overlay (subtle) */
  body::after{
    content:"";
    position:fixed; inset:0; z-index:-3;
    background-image:
      radial-gradient(1px 1px at 12% 22%, rgba(255,255,255,0.95) 0, transparent 40%),
      radial-gradient(1.6px 1.6px at 30% 60%, rgba(255,255,255,0.85) 0, transparent 40%),
      radial-gradient(1.2px 1.2px at 70% 10%, rgba(255,255,255,0.9) 0, transparent 40%),
      radial-gradient(0.9px 0.9px at 90% 70%, rgba(255,255,255,0.85) 0, transparent 40%);
    opacity:.55;
    pointer-events:none;
    mix-blend-mode:screen;
  }

  /* dynamic background (admin-changeable) */
  #dynamicBg {
    position:fixed; inset:0; z-index:-2;
    background-size:cover; background-position:center; opacity:.22; pointer-events:none;
    filter: blur(0px) saturate(110%);
  }

  header{
    position:sticky; top:0; z-index:40;
    background: linear-gradient(90deg, rgba(7,14,22,0.82), rgba(6,12,18,0.6));
    border-bottom: 1px solid rgba(20,60,70,0.12);
    display:flex; align-items:center; justify-content:space-between;
    padding:10px 14px;
    backdrop-filter: blur(6px);
  }
  .logoRow{display:flex; align-items:center; gap:12px}
  .logoBox{
    width:56px; height:56px; border-radius:12px; overflow:hidden;
    display:grid; place-items:center; background:linear-gradient(180deg,#0b1f27,#07202a);
    border:1px solid rgba(20,60,80,0.18);
    box-shadow: inset 0 0 12px rgba(50,170,180,0.08);
  }
  .brandTitle{font-weight:800; font-size:16px; letter-spacing:0.4px}
  .brandSub{font-size:12px; color:var(--muted)}

  .controls{display:flex; align-items:center; gap:10px}
  .badge{background:#052a31;color:#9fe;border-radius:999px;padding:6px 10px;font-size:13px;border:1px solid rgba(255,255,255,0.03)}
  .adminToggle{display:flex; align-items:center; gap:8px; font-size:13px; color:var(--muted)}

  /* tabs */
  .tabs{display:flex; gap:8px; flex-wrap:wrap; padding:12px; max-width:1100px; margin:6px auto}
  .tabs button{background:rgba(12,36,44,0.6); border:1px solid rgba(30,95,110,0.08); color:#cfe; padding:8px 12px; border-radius:10px; cursor:pointer}
  .tabs button.active{background:var(--accent); color:#042d2c; font-weight:700; border-color:transparent}

  .wrap{max-width:1100px; margin:12px auto; padding:0 14px}
  .panel{display:none; background:var(--panel); border-radius:12px; padding:14px; border:1px solid rgba(20,70,90,0.08); box-shadow: 0 10px 30px rgba(0,0,0,0.45)}
  .panel.show{display:block; animation:fadeIn .28s ease}
  @keyframes fadeIn{from{opacity:0; transform:translateY(6px)} to{opacity:1; transform:none}}

  .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); border-radius:10px; padding:12px; border:1px solid rgba(255,255,255,0.02)}
  .row{display:flex; gap:12px; flex-wrap:wrap}
  .grow{flex:1 1 280px}
  label{display:block; color:var(--muted); font-size:13px; margin-bottom:6px}
  input, select, textarea{width:100%; padding:10px 12px; border-radius:8px; background:#071b22; color:var(--text); border:1px solid rgba(70,130,140,0.06)}
  button.btn{padding:10px 14px; border-radius:10px; border:none; cursor:pointer; font-weight:700; background:var(--accent); color:#032c2a}
  button.btn.secondary{background:transparent; border:1px solid rgba(255,255,255,0.04); color:var(--muted)}
  .hint{font-size:12px; color:var(--muted); margin-top:6px}
  pre{white-space:pre-wrap; background:rgba(255,255,255,0.02); padding:10px; border-radius:8px; border:1px dashed rgba(255,255,255,0.03)}
  #map{height:360px; border-radius:10px; overflow:hidden; border:1px solid rgba(255,255,255,0.03)}

  footer{padding:20px 10px; text-align:center; color:var(--muted); font-size:13px}
  .small{font-size:12px; color:var(--muted)}
  /* responsive */
  @media (max-width:720px){
    .logoBox{width:46px;height:46px}
    .brandTitle{font-size:14px}
  }
</style>
</head>
<body>
  <div id="dynamicBg" aria-hidden="true"></div>

  <header>
    <div class="logoRow">
      <div class="logoBox" title="Smart Crime Tracker logo">
        <!-- Inline SVG logo (mosque+shield + laurel) — looks like the provided logo -->
        <svg id="svgLogo" width="52" height="52" viewBox="0 0 64 64" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="SCT logo">
          <defs>
            <linearGradient id="g1" x1="0" x2="1" y1="0" y2="1">
              <stop offset="0" stop-color="#6dbf93"/>
              <stop offset="1" stop-color="#1f6b48"/>
            </linearGradient>
          </defs>
          <rect rx="10" width="64" height="64" fill="url(#g1)"/>
          <g transform="translate(6,6)" fill="#083b2a">
            <path d="M18 2 C10 2 4 8 4 16 L4 28 L32 28 L32 16 C32 8 26 2 18 2 Z" />
            <path d="M22 10 A4 4 0 1 1 14 10 A4 4 0 0 1 22 10 Z" fill="#1e6b4a"/>
            <rect x="20" y="14" width="2" height="8" rx="1" fill="#083b2a"/>
            <!-- laurel -->
            <path d="M0 20 C4 16 8 18 10 20" stroke="#083b2a" stroke-width="1.8" fill="none"/>
            <path d="M28 20 C24 16 20 18 18 20" stroke="#083b2a" stroke-width="1.8" fill="none"/>
          </g>
        </svg>
      </div>
      <div>
        <div class="brandTitle">SMART CRIME TRACKER</div>
        <div class="brandSub">Sirātul Mustaqīm — Demo</div>
      </div>
    </div>

    <div class="controls">
      <div class="badge" id="userBadge">Guest</div>
      <label class="adminToggle"><input type="checkbox" id="adminToggle" /> Admin</label>
    </div>
  </header>

  <div class="tabs" id="tabs"></div>

  <main class="wrap">
    <!-- Splash (intro) -->
    <section id="tab-splash" class="panel show">
      <div style="text-align:center">
        <h2 style="margin:8px 0 10px">بِسْمِ اللّٰهِ الرَّحْمٰنِ الرَّحِيمِ</h2>
        <div class="card small" style="max-width:760px;margin:8px auto">
          <div style="text-align:left; line-height:1.6">
            <p><strong>أعوذُ باللهِ من الشيطانِ الرجيم</strong> — আমি আল্লাহর আশ্রয় প্রার্থনা করছি।</p>
            <p><strong>قُلْ هُوَ اللَّهُ أَحَدٌ</strong> — সূরা ইখলাস (১-৪)।</p>
          </div>
        </div>
        <div style="margin-top:12px">
          <button class="btn" onclick="go('tab-number')">Proceed to Demo</button>
          <div class="hint">৩ সেকেন্ড পরে স্বয়ংক্রিয়ভাবে Number Search ট্যাবে যাবে।</div>
        </div>
      </div>
    </section>

    <!-- Login -->
    <section id="tab-login" class="panel">
      <div class="card">
        <h3>Login (Demo)</h3>
        <div class="row">
          <div class="grow">
            <label>Username / Email</label>
            <input id="lgUser" placeholder="admin@local"/>
          </div>
          <div class="grow">
            <label>Password</label>
            <input id="lgPass" type="password" placeholder="admin12345"/>
          </div>
        </div>
        <div style="margin-top:10px">
          <button class="btn" onclick="demoLogin()">Login</button>
          <span class="hint">Demo credentials: <code>admin@local / admin12345</code></span>
        </div>
      </div>
    </section>

    <!-- Number Search (main) -->
    <section id="tab-number" class="panel">
      <div class="card">
        <h3>Mobile Number Search</h3>
        <div class="row">
          <div class="grow">
            <label>MSISDN (e.g., 8801XXXXXXXXX)</label>
            <input id="msisdn" placeholder="8801XXXXXXXXX" />
          </div>
          <div style="align-self:end">
            <button class="btn" onclick="searchNumber()">Search</button>
          </div>
        </div>

        <div class="row" style="margin-top:12px">
          <div class="card grow">
            <h4>Result</h4>
            <pre id="numberOut">Ready.</pre>
          </div>

          <div class="card grow">
            <h4>Suspicious Filter</h4>
            <label>Keywords (comma separated)</label>
            <input id="filterKeys" placeholder="gold, border, hawala" />
            <div style="margin-top:8px"><button class="btn secondary" onclick="applyFilter()">Apply Filter</button></div>
            <pre id="filterOut">No filter applied.</pre>
          </div>
        </div>
      </div>
    </section>

    <!-- CDR -->
    <section id="tab-cdr" class="panel">
      <div class="card">
        <h3>Call Detail Records (CDR) — Demo</h3>
        <div class="row">
          <div class="grow"><label>MSISDN</label><input id="cdrMsisdn" placeholder="8801XXXXXXXXX" /></div>
          <div class="grow"><label>Days</label><select id="cdrDays"><option>7</option><option>15</option><option selected>30</option><option>90</option></select></div>
          <div style="align-self:end"><button class="btn" onclick="fetchCDR()">Fetch CDR</button></div>
        </div>

        <div class="row" style="margin-top:12px">
          <div class="card grow"><h4>Summary</h4><pre id="cdrSummary">—</pre></div>
          <div class="card grow"><h4>Records</h4><pre id="cdrOut">Waiting…</pre></div>
        </div>

        <div class="card" style="margin-top:12px">
          <h4>Call Recording (Demo)</h4>
          <div class="row">
            <div class="grow">
              <button class="btn secondary" onclick="playBeep()">Play Demo Tone</button>
              <div class="hint">Demo: plays a short tone as placeholder for call recording playback</div>
            </div>
            <div class="grow">
              <label>Source</label>
              <select id="recSrc"><option>Telephony</option><option>WhatsApp</option><option>Imo</option><option>Telegram</option></select>
            </div>
          </div>
          <pre id="recOut">No playback.</pre>
        </div>

      </div>
    </section>

    <!-- Location -->
    <section id="tab-map" class="panel">
      <div class="card">
        <h3>Live / Last Known Location (Demo)</h3>
        <div class="row">
          <div class="grow"><label>MSISDN</label><input id="locMsisdn" placeholder="8801XXXXXXXXX"/></div>
          <div style="align-self:end"><button class="btn" onclick="loadLocation()">Locate</button></div>
        </div>

        <div id="map" style="margin-top:12px"></div>
        <pre id="mapOut">No location loaded.</pre>
      </div>
    </section>

    <!-- Profile -->
    <section id="tab-profile" class="panel">
      <div class="card">
        <h3>Criminal Profile (Demo)</h3>
        <div class="row">
          <div class="grow"><label>Name / NID / Internal ID</label><input id="profKey" placeholder="Name / NID / ID"/></div>
          <div style="align-self:end"><button class="btn" onclick="fetchProfile()">Fetch</button></div>
        </div>
        <pre id="profOut">Waiting…</pre>
      </div>
    </section>

    <!-- Alerts -->
    <section id="tab-alerts" class="panel">
      <div class="card">
        <h3>Alerts / Watchlist (Demo)</h3>
        <div class="row">
          <div class="grow"><label>Keyword / MSISDN</label><input id="alertKey" placeholder="gold smuggling / 8801xxxx"/></div>
          <div style="align-self:end"><button class="btn warn" onclick="addAlert()">Add</button></div>
        </div>
        <pre id="alertOut">No alerts.</pre>
      </div>
    </section>

    <!-- Audit -->
    <section id="tab-audit" class="panel">
      <div class="card">
        <h3>Audit Log (Demo)</h3>
        <pre id="auditLog">[]</pre>
      </div>
    </section>

    <!-- Admin -->
    <section id="tab-admin" class="panel">
      <div class="card">
        <h3>Admin — Management (Demo)</h3>
        <div class="row">
          <div class="grow"><h4>Pending Users</h4><pre id="adminUsers">Loading…</pre></div>
          <div class="grow"><h4>Requests</h4><pre id="adminReqs">Loading…</pre></div>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <h3>Branding / Logo & Background</h3>
        <div class="row">
          <div class="grow">
            <label>Upload Logo (PNG/JPG/SVG)</label>
            <input id="logoFile" type="file" accept="image/*" />
            <div class="hint">Upload replaces header logo (stored in browser localStorage on save).</div>
          </div>
          <div class="grow">
            <label>Logo URL (alternative)</label>
            <input id="logoUrl" placeholder="https://example.com/logo.png" />
            <label style="margin-top:8px">Background Image URL</label>
            <input id="bgUrl" placeholder="https://example.com/bg.jpg" />
          </div>
        </div>

        <div style="margin-top:10px" class="row">
          <div style="display:flex; gap:10px; align-items:center">
            <div style="width:68px; height:68px; border-radius:10px; overflow:hidden; border:1px solid rgba(255,255,255,0.03)"><img id="logoPreview" alt="preview" style="width:100%;height:100%;object-fit:cover" /></div>
            <div>
              <button class="btn ok" onclick="saveBranding()">Save</button>
              <button class="btn secondary" onclick="loadBranding()">Reload</button>
              <button class="btn secondary" onclick="resetBranding()">Reset</button>
            </div>
          </div>
        </div>
        <pre id="brandOut">Ready</pre>
      </div>
    </section>

    <footer class="small">Demo build • Replace mock endpoints with secured internal APIs to go live • For official deployment on a .gov.bd server provide files to your IT team.</footer>
  </main>

<script>
/* ================== AUTO PROCEED FROM SPLASH (3s) ================== */
setTimeout(()=>{ 
  // activate second tab (Number)
  const tabButtons = document.querySelectorAll('.tabs button');
  if(tabButtons[1]){ document.querySelectorAll('.tabs button').forEach(b=>b.classList.remove('active')); tabButtons[1].classList.add('active'); }
  go('tab-number'); 
}, 3000);

/* ================== CONFIG & STATE ================== */
const USE_MOCK = true;            // keep true for demo; set false when you have real API
const API_BASE = "https://your-secure-api.example.com";
const API_KEY  = "YOUR_TOKEN";
const BRAND_KEY = "SCT_BRAND_V1";

const AUDIT = []; const ALERTS = [];

/* ================== MOUNT TABS ================== */
const TABS = [
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
(function mountTabs(){
  const box = document.getElementById('tabs');
  TABS.forEach(([id,label],i)=>{
    const b = document.createElement('button');
    b.textContent = label;
    if(i===0) b.classList.add('active');
    b.onclick = ()=>{ document.querySelectorAll('.tabs button').forEach(x=>x.classList.remove('active')); b.classList.add('active'); go(id); };
    box.appendChild(b);
  });
  loadBranding();
})();

function go(id){
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('show'));
  const el = document.getElementById(id);
  if(el) el.classList.add('show');
  if(id==='tab-map') ensureMap();
}

/* ================== LOGIN (demo) ================== */
let SESSION = {logged:false, user:'Guest', role:'guest'};
function demoLogin(){
  const u = document.getElementById('lgUser').value.trim() || 'admin@local';
  const p = document.getElementById('lgPass').value.trim() || 'admin12345';
  if(u==='admin@local' && p==='admin12345'){
    SESSION = {logged:true, user:'Admin (Demo)', role:'admin'};
  } else {
    SESSION = {logged:true, user: u, role:'analyst'};
  }
  document.getElementById('userBadge').textContent = SESSION.user;
  document.getElementById('loginState')?.remove?.(); // optional
  logAudit('login', {by: SESSION.user});
  go('tab-number');
  renderAudit();
}

/* ================== API HELPERS (mock-supported) ================== */
async function apiGet(path){
  if(USE_MOCK) return mockGet(path);
  const res = await fetch(API_BASE + path, { headers: { Authorization: `Bearer ${API_KEY}` }});
  if(!res.ok) throw new Error('API error');
  return res.json();
}
async function apiPost(path, body){
  if(USE_MOCK) return mockPost(path, body);
  const res = await fetch(API_BASE + path, { method:'POST', headers: {'Content-Type':'application/json', Authorization:`Bearer ${API_KEY}`}, body: JSON.stringify(body) });
  if(!res.ok) throw new Error('API error');
  return res.json();
}
function logAudit(action, extra={}){ AUDIT.push(Object.assign({t:new Date().toISOString(), a: action}, extra)); renderAudit(); }

/* ================== NUMBER SEARCH ================== */
async function searchNumber(){
  const msisdn = document.getElementById('msisdn').value.trim();
  const out = document.getElementById('numberOut');
  if(!msisdn){ out.textContent = '⚠️ MSISDN দিন'; return; }
  out.textContent = '⏳ Searching…';
  try{
    const data = await apiGet(`/number?msisdn=${encodeURIComponent(msisdn)}`);
    out.textContent = JSON.stringify(data, null, 2);
    logAudit('number-search', {msisdn});
  }catch(e){ out.textContent = 'API Error: ' + (e.message||e); }
}

/* ========== SUSPICIOUS FILTER ========== */
function applyFilter(){
  const keys = (document.getElementById('filterKeys').value || '').toLowerCase().split(',').map(s=>s.trim()).filter(Boolean);
  const out = document.getElementById('filterOut');
  if(!keys.length){ out.textContent = 'No filter applied.'; return; }
  const score = Math.min(100, keys.length * 18 + (Math.random()*20|0));
  out.textContent = `Keywords: ${keys.join(', ')}\nRisk score: ${score}/100\nFlag: ${score>60 ? 'HIGH' : (score>35 ? 'MEDIUM' : 'LOW')}`;
  logAudit('filter', {keys, score});
}

/* ========== CDR ========== */
async function fetchCDR(){
  const msisdn = document.getElementById('cdrMsisdn').value.trim();
  const days = document.getElementById('cdrDays').value;
  const out = document.getElementById('cdrOut'), sum = document.getElementById('cdrSummary');
  if(!msisdn){ out.textContent='⚠️ MSISDN দিন'; return; }
  out.textContent = '⏳ Loading…';
  try{
    const data = await apiGet(`/cdr?msisdn=${encodeURIComponent(msisdn)}&days=${days}`);
    sum.textContent = JSON.stringify({msisdn, days, total: data.rows.length}, null, 2);
    out.textContent = JSON.stringify(data.rows, null, 2);
    logAudit('cdr', {msisdn, days, total: data.rows.length});
  }catch(e){ out.textContent = 'API Error: ' + (e.message||e); }
}

/* ========== Call Recording demo ========== */
function playBeep(){
  try{
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const o = ctx.createOscillator();
    const g = ctx.createGain();
    o.type = 'sine'; o.frequency.value = 720;
    g.gain.setValueAtTime(0.001, ctx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.3, ctx.currentTime + 0.05);
    g.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + 0.55);
    o.connect(g).connect(ctx.destination); o.start(); o.stop(ctx.currentTime + 0.55);
    document.getElementById('recOut').textContent = 'Playing demo tone...';
  }catch(e){ document.getElementById('recOut').textContent = 'Audio not supported.'; }
}

/* ========== MAP ========== */
let MAP, MARKER;
function ensureMap(){
  if(MAP) return;
  MAP = L.map('map').setView([23.8103, 90.4125], 11);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19, attribution: '© OSM' }).addTo(MAP);
}
async function loadLocation(){
  ensureMap();
  const msisdn = document.getElementById('locMsisdn').value.trim();
  const out = document.getElementById('mapOut');
  if(!msisdn){ out.textContent='⚠️ MSISDN দিন'; return; }
  out.textContent = '⏳ Fetching location…';
  try{
    const d = await apiGet(`/location?msisdn=${encodeURIComponent(msisdn)}`);
    if(MARKER) MAP.removeLayer(MARKER);
    MARKER = L.marker([d.lat, d.lng]).addTo(MAP).bindPopup(`📍 ${d.lat.toFixed(5)}, ${d.lng.toFixed(5)}\n±${d.accuracy}m`).openPopup();
    MAP.setView([d.lat, d.lng], 13);
    out.textContent = JSON.stringify(d, null, 2);
    logAudit('location', {msisdn, coords: [d.lat, d.lng]});
  }catch(e){ out.textContent = 'API Error: ' + (e.message||e); }
}

/* ========== Profile ========== */
async function fetchProfile(){
  const q = document.getElementById('profKey').value.trim();
  const out = document.getElementById('profOut');
  if(!q){ out.textContent='⚠️ Query দিন'; return; }
  out.textContent = '⏳ Fetching…';
  try{
    const d = await apiGet(`/profile?q=${encodeURIComponent(q)}`);
    out.textContent = JSON.stringify(d, null, 2);
    logAudit('profile', {q});
  }catch(e){ out.textContent = 'API Error: ' + (e.message||e); }
}

/* ========== Alerts / Audit ========== */
function addAlert(){
  const k = document.getElementById('alertKey').value.trim();
  if(!k) return;
  ALERTS.push({t: new Date().toISOString(), key: k});
  document.getElementById('alertOut').textContent = JSON.stringify(ALERTS, null, 2);
  logAudit('alert-add', {key: k});
}
function renderAudit(){ document.getElementById('auditLog').textContent = JSON.stringify(AUDIT.slice(-40), null, 2); }

/* ========== Admin load (demo) ========== */
async function loadAdmin(){
  try{
    const users = await apiGet('/admin/users');
    const reqs = await apiGet('/requests');
    document.getElementById('adminUsers').textContent = JSON.stringify(users, null, 2);
    document.getElementById('adminReqs').textContent = JSON.stringify(reqs, null, 2);
    logAudit('admin-load', {users: users.length, reqs: reqs.length});
  }catch(e){
    document.getElementById('adminUsers').textContent = 'API error';
    document.getElementById('adminReqs').textContent = 'API error';
  }
}

/* ========== Branding: logo/background saved in localStorage ========== */
function loadBranding(){
  const saved = JSON.parse(localStorage.getItem(BRAND_KEY) || '{}');
  const logo = saved.logoDataUrl || saved.logoUrl || '';
  const bg = saved.bgUrl || '';
  const logoImg = document.getElementById('logoPreview');
  const headerLogo = document.getElementById('svgLogo');
  if(logo){
    // if dataURL or remote url: set preview and header img replacement
    document.getElementById('logoPreview').src = logo;
    // replace the svg container with an <img> to preserve aspect
    // but keep fallback to SVG. We'll create an <img> if not exists.
    let imgHeader = document.getElementById('headerLogoImg');
    if(!imgHeader){
      imgHeader = document.createElement('img');
      imgHeader.id = 'headerLogoImg';
      imgHeader.style.width = '100%';
      imgHeader.style.height = '100%';
      imgHeader.style.objectFit = 'cover';
      const box = document.querySelector('.logoBox');
      box.appendChild(imgHeader);
      // hide embedded svg
      document.getElementById('svgLogo').style.display = 'none';
    }
    imgHeader.src = logo;
  } else {
    // remove header img if present
    const imgHeader = document.getElementById('headerLogoImg');
    if(imgHeader) imgHeader.remove();
    document.getElementById('svgLogo').style.display = 'block';
    document.getElementById('logoPreview').src = '';
  }
  // background
  const bgEl = document.getElementById('dynamicBg');
  if(bg){
    bgEl.style.backgroundImage = `url('${bg}')`;
    document.getElementById('bgUrl').value = bg;
  } else {
    bgEl.style.backgroundImage = '';
    document.getElementById('bgUrl').value = '';
  }
  document.getElementById('brandOut').textContent = JSON.stringify(saved||{}, null, 2);
}
function fileToDataURL(file){ return new Promise((res, rej)=>{ const r = new FileReader(); r.onload = ()=>res(r.result); r.onerror = rej; r.readAsDataURL(file); }); }
document.getElementById('logoFile')?.addEventListener('change', async (e)=>{
  const f = e.target.files?.[0]; if(!f) return;
  const d = await fileToDataURL(f);
  document.getElementById('logoPreview').src = d;
});
async function saveBranding(){
  const fileEl = document.getElementById('logoFile');
  const urlEl = document.getElementById('logoUrl');
  const bgEl = document.getElementById('bgUrl');
  let logoDataUrl = null, logoUrl = (urlEl.value||'').trim();
  if(fileEl.files && fileEl.files[0]) logoDataUrl = await fileToDataURL(fileEl.files[0]);
  const payload = { logoDataUrl, logoUrl, bgUrl: (bgEl.value||'').trim(), ts: new Date().toISOString() };
  localStorage.setItem(BRAND_KEY, JSON.stringify(payload));
  loadBranding();
  document.getElementById('brandOut').textContent = '✅ Saved to browser storage';
}
function resetBranding(){ localStorage.removeItem(BRAND_KEY); document.getElementById('logoFile').value=''; document.getElementById('logoUrl').value=''; document.getElementById('bgUrl').value=''; loadBranding(); document.getElementById('brandOut').textContent = '↺ Reset to defaults'; }

/* ========== MOCK SERVER (returns demo JSON) ========== */
function mockDelay(ms=250){ return new Promise(r=>setTimeout(r,ms)); }
async function mockGet(path){
  await mockDelay();
  if(path.startsWith('/number')){
    const params = new URL('https://x'+path).searchParams;
    const msisdn = params.get('msisdn');
    return { msisdn, status:'ok', carrier:'DemoTel', imsi:'470-01-123456789', circle:'Sylhet', tags:['watchlist','gold-smuggling'], lastSeen: new Date(Date.now()-3600e3).toISOString(), note:'Demo record' };
  }
  if(path.startsWith('/cdr')){
    const rows = Array.from({length:12}, (_,i)=>({date:`2025-08-${10+i}`, time:`${9+i}:2${i}`, type: i%3 ? 'OUT' : 'IN', to:'+88017'+(10000000+i), duration: 45+i*6, cell:`LAC-12 CID-${200+i}`}));
    return { rows, total: rows.length };
  }
  if(path.startsWith('/location')){
    const pts = [[24.8949,91.8687],[23.8103,90.4125],[22.3569,91.7832],[24.9036,91.8736]];
    const p = pts[Math.floor(Math.random()*pts.length)];
    return { lat: p[0], lng: p[1], accuracy: 45, ts: new Date().toISOString() };
  }
  if(path.startsWith('/profile')){
    return { name:'Md. Demo', nid:'1234567890', risk:'HIGH', associates: ['88017xxxxxxx','88018xxxxxxx'], notes:'Watchlist: demo' };
  }
  if(path.startsWith('/admin/users')){
    return [{id:'u1', name:'Analyst A', status:'pending'}, {id:'u2', name:'Field B', status:'active'}];
  }
  if(path.startsWith('/requests')){
    return [{id:'r1', type:'CDR', msisdn:'8801xxxxxx', status:'open'}];
  }
  return { ok:true };
}
async function mockPost(path, body){ await mockDelay(); return { ok:true, path, body }; }

/* ================== API wrapper ================ */
async function apiGet(path){
  if(USE_MOCK) return mockGet(path);
  return fetch(API_BASE + path, { headers:{ Authorization: `Bearer ${API_KEY}` } }).then(r=>r.json());
}

/* ================== initial states =============== */
loadBranding();
renderAudit();

/* optional: prefill some demo inputs for convenience */
document.getElementById('msisdn').value = '8801760000000';
document.getElementById('cdrMsisdn').value = '8801760000000';
document.getElementById('locMsisdn').value = '8801760000000';
document.getElementById('profKey').value = 'Md Demo';

</script>
</body>
</html>
