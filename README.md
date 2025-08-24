<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>Smart Crime Tracker</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: Arial, sans-serif;
      background: url('background.jpg') no-repeat center center fixed;
      background-size: cover;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .container {
      background: rgba(0,0,0,0.75);
      padding: 30px;
      border-radius: 15px;
      text-align: center;
      color: white;
      width: 350px;
    }
    .logo img {
      width: 100px;
      margin-bottom: 20px;
    }
    h2 {
      margin-bottom: 20px;
    }
    input {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 8px;
      border: none;
    }
    button {
      width: 100%;
      padding: 10px;
      border: none;
      border-radius: 8px;
      background: #1e90ff;
      color: white;
      font-weight: bold;
      cursor: pointer;
      margin-top: 10px;
    }
    button:hover {
      background: #4682b4;
    }
    .dashboard {
      display: none;
      text-align: left;
    }
    .dashboard h3 {
      text-align: center;
      margin-bottom: 15px;
    }
    .feature {
      background: rgba(255,255,255,0.1);
      margin: 10px 0;
      padding: 10px;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <div class="container" id="loginBox">
    <div class="logo">
      <img src="logo.png" alt="App Logo">
    </div>
    <h2>Admin Login</h2>
    <input type="text" id="username" placeholder="ইউজারনেম দিন">
    <input type="password" id="password" placeholder="পাসওয়ার্ড দিন">
    <button onclick="login()">Login</button>
  </div>

  <div class="container dashboard" id="dashboard">
    <div class="logo">
      <img src="logo.png" alt="App Logo">
    </div>
    <h3>📌 Smart Crime Tracker Dashboard</h3>
    <div class="feature">🔍 নাম্বার সার্চ</div>
    <div class="feature">📍 লাইভ লোকেশন ট্র্যাক</div>
    <div class="feature">📞 কল রেকর্ডস (CDR)</div>
    <div class="feature">👤 প্রোফাইল আইডেন্টিফিকেশন</div>
    <div class="feature">⚠️ সাসপিশাস হ্যান্ডলার অ্যালার্ট</div>
    <button onclick="logout()">Logout</button>
  </div>

  <script>
    function login() {
      let user = document.getElementById("username").value;
      let pass = document.getElementById("password").value;

      if(user === "admin" && pass === "1234"){ 
        document.getElementById("loginBox").style.display = "none";
        document.getElementById("dashboard").style.display = "block";
      } else {
        alert("ভুল ইউজারনেম অথবা পাসওয়ার্ড!");
      }
    }
    function logout(){
      document.getElementById("dashboard").style.display = "none";
      document.getElementById("loginBox").style.display = "block";
    }
  </script>
</body>
</html>
