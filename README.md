<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Crime Tracker</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url("background.jpg") no-repeat center center fixed;
      background-size: cover;
      color: white;
      text-align: center;
    }
    .overlay {
      background: rgba(0, 0, 0, 0.6);
      min-height: 100vh;
      padding: 20px;
    }
    .logo {
      margin-top: 30px;
    }
    .logo img {
      width: 180px;
      border-radius: 20px;
      box-shadow: 0 0 25px gold;
    }
    .title {
      font-size: 32px;
      font-weight: bold;
      margin-top: 15px;
      color: gold;
      text-shadow: 0 0 15px black;
    }
    .card {
      background: rgba(255, 255, 255, 0.1);
      padding: 20px;
      margin: 20px auto;
      max-width: 400px;
      border-radius: 15px;
      box-shadow: 0 0 15px black;
    }
    input, button {
      width: 90%;
      padding: 12px;
      margin: 8px 0;
      border: none;
      border-radius: 8px;
      font-size: 16px;
    }
    input {
      background: rgba(255,255,255,0.9);
    }
    button {
      background: gold;
      color: black;
      cursor: pointer;
      font-weight: bold;
    }
    button:hover {
      background: orange;
    }
  </style>
</head>
<body>
  <div class="overlay">
    <!-- Logo Section -->
    <div class="logo">
      <img src="logo.png" alt="Smart Crime Tracker Logo">
    </div>
    <div class="title">Smart Crime Tracker</div>

    <!-- Login Form -->
    <div class="card">
      <h2>🔐 Login</h2>
      <form>
        <input type="text" placeholder="Username" required><br>
        <input type="password" placeholder="Password" required><br>
        <button type="submit">Login</button>
      </form>
    </div>

    <!-- Number Search -->
    <div class="card">
      <h2>🔍 Number Search</h2>
      <form>
        <input type="text" placeholder="Enter Mobile Number"><br>
        <button type="submit">Search</button>
      </form>
    </div>

    <!-- Admin Panel Placeholder -->
    <div class="card">
      <h2>⚙️ Admin Panel</h2>
      <p>Only admin can access this section.</p>
    </div>
  </div>
</body>
</html>
