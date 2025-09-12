<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Hidden iOS Boot</title>
  <style>
    body {
      margin: 0;
      background: url('https://i.ibb.co/wYV4zLb/ios-wallpaper.jpg') no-repeat center center/cover;
      height: 100vh;
      width: 100vw;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .iphone {
      width: 375px; /* iPhoneの標準幅 */
      height: 812px; /* iPhone X以降の高さ */
      border: 2px solid #000;
      border-radius: 40px;
      overflow: hidden;
      box-shadow: 0 0 30px rgba(0,0,0,0.6);
      position: relative;
      background: url('https://i.ibb.co/wYV4zLb/ios-wallpaper.jpg') no-repeat center center/cover;
    }
    /* ステータスバー */
    .status-bar {
      height: 24px;
      background: rgba(0,0,0,0.2);
      color: white;
      font-size: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 8px;
      font-family: sans-serif;
    }
    .status-icons {
      display: flex;
      gap: 8px;
    }
    /* ホーム画面 */
    .home {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-template-rows: repeat(6, 1fr);
      gap: 15px;
      padding: 40px 20px 100px 20px;
      box-sizing: border-box;
      height: calc(100% - 24px);
    }
    .app {
      width: 60px;
      height: 60px;
      border-radius: 15px;
      background: rgba(255,255,255,0.2);
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 12px;
      color: white;
      text-align: center;
      backdrop-filter: blur(10px);
    }
    /* Dock */
    .dock {
      position: absolute;
      bottom: 15px;
      left: 50%;
      transform: translateX(-50%);
      width: 90%;
      height: 80px;
      background: rgba(255,255,255,0.2);
      border-radius: 30px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      backdrop-filter: blur(15px);
    }
    .dock .app {
      width: 55px;
      height: 55px;
    }
    /* 電源ボタン */
    .power-btn {
      position: absolute;
      top: 10px;
      right: -40px;
      width: 20px;
      height: 60px;
      background: #444;
      border-radius: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="iphone">
    <!-- ステータスバー -->
    <div class="status-bar">
      <div class="time" id="time">9:41</div>
      <div class="status-icons">
        <span>📶</span>
        <span>📡</span>
        <span>🔋100%</span>
      </div>
    </div>
    <!-- ホーム画面 -->
    <div class="home">
      <div class="app">📞</div>
      <div class="app">💬</div>
      <div class="app">📷</div>
      <div class="app">🎵</div>
      <div class="app">🌐</div>
      <div class="app">📺</div>
      <div class="app">📝</div>
      <div class="app">🕹️</div>
      <div class="app">📂</div>
      <div class="app">⚙️</div>
      <div class="app">🛒</div>
      <div class="app">💡</div>
    </div>
    <!-- Dock -->
    <div class="dock">
      <div class="app">📞</div>
      <div class="app">💬</div>
      <div class="app">🌐</div>
      <div class="app">🎵</div>
    </div>
    <!-- 電源ボタン -->
    <div class="power-btn" onclick="window.close()"></div>
  </div>

  <script>
    function updateTime() {
      const now = new Date();
      let h = now.getHours();
      let m = now.getMinutes();
      if (m < 10) m = "0" + m;
      document.getElementById("time").textContent = h + ":" + m;
    }
    setInterval(updateTime, 1000);
    updateTime();
  </script>
</body>
</html>
