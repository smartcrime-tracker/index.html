<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Crime Tracker - Presentation</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f4f4;
    }
    .screen {
      display: none;
      height: 100vh;
      width: 100vw;
      text-align: center;
      padding: 30px;
    }
    .active {
      display: block;
    }
    .opening {
      background: url('background.jpg') no-repeat center center/cover;
      color: white;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }
    .opening h1 {
      font-size: 40px;
      margin-bottom: 20px;
    }
    .dashboard {
      background: white;
    }
    nav {
      background: #222;
      color: white;
      padding: 15px;
    }
    nav button {
      margin: 0 10px;
      padding: 10px 20px;
      border: none;
      cursor: pointer;
      border-radius: 5px;
    }
    section {
      padding: 30px;
      text-align: left;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }
    table, th, td {
      border: 1px solid #555;
      padding: 8px;
    }
    .alert {
      background: red;
      color: white;
      padding: 20px;
      font-weight: bold;
      border-radius: 8px;
    }
  </style>
</head>
<body>

<!-- Opening Screen -->
<div id="opening" class="screen opening active">
  <h1>﷽</h1>
  <h2>أعوذ بالله من الشيطان الرجيم</h2>
  <h2>بسم الله الرحمن الرحيم</h2>
  <p>اللهم احفظ هذا النظام لأمة محمد ﷺ</p>
  <p><b>বাংলা অর্থ:</b> হে আল্লাহ! এই সিস্টেমটিকে মুহাম্মদ ﷺ এর উম্মাহর জন্য হেফাজত করুন।</p>
  <p>সূরা আল-ইখলাস:</p>
  <audio controls autoplay>
    <source src="surah-ikhlas.mp3" type="audio/mpeg">
  </audio>
  <br><br>
  <button onclick="showScreen('dashboard')">শুরু করুন</button>
</div>

<!-- Dashboard -->
<div id="dashboard" class="screen dashboard">
  <nav>
    <button onclick="showTab('home')">🏠 Home</button>
    <button onclick="showTab('number')">📱 Number Search</button>
    <button onclick="showTab('location')">📍 Live Location</button>
    <button onclick="showTab('criminal')">👤 Criminal Profile</button>
    <button onclick="showTab('cdr')">📞 CDR Analysis</button>
    <button onclick="showTab('alert')">⚠ Suspicious Handler</button>
    <button onclick="showTab('admin')">🔑 Admin Panel</button>
  </nav>

  <section id="home" class="tab active">
    <h1>Welcome to Smart Crime Tracker</h1>
    <p>World’s most advanced autonomous tracking & crime prevention system.</p>
    <img src="logo.png" alt="Logo" width="200">
  </section>

  <section id="number" class="tab">
    <h2>Number Search</h2>
    <input type="text" placeholder="Enter Mobile Number">
    <button>Search</button>
    <p><b>Result:</b> Demo Number → Name: Rahim | Location: Dhaka</p>
  </section>

  <section id="location" class="tab">
    <h2>Live Location</h2>
    <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3651.845099885049!2d90.4071430153854!3d23.75086799465337!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3755b895e4f5a3d1%3A0x6a85f10dba3db1c5!2sBaitul%20Mukarram!5e0!3m2!1sen!2sbd!4v1690325476823!5m2!1sen!2sbd" width="100%" height="300" style="border:0;" allowfullscreen="" loading="lazy"></iframe>
  </section>

  <section id="criminal" class="tab">
    <h2>Criminal Profile Search</h2>
    <table>
      <tr><th>Name</th><th>Crime</th><th>Status</th></tr>
      <tr><td>Hasan</td><td>Smuggling</td><td>Arrested</td></tr>
      <tr><td>Kamal</td><td>Gold Bar Smuggling</td><td>Under Surveillance</td></tr>
    </table>
  </section>

  <section id="cdr" class="tab">
    <h2>CDR Analysis</h2>
    <table>
      <tr><th>Caller</th><th>Receiver</th><th>Duration</th><th>Time</th></tr>
      <tr><td>01812345678</td><td>01798765432</td><td>3 min</td><td>10:30 AM</td></tr>
      <tr><td>01655555555</td><td>01944444444</td><td>7 min</td><td>11:45 AM</td></tr>
    </table>
  </section>

  <section id="alert" class="tab">
    <h2>Suspicious Handler Alert</h2>
    <div class="alert">🚨 Suspicious Contact Detected at Tati Bazar! Take Immediate Action.</div>
  </section>

  <section id="admin" class="tab">
    <h2>Admin Approval Panel</h2>
    <form>
      <label>User ID Request: <input type="text"></label><br><br>
      <label>Assign User ID: <input type="text"></label><br><br>
      <button type="submit">Approve</button>
      <button type="reset">Reject</button>
    </form>
  </section>
</div>

<script>
  function showScreen(id) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }
  function showTab(id) {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }
</script>

</body>
</html>
