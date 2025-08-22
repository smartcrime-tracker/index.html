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
