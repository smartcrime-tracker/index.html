<!doctype html>
<html lang="bn">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Smart Crime Tracker — Presentation</title>

  <!-- Tailwind via CDN -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Icons -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
  <style>
    :root{
      --brand:#10b981;          /* emerald / brand */
      --brand-2:#22d3ee;        /* cyan accent */
      --glass: rgba(15,23,42,.55);
    }
    html,body{ height:100%; }
    body{ font-family:Inter,system-ui,Segoe UI,Roboto,Arial; }
    .glass{ background:var(--glass); backdrop-filter: blur(10px); }
    .nav-btn{ @apply px-3 py-2 rounded-xl text-sm font-semibold hover:bg-white/10 transition; }
    .tab{ display:none; }
    .tab.active{ display:block; }
    .kbd{ @apply px-2 py-1 rounded bg-slate-800 text-slate-100 text-xs font-semibold; }
    /* Mobile map iframe nicer corners */
    iframe{ border-radius: 12px; }
  </style>
</head>
<body class="min-h-screen text-slate-100"
      style="background: #0b1020 url('bg.jpg') center/cover no-repeat fixed">

  <!-- Opening / Bismillah -->
  <section id="opening" class="min-h-screen flex items-center justify-center p-6">
    <div class="glass max-w-3xl w-full rounded-3xl p-8 md:p-12 shadow-2xl ring-1 ring-white/10">
      <div class="flex items-center gap-4">
        <img src="logo.png" alt="Smart Crime Tracker Logo" class="w-16 h-16 object-contain rounded-xl bg-white/5 p-2">
        <div>
          <h1 class="text-3xl md:text-4xl font-extrabold">SMART CRIME TRACKER</h1>
          <p class="text-sm text-slate-300">Nuqta • National Threat Core Analysis Unit</p>
        </div>
      </div>

      <div class="mt-6 space-y-2 text-center">
        <p class="text-3xl md:text-4xl">﷽</p>
        <p class="text-lg md:text-xl">أعوذُ بالله من الشيطان الرجيم</p>
        <p class="text-lg md:text-xl">بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيْم</p>
        <p class="text-sm text-slate-300 mt-2">“اهدِنَا الصِّرَاطَ المُستَقِيمَ” — আমাদেরকে সরল পথ দেখাও</p>
      </div>

      <div class="mt-6 grid md:grid-cols-3 gap-4">
        <div class="glass rounded-2xl p-4 ring-1 ring-white/10">
          <p class="text-sm text-slate-300">মিশন</p>
          <p class="font-semibold">ন্যায় ও সত্যের পথে অপরাধ দমন</p>
        </div>
        <div class="glass rounded-2xl p-4 ring-1 ring-white/10">
          <p class="text-sm text-slate-300">নীতিমালা</p>
          <p class="font-semibold">কেবল আইনগত/অনুমোদিত ডেটাই ব্যবহার</p>
        </div>
        <div class="glass rounded-2xl p-4 ring-1 ring-white/10">
          <p class="text-sm text-slate-300">স্ট্যাটাস</p>
          <p class="font-semibold">Presentation Demo (API-ready)</p>
        </div>
      </div>

      <div class="mt-8 flex flex-wrap items-center justify-center gap-3">
        <button id="startBtn"
                class="px-6 py-3 rounded-2xl font-bold bg-emerald-500 hover:bg-emerald-400 text-slate-900 shadow-lg">
          শুরু করুন (Enter)
        </button>
        <span class="kbd">Enter</span>
      </div>
    </div>
  </section>

  <!-- App Shell -->
  <section id="app" class="hidden">
    <!-- Top Navbar -->
    <header class="sticky top-0 z-40 glass ring-1 ring-white/10">
      <div class="mx-auto max-w-7xl px-4 py-3 flex items-center justify-between">
        <div class="flex items-center gap-3">
          <img src="logo.png" class="w-9 h-9 rounded-lg bg-white/5 p-1" alt="">
          <span class="font-extrabold tracking-wide">Smart Crime Tracker</span>
        </div>
        <nav class="flex flex-wrap gap-1 text-sm">
          <button data-tab="home"       class="nav-btn text-cyan-300">Home</button>
          <button data-tab="number"     class="nav-btn">Number</button>
          <button data-tab="location"   class="nav-btn">Location</button>
          <button data-tab="cdr"        class="nav-btn">CDR</button>
          <button data-tab="profiles"   class="nav-btn">Profiles</button>
          <button data-tab="evidence"   class="nav-btn">Evidence</button>
          <button data-tab="alerts"     class="nav-btn">Alerts</button>
          <button data-tab="admin"      class="nav-btn">Admin</button>
        </nav>
      </div>
    </header>

    <!-- Content -->
    <main class="mx-auto max-w-7xl p-4 md:p-6 space-y-6">

      <!-- HOME -->
      <section id="tab-home" class="tab active">
        <div class="grid md:grid-cols-2 gap-6">
          <div class="glass rounded-3xl p-6 ring-1 ring-white/10">
            <h2 class="text-xl font-extrabold mb-2">Overview</h2>
            <p class="text-slate-300">
              এটি একটি **প্রেজেন্টেশন ডেমো** — বাস্তব ডেটা/টেলিকম অ্যাক্সেস ছাড়াই UI/Flow বোঝানোর জন্য।
              নিচের যে কোনো ফিচারে API যোগ করতে হলে কোডে দেয়া
              <span class="kbd">/* TODO: connect API */</span> জায়গায় ইন্টিগ্রেশন করলেই হবে।
            </p>
            <ul class="list-disc ml-6 mt-3 text-slate-300 space-y-1">
              <li>Number Intelligence, Live Location (map), CDR Graph & Table</li>
              <li>Criminal Profiles, Evidence Inbox, Risk Alerts</li>
              <li>Admin Approval, Logo/Theme change (client-side)</li>
            </ul>
          </div>

          <div class="glass rounded-3xl p-6 ring-1 ring-white/10">
            <h2 class="text-xl font-extrabold mb-3">Quick Stats (Demo)</h2>
            <div class="grid grid-cols-2 gap-3">
              <div class="rounded-2xl p-4 bg-emerald-500/10 ring-1 ring-emerald-400/30">
                <p class="text-4xl font-extrabold text-emerald-300">128</p>
                <p class="text-slate-300">Profiles Tagged</p>
              </div>
              <div class="rounded-2xl p-4 bg-cyan-500/10 ring-1 ring-cyan-400/30">
                <p class="text-4xl font-extrabold text-cyan-300">42</p>
                <p class="text-slate-300">Active Alerts</p>
              </div>
              <div class="rounded-2xl p-4 bg-fuchsia-500/10 ring-1 ring-fuchsia-400/30">
                <p class="text-4xl font-extrabold text-fuchsia-300">6</p>
                <p class="text-slate-300">Evidence Pend.</p>
              </div>
              <div class="rounded-2xl p-4 bg-amber-500/10 ring-1 ring-amber-400/30">
                <p class="text-4xl font-extrabold text-amber-300">3</p>
                <p class="text-slate-300">Hotzones</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- NUMBER -->
      <section id="tab-number" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">📱 Number Intelligence</h2>
          <div class="flex gap-2 flex-col md:flex-row">
            <input id="msisdn" type="text" inputmode="numeric" placeholder="01XXXXXXXXX বা +8801XXXXXXXXX"
                   class="w-full px-4 py-3 rounded-2xl bg-white/5 ring-1 ring-white/10 focus:ring-emerald-400 outline-none" />
            <button id="btnNumber"
                    class="px-5 py-3 rounded-2xl font-bold bg-emerald-500 hover:bg-emerald-400 text-slate-900">
              Search
            </button>
          </div>
          <div id="numResult" class="grid md:grid-cols-3 gap-3"></div>

          <details class="mt-2">
            <summary class="cursor-pointer text-slate-300">API Hook</summary>
            <pre class="text-xs text-slate-300 overflow-auto p-3 bg-black/30 rounded-xl">
/* TODO: connect API
fetch('YOUR_NUMBER_INTEL_ENDPOINT?msisdn=' + value)
  .then(r => r.json()).then(showNumberIntel);
*/            </pre>
          </details>
        </div>
      </section>

      <!-- LOCATION -->
      <section id="tab-location" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">📍 Live Location (Demo)</h2>
          <p class="text-slate-300">ডেমো ম্যাপ: Default Dhaka marker. API পেলে রিয়াল-টাইম কোঅর্ডিনেট বসবে।</p>
          <iframe
            src="https://www.openstreetmap.org/export/embed.html?bbox=90.35%2C23.70%2C90.50%2C23.82&layer=mapnik&marker=23.76%2C90.40"
            class="w-full" height="360" loading="lazy"></iframe>
        </div>
      </section>

      <!-- CDR -->
      <section id="tab-cdr" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">📞 CDR Analysis (Demo)</h2>
          <div class="overflow-auto rounded-2xl ring-1 ring-white/10">
            <table class="min-w-full text-sm">
              <thead class="bg-white/10">
                <tr>
                  <th class="px-3 py-2 text-left">Caller</th>
                  <th class="px-3 py-2 text-left">Receiver</th>
                  <th class="px-3 py-2">Duration</th>
                  <th class="px-3 py-2">Time</th>
                  <th class="px-3 py-2">Cell</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-white/10">
                <tr>
                  <td class="px-3 py-2">01812-345678</td>
                  <td class="px-3 py-2">01798-765432</td>
                  <td class="px-3 py-2 text-center">03:12</td>
                  <td class="px-3 py-2 text-center">10:30</td>
                  <td class="px-3 py-2 text-center">Gulshan-A</td>
                </tr>
                <tr>
                  <td class="px-3 py-2">01655-555555</td>
                  <td class="px-3 py-2">01944-444444</td>
                  <td class="px-3 py-2 text-center">07:05</td>
                  <td class="px-3 py-2 text-center">11:45</td>
                  <td class="px-3 py-2 text-center">Motijheel-N</td>
                </tr>
              </tbody>
            </table>
          </div>

          <details>
            <summary class="cursor-pointer text-slate-300">API Hook</summary>
            <pre class="text-xs text-slate-300 overflow-auto p-3 bg-black/30 rounded-xl">
/* TODO: connect API
fetch('YOUR_CDR_ENDPOINT?msisdn=...')
  .then(r=>r.json()).then(renderCDRTableAndGraph);
*/            </pre>
          </details>
        </div>
      </section>

      <!-- PROFILES -->
      <section id="tab-profiles" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">👤 Criminal Profiles (Demo)</h2>
          <div class="grid md:grid-cols-3 gap-3">
            <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
              <p class="font-bold">Hasan</p>
              <p class="text-slate-300 text-sm">Smuggling</p>
              <span class="mt-2 inline-block rounded-lg px-2 py-1 text-xs bg-emerald-500/20 ring-1 ring-emerald-400/40 text-emerald-300">Arrested</span>
            </div>
            <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
              <p class="font-bold">Kamal</p>
              <p class="text-slate-300 text-sm">Gold Bar Chain</p>
              <span class="mt-2 inline-block rounded-lg px-2 py-1 text-xs bg-amber-500/20 ring-1 ring-amber-400/40 text-amber-300">Under Watch</span>
            </div>
            <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
              <p class="font-bold">Rafiq</p>
              <p class="text-slate-300 text-sm">Hawala</p>
              <span class="mt-2 inline-block rounded-lg px-2 py-1 text-xs bg-fuchsia-500/20 ring-1 ring-fuchsia-400/40 text-fuchsia-300">Interrogation</span>
            </div>
          </div>
        </div>
      </section>

      <!-- EVIDENCE -->
      <section id="tab-evidence" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">🧾 Evidence Inbox (Demo)</h2>
          <p class="text-slate-300">ফাইল সিলেক্ট করলে প্রেজেন্টেশনে থাম্বনেইল দেখা যাবে (লোকাল-only)।</p>
          <input id="ev" type="file" multiple
                 class="file:mr-3 file:px-4 file:py-2 file:rounded-xl file:border-0 file:bg-emerald-500 file:text-slate-900 file:font-bold rounded-xl bg-white/5 px-3 py-2" />
          <div id="evList" class="grid md:grid-cols-4 gap-3"></div>
        </div>
      </section>

      <!-- ALERTS -->
      <section id="tab-alerts" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">🚨 Risk & Hotzone Alerts</h2>
          <div class="rounded-2xl p-4 bg-red-500/15 ring-1 ring-red-400/40">
            <p class="font-bold text-red-300">Tati Bazar Corridor</p>
            <p class="text-slate-200 text-sm">Gold-bar flow detected towards border; patrol advised.</p>
          </div>
          <div class="rounded-2xl p-4 bg-orange-500/15 ring-1 ring-orange-400/40">
            <p class="font-bold text-orange-300">Benapole Gate</p>
            <p class="text-slate-200 text-sm">Freight screening intensity ↑</p>
          </div>
        </div>
      </section>

      <!-- ADMIN -->
      <section id="tab-admin" class="tab">
        <div class="glass rounded-3xl p-6 ring-1 ring-white/10 space-y-4">
          <h2 class="text-xl font-extrabold">🔑 Admin Panel (Demo)</h2>
          <div class="grid md:grid-cols-2 gap-6">
            <form class="space-y-3">
              <div>
                <label class="block text-sm text-slate-300 mb-1">User ID (Request)</label>
                <input class="w-full px-4 py-3 rounded-2xl bg-white/5 ring-1 ring-white/10" placeholder="e.g. CID-9011141745">
              </div>
              <div>
                <label class="block text-sm text-slate-300 mb-1">Assign Role</label>
                <select class="w-full px-4 py-3 rounded-2xl bg-white/5 ring-1 ring-white/10">
                  <option>Analyst</option>
                  <option>Supervisor</option>
                  <option>Admin</option>
                </select>
              </div>
              <div class="flex gap-2">
                <button type="button" class="px-5 py-3 rounded-2xl font-bold bg-emerald-500 text-slate-900">Approve</button>
                <button type="button" class="px-5 py-3 rounded-2xl font-bold bg-rose-500 text-slate-900">Reject</button>
              </div>
            </form>

            <div>
              <p class="font-semibold mb-2">Logo/Theme (Local preview)</p>
              <input id="logoInput" type="file" accept="image/*"
                     class="file:mr-3 file:px-4 file:py-2 file:rounded-xl file:border-0 file:bg-cyan-400 file:text-slate-900 file:font-bold rounded-xl bg-white/5 px-3 py-2" />
              <div class="mt-3 flex items-center gap-3">
                <img id="logoPreview" src="logo.png" class="w-16 h-16 rounded-xl bg-white/5 p-2" alt="">
                <span class="text-slate-300 text-sm">এটা শুধু প্রেজেন্টেশন প্রিভিউ—সেভ করতে চাইলে `logo.png` ওভাররাইট করুন।</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <footer class="text-center text-slate-300 py-8">
        <p class="text-xs">© <span id="y"></span> Smart Crime Tracker • Demo UI (API-ready)</p>
      </footer>
    </main>
  </section>

  <!-- App JS -->
  <script>
    // year
    document.getElementById('y').textContent = new Date().getFullYear();

    // start (Enter or button)
    const start = () => {
      document.getElementById('opening').classList.add('hidden');
      document.getElementById('app').classList.remove('hidden');
    };
    document.getElementById('startBtn').addEventListener('click', start);
    window.addEventListener('keydown', e => { if(e.key==='Enter') start(); });

    // tabs
    document.querySelectorAll('button[data-tab]').forEach(btn=>{
      btn.addEventListener('click', ()=>{
        const id = btn.getAttribute('data-tab');
        document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
        document.getElementById('tab-'+id).classList.add('active');
        document.querySelectorAll('button[data-tab]').forEach(b=>b.classList.remove('text-cyan-300'));
        btn.classList.add('text-cyan-300');
      });
    });

    // Number demo
    document.getElementById('btnNumber').addEventListener('click', ()=>{
      const v = (document.getElementById('msisdn').value || '').trim();
      const box = document.getElementById('numResult');
      if(!v){ box.innerHTML = '<p class="text-slate-300">নাম্বার দিন…</p>'; return; }
      // Mock result (presentation)
      const mock = {
        name:'Rahim Uddin', operator:'(demo) Operator-A', risk:'LOW',
        lastCell:'Dhaka-Motijheel', lastSeen:'Today 10:32'
      };
      box.innerHTML = `
        <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
          <p class="text-slate-300 text-xs mb-1">Subscriber</p>
          <p class="font-bold">${mock.name}</p>
        </div>
        <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
          <p class="text-slate-300 text-xs mb-1">Operator</p>
          <p class="font-bold">${mock.operator}</p>
        </div>
        <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
          <p class="text-slate-300 text-xs mb-1">Risk</p>
          <p class="font-bold">${mock.risk}</p>
        </div>
        <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
          <p class="text-slate-300 text-xs mb-1">Last Cell</p>
          <p class="font-bold">${mock.lastCell}</p>
        </div>
        <div class="rounded-2xl p-4 bg-white/5 ring-1 ring-white/10">
          <p class="text-slate-300 text-xs mb-1">Last Seen</p>
          <p class="font-bold">${mock.lastSeen}</p>
        </div>`;
    });

    // Evidence thumbnails (local preview)
    document.getElementById('ev').addEventListener('change', (e)=>{
      const wrap = document.getElementById('evList'); wrap.innerHTML='';
      [...e.target.files].forEach(f=>{
        const url = URL.createObjectURL(f);
        const card = document.createElement('div');
        card.className = 'rounded-2xl overflow-hidden ring-1 ring-white/10 bg-white/5';
        card.innerHTML = `<img src="${url}" class="w-full h-36 object-cover"><div class="p-2 text-xs">${f.name}</div>`;
        wrap.appendChild(card);
      });
    });

    // Admin: logo live preview (local)
    document.getElementById('logoInput').addEventListener('change',(e)=>{
      const f = e.target.files[0]; if(!f) return;
      const reader = new FileReader();
      reader.onload = ev => document.getElementById('logoPreview').src = ev.target.result;
      reader.readAsDataURL(f);
    });
  </script>
</body>
</html>
