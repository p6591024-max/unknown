
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hidden iOS Boot</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      background: url('https://i.ibb.co/6bJjz9w/ios-wallpaper.jpg') no-repeat center center/cover;
      font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
      overflow: hidden;
    }

    .home-screen {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 20px;
      box-sizing: border-box;
    }

    /* ステータスバー */
    .status-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      color: white;
      font-size: 14px;
      text-shadow: 0 1px 2px black;
      margin-bottom: 20px;
    }
    .status-left {
      margin-left: 10px;
    }
    .status-right {
      margin-right: 10px;
      display: flex;
      gap: 10px;
      align-items: center;
    }

    /* 電源ボタン */
    .top-bar {
      display: flex;
      justify-content: flex-end;
      margin-top: -20px;
    }

    .power-button {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: rgba(255, 0, 0, 0.8);
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      font-size: 20px;
      cursor: pointer;
      box-shadow: 0 2px 5px rgba(0,0,0,0.4);
    }

    /* アプリグリッド */
    .app-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 25px 15px;
      justify-items: center;
    }

    .app {
      width: 65px;
      height: 65px;
      border-radius: 20px;
      background: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 28px;
      box-shadow: 0 3px 6px rgba(0,0,0,0.3);
      cursor: pointer;
      transition: transform 0.2s;
    }

    .app:active {
      transform: scale(0.9);
    }

    .label {
      text-align: center;
      font-size: 12px;
      margin-top: 5px;
      color: white;
      text-shadow: 0 1px 2px black;
    }

    /* Dock */
    .dock {
      background: rgba(255, 255, 255, 0.3);
      border-radius: 20px;
      padding: 8px 20px;
      display: flex;
      justify-content: space-around;
      align-items: center;
    }

    .dock .app {
      width: 55px;
      height: 55px;
    }

    /* 電源OFF画面 */
    .power-off-screen {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: black;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: white;
      font-size: 20px;
      z-index: 9999;
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.5s;
    }

    .power-off-screen.active {
      opacity: 1;
      visibility: visible;
    }

    .slide {
      background: #444;
      border-radius: 50px;
      padding: 10px 30px;
      font-size: 18px;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="home-screen">
    <!-- ステータスバー -->
    <div class="status-bar">
      <div class="status-left" id="time">9:41</div>
      <div class="status-right">
        📶 LTE 🔋100%
      </div>
    </div>

    <!-- 電源ボタン -->
    <div class="top-bar">
      <div class="power-button" onclick="showPowerOff()">⏻</div>
    </div>

    <!-- アプリグリッド -->
    <div class="app-grid">
      <div>
        <div class="app" style="background:#007AFF;">⚡</div>
        <div class="label">System Boot</div>
      </div>
      <div>
        <div class="app" style="background:#FF3B30;">⚠️</div>
        <div class="label">System Alert</div>
      </div>
      <div>
        <div class="app" style="background:#34C759;">🔒</div>
        <div class="label">Cipher Tool</div>
      </div>
      <div>
        <div class="app" style="background:#5856D6;">📂</div>
        <div class="label">Extras</div>
      </div>
    </div>

    <!-- Dock -->
    <div class="dock">
      <div>
        <div class="app" style="background:#8E8E93;">⚙️</div>
        <div class="label">Settings</div>
      </div>
      <div>
        <div class="app" style="background:#FF9500;">🌐</div>
        <div class="label">Browser</div>
      </div>
      <div>
        <div class="app" style="background:#34C759;">💬</div>
        <div class="label">Messages</div>
      </div>
      <div>
        <div class="app" style="background:#FF2D55;">📞</div>
        <div class="label">Phone</div>
      </div>
    </div>
  </div>

  <!-- 電源OFF画面 -->
  <div class="power-off-screen" id="powerOffScreen">
    <div class="slide">🔴 スライドで電源オフ</div>
    <div>電源を切るにはスライドしてください</div>
  </div>

  <script>
    // フルスクリーン化
    document.addEventListener("click", () => {
      if (document.documentElement.requestFullscreen) {
        document.documentElement.requestFullscreen();
      }
    }, { once: true });

    // 電源オフ画面表示
    function showPowerOff() {
      const screen = document.getElementById("powerOffScreen");
      screen.classList.add("active");
      screen.addEventListener("click", powerOff);
    }

    // 電源OFF → 黒画面
    function powerOff() {
      const screen = document.getElementById("powerOffScreen");
      screen.innerHTML = "<div>電源OFF...</div>";
      setTimeout(() => {
        document.body.style.background = "black";
        document.body.innerHTML = "";
        window.close(); // ブラウザによっては無効
      }, 2000);
    }

    // 時計をリアルタイムに更新
    function updateTime() {
      const now = new Date();
      const hours = now.getHours().toString().padStart(2, '0');
      const minutes = now.getMinutes().toString().padStart(2, '0');
      document.getElementById("time").textContent = `${hours}:${minutes}`;
    }
    setInterval(updateTime, 1000);
    updateTime();
  </script>
</body>
</html>
