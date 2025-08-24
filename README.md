<!DOCTYPE html><html lang="bn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Crime Tracker</title>
  <style>
    body, html {
      margin: 0;
      padding: 0;
      height: 100%;
      width: 100%;
      font-family: Arial, sans-serif;
      overflow: hidden;
    }/* Background image */
body {
  background: url('sky-stars.jpg') no-repeat center center fixed;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

/* Logo styling */
#logo {
  max-width: 200px;
  max-height: 200px;
  margin-bottom: 20px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  padding: 10px;
  box-shadow: 0 0 20px rgba(255,255,255,0.6);
}

/* Upload button */
.upload-box {
  background: rgba(0, 0, 0, 0.5);
  padding: 15px;
  border-radius: 12px;
  color: white;
  text-align: center;
}
input[type=file] {
  margin-top: 10px;
  padding: 6px;
  border-radius: 8px;
  border: none;
}

  </style>
</head>
<body>  <!-- Logo -->  <img id="logo" src="logo.png" alt="App Logo">  <!-- Upload option -->  <div class="upload-box">
    <label for="logoUpload">নতুন লোগো আপলোড করুন:</label>
    <input type="file" id="logoUpload" accept="image/*">
  </div>  <script>
    // Live preview when new logo is uploaded
    const logo = document.getElementById('logo');
    const upload = document.getElementById('logoUpload');

    upload.addEventListener('change', function(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          logo.src = e.target.result;
        };
        reader.readAsDataURL(file);
      }
    });
  </script></body>
</html>
