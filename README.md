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
