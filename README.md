<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Smart Crime Tracker Demo</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #0f2027, #203a43, #2c5364);
      color: #fff;
      text-align: center;
      margin: 0; padding: 0;
    }
    header {
      background: #111;
      padding: 20px;
    }
    h1 {
      margin: 0;
      font-size: 26px;
      color: #00e6e6;
    }
    .container {
      margin: 50px auto;
      max-width: 600px;
      background: rgba(0,0,0,0.6);
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.8);
    }
    input, button {
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: none;
      font-size: 16px;
    }
    input {
      width: 70%;
    }
    button {
      background: #00e6e6;
      cursor: pointer;
      font-weight: bold;
    }
    button:hover {
      background: #00b3b3;
    }
    #result {
      margin-top: 20px;
      font-size: 18px;
      color: #ffdd57;
    }
  </style>
</head>
<body>
  <header>
    <h1>🚔 Smart Crime Tracker (Demo)</h1>
  </header>
  <div class="container">
    <h2>🔍 Mobile Number Tracking</h2>
    <input type="text" id="mobileNumber" placeholder="Enter Mobile Number">
    <br>
    <button onclick="searchNumber()">Search</button>
    <div id="result"></div>
  </div>

  <script>
    // Demo Search Function
    function searchNumber() {
      const number = document.getElementById("mobileNumber").value.trim();
      const resultDiv = document.getElementById("result");

      if (number === "") {
        resultDiv.innerHTML = "⚠️ Please enter a mobile number.";
        return;
      }

      // Demo Result (Later will connect with backend API)
      resultDiv.innerHTML = `
        ✅ Tracking Number: ${number}<br>
        🌍 Location: Sylhet (Demo Data)<br>
        📞 Last Call: 21-Aug-2025, 10:45 PM<br>
        🕵️ Status: Suspicious Handler Filter Active
      `;
    }
  </script>
</body>
</html>
