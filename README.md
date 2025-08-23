const USE_MOCK = false;
const API_BASE  = "https://YOUR_PROD_API";
const API_KEY   = "YOUR_TOKEN";
<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1"/>
<title>SMART CRIME TRACKER — Full Demo</title>

<!-- Map (Leaflet) -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<style>
  :root{
    --bg:#070e16; --panel:#0c1c27; --card:#102735; --line:#1a3d4f;
    --text:#e9feff; --muted:#96bec4; --accent:#31d0c6; --ok:#27c084; --warn:#ffd166; --danger:#ff6b6b;
  }
  *{box-sizing:border-box} html,body{height:100%}
  body{margin:0;font-family:ui-sans-serif,system-ui,Segoe UI,Roboto;background:var(--bg);color:var(--text)}
  /* starry bg */
  body::before, body::after{
    content:""; position:fixed; inset:0; z-index:-2;
    background:
      radial-gradient(2px 2px at 20% 30%, #fff8, transparent 40%),
      radial-gradient(1.5px 1.5px at 70% 60%, #fff6, transparent 40%),
      radial-gradient(1.8px 1.8px at 40% 80%, #fff7, transparent 40%),
      radial-gradient(1.2px 1.2px at 85% 20%, #fff5, transparent 40%),
      linear-gradient(180deg,#08111a 0%, #0a1923 60%, #071018 100%);
    animation: twinkle 12s linear infinite;
  }
  body::after{ filter:blur(1px); opacity:.7 }
  @keyframes twinkle{0%{opacity:.9}50%{opacity:.7}100%{opacity:.9}}

  header{position:sticky;top:0;z-index:10;background:rgba(7,14,22,.8);backdrop-filter:blur(6px);
    border-bottom:1px solid #0f2b37; padding:10px 14px}
  .flex{display:flex;align-items:center;gap:10px}
  .space{justify-content:space-between}
  .logo{display:flex;align-items:center;gap:10px}
  .logoMark{width:34px;height:34px;border-radius:50%;background:
    radial-gradient(circle at 30% 30%, #44f5ff 0 20%, #198a95 21% 60%, #0b3e47 61% 100%);
    box-shadow:0 0 10px #31d0c6aa inset, 0 0 16px #31d0c655}
  .brand{font-weight:800;letter-spacing:.5px}
  .tabs{display:flex;flex-wrap:wrap;gap:8px;margin:10px auto;max-width:1100px;padding:0 10px}
  .tabs button{background:#123646;border:1px solid var(--line);color:#daf; padding:9px 12px;border-radius:10px;cursor:pointer}
  .tabs button.active{background:var(--accent);color:#032c2a;border-color:transparent;font-weight:800}
  .wrap{max-width:1100px;margin:14px auto;padding:0 12px}
  .panel{display:none;background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:14px;box-shadow:0 10px 26px #00000040}
  .panel.show{display:block}
  .card{background:var(--card);border:1px solid var(--line);border-radius:12px;padding:12px;margin:10px 0}
  .row{display:flex;gap:12px;flex-wrap:wrap}
  .grow{flex:1 1 280px}
  label{display:block;color:var(--muted);font-size:13px;margin-bottom:6px}
  input,select,textarea{width:100%;padding:10px 12px;border-radius:10px;border:1px solid #244e61;background:#0b1d27;color:#e9feff}
  textarea{min-height:90px}
  .btn{display:inline-flex;align-items:center;gap:8px;background:var(--accent);color:#04201f;border:none;padding:10px 14px;border-radius:10px;font-weight:800;cursor:pointer}
  .btn.secondary{background:#1a3f4d;color:#cff}
  .btn.ok{background:var(--ok);color:#052018}
  .btn.warn{background:var(--warn);color:#241a00}
  .btn.danger{background:var(--danger);color:#2a0000}
  pre{white-space:pre-wrap;background:#081b23;border:1px dashed #235165;border-radius:12px;padding:10px}
  .hint{color:var(--muted);font-size:12px}
  .badge{display:inline-block;padding:4px 8px;border-radius:999px;background:#0a2230;border:1px solid #1e5063;color:#9fd}
  .switch{display:flex;align-items:center;gap:8px}
  #map{height:360px;border-radius:12px;border:1px solid var(--line)}
  .splash{padding:26px;border-radius:12px;background:
    linear-gradient(180deg,#071119 0%, transparent 50%, #071019 100%)}
  .dua{font-size:16px;line-height:1.9}
  .noskip{user-select:none}
  .footer{color:#8fb5b2;text-align:center;font-size:12px;padding:20px}
</style>
</head>
<body>

<header class="flex space">
  <div class="logo">
    <div class="logoMark" aria-hidden="true"></div>
    <div>
      <div class="brand">SMART CRIME TRACKER</div>
      <div style="font-size:12px;color:#9bd">“Sirātul Mustaqīm” Initiative</div>
    </div>
  </div>
  <div class="flex">
    <span id="loginInfo" class="badge">Guest</span>
    <label class="switch"><input type="checkbox" id="adminToggle" onchange="toggleAdmin()"> Admin</label>
  </div>
</header>

<!-- dynamic tab buttons will be injected here -->
<div class="tabs" id="tabs"></div>

<div class="wrap">
  <!-- Splash / Islamic Opening (no-skip) -->
  <section id="tab-splash" class="panel show">
    <div class="splash noskip">
      <h2 style="margin:6px 0 12px 0;">بِسْمِ اللّٰهِ الرَّحْمٰنِ الرَّحِيمِ</h2>
      <div class="card dua">
        <div><b>أعوذُ باللهِ منَ الشَّيطانِ الرَّجيمِ</b> — আমি ধিকৃত শয়তান হতে আল্লাহর নিকট আশ্রয় প্রার্থনা করছি।</div>
        <hr/>
        <div><b>قُلْ هُوَ اللَّهُ أَحَدٌ ...</b> (সূরা ইখলাস ১-৪)</div>
      </div>
      <button class="btn ok" onclick="go('tab-login')">Proceed</button>
      <div class="hint">এই স্ক্রিনটি স্কিপ করা যাবে না (ডেমো)</div>
    </div>
  </section>

  <!-- Login -->
  <section id="tab-login" class="panel">
    <div class="card">
      <h3>🔐 Login</h3>
      <div class="row">
        <div class="grow"><label>Email / Username</label><input id="lgUser" placeholder="admin@local"/></div>
        <div class="grow"><label>Password</label><input id="lgPass" type="password" placeholder="admin12345"/></div>
      </div>
      <button class="btn" onclick="demoLogin()">Login</button>
      <div class="hint">ডেমো: <code>admin@local</code> / <code>admin12345</code></div>
      <pre id="loginState" class="card">Not logged in</pre>
    </div>
    <div class="row">
      <div class="card grow">
        <h4>Quick Access</h4>
        <button class="btn secondary" onclick="go('tab-number')">Number</button>
        <button class="btn secondary" onclick="go('tab-cdr')">CDR</button>
        <button class="btn secondary" onclick="go('tab-map')">Location</button>
        <button class="btn secondary" onclick="go('tab-profile')">Profile</button>
      </div>
      <div class="card grow">
        <h4>Audit (last 10)</h4>
        <pre id="auditOut">[]</pre>
      </div>
    </div>
  </section>

  <!-- Number Search -->
  <section id="tab-number" class="panel">
    <div class="card">
      <h3>📱 Number Search</h3>
      <div class="row">
        <div class="grow">
          <label>MSISDN (8801XXXXXXXXX)</label>
          <input id="msisdn" placeholder="8801XXXXXXXXX"/>
        </div>
        <div style="align-self:end"><button class="btn" onclick="searchNumber()">Search</button></div>
      </div>
      <div class="row">
        <div class="card grow">
          <h4>Result</h4>
          <pre id="numberOut">Ready.</pre>
        </div>
        <div class="card grow">
          <h4>Suspicious Filter</h4>
          <div class="row">
            <div class="grow">
              <label>Keywords</label>
              <input id="filterKeys" placeholder="e.g., gold, border, hawala"/>
            </div>
            <div style="align-self:end"><button class="btn warn" onclick="applyFilter()">Apply</button></div>
          </div>
          <pre id="filterOut">No filter applied.</pre>
        </div>
      </div>
      <div class="hint">API placeholder: <code>GET {API_BASE}/number?msisdn=...</code></div>
    </div>
  </section>

  <!-- CDR / Call Logs -->
  <section id="tab-cdr" class="panel">
    <div class="card">
      <h3>📞 CDR / Call Logs (Demo)</h3>
      <div class="row">
        <div class="grow"><label>MSISDN</label><input id="cdrMsisdn" placeholder="8801XXXXXXXXX"/></div>
        <div class="grow"><label>Days</label><select id="cdrDays"><option>7</option><option>15</option><option selected>30</option><option>90</option></select></div>
        <div style="align-self:end"><button class="btn" onclick="fetchCDR()">Fetch</button></div>
      </div>
      <div class="row">
        <div class="card grow"><h4>Summary</h4><pre id="cdrSummary">—</pre></div>
        <div class="card grow"><h4>Records</h4><pre id="cdrOut">Waiting…</pre></div>
      </div>
      <div class="card">
        <h4>🎙️ Call Recording (Demo Source)</h4>
        <div class="row">
          <div class="grow">
            <button class="btn secondary" onclick="playBeep()">Play Demo Tone</button>
            <div class="hint">ডেমো: WebAudio tone—লাইভে এখানে ভয়েস ফাইল/স্ট্রিম বসবে</div>
          </div>
          <div class="grow">
            <label>Source Tag</label>
            <select id="recSrc"><option>Telephony</option><option>WhatsApp</option><option>Imo</option><option>Telegram</option></select>
          </div>
        </div>
        <pre id="recOut">No playback.</pre>
      </div>
      <div class="hint">API placeholders: <code>GET {API_BASE}/cdr</code>, <code>GET {API_BASE}/recording?...</code></div>
    </div>
  </section>

  <!-- Live / Last Location -->
  <section id="tab-map" class="panel">
    <div class="card">
      <h3>🗺️ Location</h3>
      <div class="row">
        <div class="grow"><label>MSISDN</label><input id="locMsisdn" placeholder="8801XXXXXXXXX"/></div>
        <div style="align-self:end"><button class="btn" onclick="loadLocation()">Locate</button></div>
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
        <div class="grow"><label>Search Name/NID/ID</label><input id="profKey" placeholder="Name / NID / ID"/></div>
        <div style="align-self:end"><button class="btn" onclick="fetchProfile()">Fetch</button></div>
      </div>
      <pre id="profOut">Waiting…</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/profile?q=...</code></div>
    </div>
  </section>

  <!-- Alerts -->
  <section id="tab-alerts" class="panel">
    <div class="card">
      <h3>⚠️ Alerts (Watchlist)</h3>
      <div class="row">
        <div class="grow"><label>Keyword / MSISDN</label><input id="alertKey" placeholder="gold smuggling / 8801xxxx"/></div>
        <div style="align-self:end"><button class="btn warn" onclick="addAlert()">Add Alert</button></div>
      </div>
      <pre id="alertOut">No alerts.</pre>
      <div class="hint">API placeholders: <code>POST {API_BASE}/alerts</code>, <code>GET {API_BASE}/alerts</code></div>
    </div>
  </section>

  <!-- Audit -->
  <section id="tab-audit" class="panel">
    <div class="card">
      <h3>🧾 Audit Log</h3>
      <pre id="auditLog">[]</pre>
      <div class="hint">API placeholder: <code>GET {API_BASE}/audit</code></div>
    </div>
  </section>

  <!-- Admin (optional, toggled) -->
  <section id="tab-admin" class="panel">
    <div class="card">
      <h3>🛠️ Admin (Demo)</h3>
      <div class="row">
        <div class="grow"><h4>Pending Users</h4><pre id="adminUsers">Loading…</pre></div>
        <div class="grow"><h4>Requests</h4><pre id="adminReqs">Loading…</pre></div>
      </div>
      <div class="hint">API placeholders: <code>/admin/users</code>, <code>/admin/approve</code></div>
    </div>
  </section>

  <div class="footer">Demo build. Replace placeholders with secured APIs to go live.</div>
</div>

<script>
/* ========= CONFIG (এক জায়গায়) ========= */
const USE_MOCK = true; // ডেমো চালাতে true; লাইভে false
const API_BASE  = "https://your-secure-api.example.com";
const API_KEY   = "YOUR_TOKEN";
const AUDIT     = [];
const ALERTS    = [];

/* ========= Tabs ========= */
const TABS = [
  ["tab-splash","Splash"],
  ["tab-login","Login"],
  ["tab-number","Number"],
  ["tab-cdr","CDR"],
  ["tab-map","Location"],
  ["tab-profile","Profile"],
  ["tab-alerts","Alerts"],
  ["tab-audit","Audit"],
  ["tab-admin","Admin"]
];
(function mountTabs(){
  const box=document.getElementById('tabs');
  TABS.forEach(([id,label],i)=>{
    const b=document.createElement('button');
    b.textContent=label;
    if(i===0) b.classList.add('active');
    b.onclick=()=>{ document.querySelectorAll('.tabs button').forEach(x=>x.classList.remove('active')); b.classList.add('active'); go(id); };
    box.appendChild(b);
  });
})();
function go(id){
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('show'));
  document.getElementById(id).classList.add('show');
  if(id==='tab-map') ensureMap();
}
function toggleAdmin(){
  const on=document.getElementById('adminToggle').checked;
  if(on){ go('tab-admin'); loadAdmin(); }
  else{ go('tab-login'); }
}

/* ========= Login (demo) ========= */
let SESSION={logged:false,user:null,role:"guest"};
function demoLogin(){
  const u=document.getElementById('lgUser').value.trim()||'admin@local';
  const p=document.getElementById('lgPass').value.trim()||'admin12345';
  if(u==='admin@local' && p==='admin12345'){
    SESSION={logged:true,user:'Admin (Demo)',role:'admin'};
  }else{
    SESSION={logged:true,user:u,role:'analyst'};
  }
  document.getElementById('loginState').textContent=JSON.stringify(SESSION,null,2);
  document.getElementById('loginInfo').textContent=SESSION.user;
  logAudit('login',{by:SESSION.user});
  go('tab-number');
  renderAudit();
}

/* ========= Helpers ========= */
async function apiGet(path){
  if(USE_MOCK) return mockGet(path);
  const r=await fetch(`${API_BASE}${path}`,{headers:{Authorization:`Bearer ${API_KEY}`}}); 
  if(!r.ok) throw new Error('API error'); return r.json();
}
async function apiPost(path,body){
  if(USE_MOCK) return mockPost(path,body);
  const r=await fetch(`${API_BASE}${path}`,{method:'POST',headers:{'Content-Type':'application/json',Authorization:`Bearer ${API_KEY}`},body:JSON.stringify(body)}); 
  if(!r.ok) throw new Error('API error'); return r.json();
}
function logAudit(action,extra={}){ AUDIT.push({t:new Date().toISOString(), a:action, ...extra}); }

/* ========= Number ========= */
async function searchNumber(){
  const msisdn=document.getElementById('msisdn').value.trim();
  const out=document.getElementById('numberOut');
  if(!msisdn){ out.textContent='⚠️ MSISDN দিন'; return; }
  out.textContent='⏳ Searching…';
  const data=await apiGet(`/number?msisdn=${encodeURIComponent(msisdn)}`);
  out.textContent=JSON.stringify(data,null,2);
  logAudit('number-search',{msisdn});
  renderAudit();
}
function applyFilter(){
  const keys=(document.getElementById('filterKeys').value||'').toLowerCase().split(',').map(s=>s.trim()).filter(Boolean);
  const out=document.getElementById('filterOut');
  if(!keys.length){ out.textContent='No filter applied.'; return; }
  // Simple demo score:
  const score = Math.min(100, keys.length*18 + (Math.random()*20|0));
  out.textContent=`Keywords: ${keys.join(', ')}\nRisk score: ${score}/100\nFlag: ${score>60?'HIGH':'MEDIUM'}`;
  logAudit('filter',{keys,score});
  renderAudit();
}

/* ========= CDR ========= */
async function fetchCDR(){
  const msisdn=document.getElementById('cdrMsisdn').value.trim();
  const days=document.getElementById('cdrDays').value;
  const out=document.getElementById('cdrOut');
  const sum=document.getElementById('cdrSummary');
  if(!msisdn){ out.textContent='⚠️ MSISDN দিন'; return; }
  out.textContent='⏳ Loading…';
  const data=await apiGet(`/cdr?msisdn=${encodeURIComponent(msisdn)}&days=${days}`);
  sum.textContent=JSON.stringify({msisdn,days,total:data.rows.length},null,2);
  out.textContent=JSON.stringify(data.rows,null,2);
  logAudit('cdr',{msisdn,days,total:data.rows.length});
  renderAudit();
}
/* demo audio tone */
function playBeep(){
  try{
    const ctx=new (window.AudioContext||window.webkitAudioContext)();
    const o=ctx.createOscillator(); const g=ctx.createGain();
    o.type='sine'; o.frequency.value=740; g.gain.setValueAtTime(0.001, ctx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.3, ctx.currentTime+0.05);
    g.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime+0.5);
    o.connect(g).connect(ctx.destination); o.start(); o.stop(ctx.currentTime+0.55);
    document.getElementById('recOut').textContent='Playing demo tone (recording placeholder)…';
  }catch(e){ document.getElementById('recOut').textContent='Audio not supported.'; }
}

/* ========= Location ========= */
let MAP, MARKER;
function ensureMap(){
  if(MAP) return;
  MAP=L.map('map').setView([23.81,90.41],11);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{maxZoom:19,attribution:'© OSM'}).addTo(MAP);
}
async function loadLocation(){
  ensureMap();
  const msisdn=document.getElementById('locMsisdn').value.trim();
  const out=document.getElementById('mapOut');
  if(!msisdn){ out.textContent='⚠️ MSISDN দিন'; return; }
  out.textContent='⏳ Fetching…';
  const d=await apiGet(`/location?msisdn=${encodeURIComponent(msisdn)}`);
  if(MARKER) MAP.removeLayer(MARKER);
  MARKER=L.marker([d.lat,d.lng]).addTo(MAP).bindPopup(`📍 ${d.lat.toFixed(5)}, ${d.lng.toFixed(5)}<br/>±${d.accuracy}m`).openPopup();
  MAP.setView([d.lat,d.lng],13);
  out.textContent=JSON.stringify(d,null,2);
  logAudit('location',{msisdn,coords:[d.lat,d.lng]});
  renderAudit();
}

/* ========= Profile ========= */
async function fetchProfile(){
  const q=document.getElementById('profKey').value.trim();
  const out=document.getElementById('profOut');
  if(!q){ out.textContent='⚠️ Query দিন'; return; }
  out.textContent='⏳ Fetching…';
  const data=await apiGet(`/profile?q=${encodeURIComponent(q)}`);
  out.textContent=JSON.stringify(data,null,2);
  logAudit('profile',{q});
  renderAudit();
}

/* ========= Alerts & Audit ========= */
function addAlert(){
  const k=document.getElementById('alertKey').value.trim();
  if(!k) return;
  ALERTS.push({t:new Date().toISOString(), key:k});
  document.getElementById('alertOut').textContent=JSON.stringify(ALERTS,null,2);
  logAudit('alert-add',{key:k});
  renderAudit();
}
function renderAudit(){
  document.getElementById('auditOut')?.textContent = JSON.stringify(AUDIT.slice(-10),null,2);
  document.getElementById('auditLog')?.textContent = JSON.stringify(AUDIT.slice(-30),null,2);
}

/* ========= Admin (optional) ========= */
async function loadAdmin(){
  const users=await apiGet('/admin/users');
  const reqs =await apiGet('/requests');
  document.getElementById('adminUsers').textContent=JSON.stringify(users,null,2);
  document.getElementById('adminReqs').textContent =JSON.stringify(reqs,null,2);
  logAudit('admin-load',{users:users.length,reqs:reqs.length});
  renderAudit();
}

/* ========= MOCK SERVER (Demo Data) ========= */
function mockDelay(ms=350){ return new Promise(r=>setTimeout(r,ms)); }
async function mockGet(path){
  await mockDelay();
  if(path.startsWith('/number')){
    const u=new URL('https://x'+path); const msisdn=u.searchParams.get('msisdn');
    return { msisdn, status:'ok', carrier:'DemoTel', imsi:'470-01-123456789', circle:'Sylhet',
             tags:['watchlist','gold-smuggling'], lastSeen:new Date(Date.now()-3600e3).toISOString() };
  }
  if(path.startsWith('/cdr')){
    const u=new URL('https://x'+path); const msisdn=u.searchParams.get('msisdn'); const days=u.searchParams.get('days');
    const rows=Array.from({length:12},(_,i)=>({date:`2025-08-${(i+10)}`,time:`${10+i}:2${i}`,type:i%3?'OUT':'IN',
      to:'+88017'+(10000000+i),duration:40+i*7,cell:`LAC-12 CID-${200+i}`}));
    return {msisdn,days,total:rows.length,rows};
  }
  if(path.startsWith('/location')){
    const pts=[[24.8949,91.8687],[23.8103,90.4125],[22.3569,91.7832],[24.9036,91.8736]];
    const p=pts[Math.random()*pts.length|0];
    return {msisdn:'',lat:p[0],lng:p[1],accuracy:35,ts:new Date().toISOString()};
  }
  if(path.startsWith('/profile')){
    const u=new URL('https://x'+path); const q=u.searchParams.get('q');
    return { q, name:'Md X Demo', nid:'1234567890', address:'Sylhet Sadar', risk:'HIGH',
             associates:['88017xxxxxxx','88018xxxxxxx'], notes:'Watchlist: bullion route'};
  }
  if(path.startsWith('/admin/users')){
    return [{id:'u-101',name:'Analyst A',status:'pending'},{id:'u-102',name:'Field B',status:'pending'}];
  }
  if(path.startsWith('/requests')){
    return [{id:'r-1',type:'CDR',msisdn:'8801xxxxxx',by:'Analyst A',status:'open'}];
  }
  return {ok:true};
}
async function mockPost(path,body){ await mockDelay(); return {ok:true,path,body}; }
</script>
</body>
</html>
