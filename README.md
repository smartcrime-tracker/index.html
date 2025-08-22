<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Crime Tracker - Demo</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>🔍 Smart Crime Tracker (Demo)</h1>
  </header>

  <nav>
    <button onclick="showTab('login')">Login</button>
    <button onclick="showTab('numberSearch')">Number Search</button>
    <button onclick="showTab('cdr')">CDR</button>
    <button onclick="showTab('map')">Map</button>
    <button onclick="showTab('profile')">Profile</button>
  </nav>

  <main>
    <!-- Login -->
    <section id="login" class="tab active">
      <h2>Login</h2>
      <input type="text" id="username" placeholder="Username">
      <input type="password" id="password" placeholder="Password">
      <button onclick="login()">Login</button>
      <p id="loginStatus"></p>
    </section>

    <!-- Number Search -->
    <section id="numberSearch" class="tab">
      <h2>Number Search</h2>
      <input type="text" id="phoneNumber" placeholder="Enter phone number">
      <button onclick="searchNumber()">Search</button>
      <pre id="numberResult"></pre>
    </section>

    <!-- CDR -->
    <section id="cdr" class="tab">
      <h2>CDR Analysis</h2>
      <input type="text" id="cdrNumber" placeholder="Enter phone number">
      <button onclick="fetchCDR()">Get CDR</button>
      <pre id="cdrResult"></pre>
    </section>

    <!-- Map -->
    <section id="map" class="tab">
      <h2>Location Map</h2>
      <div id="mapBox">[Map will load here]</div>
    </section>

    <!-- Profile -->
    <section id="profile" class="tab">
      <h2>Criminal Profile</h2>
      <input type="text" id="profileId" placeholder="Enter ID / Name">
      <button onclick="fetchProfile()">Get Profile</button>
      <pre id="profileResult"></pre>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  margin: 0; padding: 0;
  background: #f4f4f4;
}

header {
  background: #222;
  color: #fff;
  text-align: center;
  padding: 1rem;
}

nav {
  background: #444;
  padding: 0.5rem;
  text-align: center;
}

nav button {
  margin: 0.3rem;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  background: #0077cc;
  color: white;
  cursor: pointer;
}

nav button:hover {
  background: #005fa3;
}

main {
  padding: 1rem;
}

.tab {
  display: none;
  background: white;
  padding: 1rem;
  border-radius: 8px;
  margin-top: 1rem;
}

.tab.active {
  display: block;
}

input {
  display: block;
  margin: 0.5rem 0;
  padding: 0.5rem;
  width: 100%;
  max-width: 400px;
}

button {
  background: #28a745;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
function showTab(tabId) {
  document.querySelectorAll('.tab').forEach(tab => tab.classList.remove('active'));
  document.getElementById(tabId).classList.add('active');
}

// Demo login
function login() {
  const user = document.getElementById("username").value;
  const pass = document.getElementById("password").value;

  if(user === "admin" && pass === "1234") {
    document.getElementById("loginStatus").innerText = "✅ Login Successful!";
  } else {
    document.getElementById("loginStatus").innerText = "❌ Invalid Credentials.";
  }
}

// Number Search (API Placeholder)
function searchNumber() {
  const number = document.getElementById("phoneNumber").value;
  document.getElementById("numberResult").innerText = 
    "Searching number: " + number + "\n\n[Replace with API call result]";
}

// CDR Analysis (API Placeholder)
function fetchCDR() {
  const num = document.getElementById("cdrNumber").value;
  document.getElementById("cdrResult").innerText = 
    "Fetching CDR for: " + num + "\n\n[Replace with API call result]";
}

// Profile Fetch (API Placeholder)
function fetchProfile() {
  const id = document.getElementById("profileId").value;
  document.getElementById("profileResult").innerText = 
    "Fetching profile for: " + id + "\n\n[Replace with API call result]";
}
button:hover {
  background: #218838;
}

#mapBox {
  width: 100%;
  height: 300px;
  background: #ddd;
  display: flex;
  justify-content: center;
  align-items: center;
}
