<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Hidden iOS Boot</title>
  <style>
    body {
      margin: 0;
      background: #000;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
    }

    /* iPhone本体サイズ */
    .iphone {
      width: 375px;
      height: 812px;
      border-radius: 40px;
      overflow: hidden;
      position: relative;
      box-shadow: 0 0 30px rgba(0,0,0,0.7);
      background: url('https://i.ibb.co/wYV4zLb/ios-wallpaper.jpg') no-repeat center center/cover;
    }

    /* ステータスバー */
    .status-bar {
      height: 24px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 10px;
      color: white;
      font-size: 12px;
      text-shadow: 0 1px 2px black;
    }

    .status-icons {
      display: flex;
      gap: 6px;
    }

    /* ホーム画面 */
    .home {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-template-rows: repeat(6, 1fr);
      gap: 15px 0;
      padding: 40px 10px 100px 10px;
      box-sizing: border-box;
      height: calc(100% - 24px);
    }

    .app {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    .app img {
      width: 60px;
      height: 60px;
      border-radius: 18px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.3);
    }

    .label {
      font-size: 11px;
      margin-top: 4px;
      color: white;
      text-align: center;
      text-shadow: 0 1px 2px black;
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

    .dock .app img {
      width: 55px;
      height: 55px;
    }

    /* 電源ボタン */
    .power-btn {
      position: absolute;
      top: 20px;
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
      <div id="time">9:41</div>
      <div class="status-icons">
        <span>📶</span>
        <span>📡</span>
        <span>🔋100%</span>
      </div>
    </div>

    <!-- ホーム画面 -->
    <div class="home">
      <div class="app"><img src="icons/phone.png" alt="Phone"><div class="label">Phone</div></div>
      <div class="app"><img src="icons/messages.png" alt="Messages"><div class="label">Messages</div></div>
      <div class="app"><img src="icons/camera.png" alt="Camera"><div class="label">Camera</div></div>
      <div class="app"><img src="icons/music.png" alt="Music"><div class="label">Music</div></div>
      <div class="app"><img src="icons/browser.png" alt="Browser"><div class="label">Browser</div></div>
      <div class="app"><img src="icons/tv.png" alt="TV"><div class="label">TV</div></div>
      <div class="app"><img src="icons/notes.png" alt="Notes"><div class="label">Notes</div></div>
      <div class="app"><img src="icons/games.png" alt="Games"><div class="label">Games</div></div>
      <div class="app"><img src="icons/files.png" alt="Files"><div class="label">Files</div></div>
      <div class="app"><img src="icons/settings.png" alt="Settings"><div class="label">Settings</div></div>
      <div class="app"><img src="icons/store.png" alt="Store"><div class="label">Store</div></div>
      <div class="app"><img src="icons/flashlight.png" alt="Flashlight"><div class="label">Flashlight</div></div>
    </div>

    <!-- Dock -->
    <div class="dock">
      <div class="app"><img src="icons/phone.png" alt="Phone"><div class="label">Phone</div></div>
      <div class="app"><img src="icons/messages.png" alt="Messages"><div class="label">Messages</div></div>
      <div class="app"><img src="icons/browser.png" alt="Browser"><div class="label">Browser</div></div>
      <div class="app"><img src="icons/music.png" alt="Music"><div class="label">Music</div></div>
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
