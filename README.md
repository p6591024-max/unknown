<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Hidden iOS Boot</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      height: 100vh;
      width: 100vw;
      font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
      overflow: hidden;
      background: black;
    }

    /* 起動アニメーション */
    #boot-screen {
      position: absolute;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: black;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      z-index: 10;
      animation: fadeOut 1s ease forwards;
      animation-delay: 5s;
    }

    .noise {
      font-size: 16px;
      color: #0f0;
      text-shadow: 0 0 5px #0f0;
      animation: flicker 0.1s infinite;
    }

    @keyframes flicker {
      0%, 100% { opacity: 0.2; }
      50% { opacity: 1; }
    }

    .apple {
      font-size: 80px;
      color: white;
      opacity: 0;
      animation: showApple 2s ease forwards;
      animation-delay: 2s;
    }

    @keyframes showApple {
      to { opacity: 1; }
    }

    @keyframes fadeOut {
      to { opacity: 0; visibility: hidden; }
    }

    /* ホーム画面 */
    #home-screen {
      height: 100%;
      width: 100%;
      background: linear-gradient(135deg, #1c1c3c, #4a2f6e, #1b395d);
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      opacity: 0;
      animation: fadeIn 1s ease forwards;
      animation-delay: 6s;
    }

    @keyframes fadeIn {
      to { opacity: 1; }
    }

    .home {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-gap: 20px;
      padding: 40px 20px 100px;
      text-align: center;
      flex-grow: 1;
    }

    .icon {
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
    }

    .icon div {
      width: 70px;
      height: 70px;
      border-radius: 20px;
      background: #444;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 30px;
      box-shadow: 0 5px 10px rgba(0,0,0,0.3);
      margin-bottom: 5px;
      transition: transform 0.2s;
    }

    .icon div:active {
      transform: scale(0.9);
    }

    .label {
      font-size: 12px;
      color: white;
    }

    .dock {
      position: absolute;
      bottom: 15px;
      left: 50%;
      transform: translateX(-50%);
      width: 90%;
      height: 80px;
      background: rgba(255, 255, 255, 0.2);
      border-radius: 25px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      backdrop-filter: blur(20px);
    }

    .dock .icon div {
      width: 60px;
      height: 60px;
      font-size: 26px;
    }

    /* ポップアップ共通 */
    .popup {
      position: fixed;
      top: 100%;
      left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.95);
      display: flex;
      flex-direction: column;
      color: white;
      z-index: 20;
      padding: 20px;
      opacity: 0;
      transition: all 0.5s ease;
      overflow-y: auto;
    }

    .popup.active {
      top: 0;
      opacity: 1;
    }

    .popup h2 {
      margin: 10px 0;
    }

    .popup button {
      margin-top: 20px;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      background: #444;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }

    /* Cipher Tool 専用 */
    #cipher-tool textarea {
      width: 80%;
      height: 100px;
      border-radius: 8px;
      padding: 10px;
      margin-top: 10px;
      font-size: 16px;
      border: none;
    }

    #cipher-tool .btns {
      margin-top: 15px;
      display: flex;
      gap: 15px;
    }

    /* 設定画面 */
    #settings .option {
      background: #222;
      padding: 15px;
      margin: 8px 0;
      border-radius: 10px;
      font-size: 18px;
      cursor: pointer;
    }

    .subscreen {
      display: none;
    }

    .subscreen.active {
      display: block;
    }

    .back {
      color: #0af;
      cursor: pointer;
      margin-bottom: 20px;
      display: inline-block;
    }
  </style>
</head>
<body>
  <!-- 起動アニメーション -->
  <div id="boot-screen">
    <div class="noise">[ 起動シーケンス開始... ]</div>
    <div class="apple"></div>
  </div>

  <!-- ホーム画面 -->
  <div id="home-screen">
    <div class="home">
      <div class="icon" onclick="openPopup('system-alert')"><div>⚠️</div><div class="label">System Alert</div></div>
      <div class="icon" onclick="openPopup('cipher-tool')"><div>🔒</div><div class="label">Cipher Tool</div></div>
    </div>
    <div class="dock">
      <div class="icon" onclick="openPopup('settings')"><div>⚙️</div></div>
      <div class="icon"><div>🌐</div></div>
      <div class="icon"><div>💬</div></div>
      <div class="icon"><div>⏻</div></div>
    </div>
  </div>

  <!-- System Alert ポップアップ -->
  <div id="system-alert" class="popup">
    <h2>⚠️ システム侵入検知！</h2>
    <p>不正アクセスが検出されました。</p>
    <button onclick="closePopup('system-alert')">閉じる</button>
  </div>

  <!-- Cipher Tool ポップアップ -->
  <div id="cipher-tool" class="popup">
    <h2>🔒 Cipher Tool</h2>
    <textarea id="cipher-input" placeholder="ここにテキストを入力"></textarea>
    <div class="btns">
      <button onclick="encrypt()">Encrypt</button>
      <button onclick="decrypt()">Decrypt</button>
    </div>
    <textarea id="cipher-output" placeholder="結果がここに表示されます" readonly></textarea>
    <button onclick="closePopup('cipher-tool')">閉じる</button>
  </div>

  <!-- 設定画面 -->
  <div id="settings" class="popup">
    <div id="main-settings" class="subscreen active">
      <h2>⚙️ 設定</h2>
      <div class="option" onclick="openSub('wifi')">Wi-Fi</div>
      <div class="option" onclick="openSub('bt')">Bluetooth</div>
      <div class="option" onclick="openSub('display')">画面表示と明るさ</div>
      <div class="option" onclick="openSub('sound')">サウンドと触覚</div>
      <div class="option" onclick="openSub('notify')">通知</div>
      <div class="option" onclick="openSub('general')">一般</div>
      <button onclick="closePopup('settings')">閉じる</button>
    </div>

    <div id="wifi" class="subscreen">
      <span class="back" onclick="backToMain()">＜ 設定</span>
      <h2>Wi-Fi</h2>
      <p>利用可能なネットワーク：</p>
      <ul>
        <li>Home_Network</li>
        <li>Cafe_WiFi</li>
        <li>Free_Public_WiFi</li>
      </ul>
    </div>

    <div id="bt" class="subscreen">
      <span class="back" onclick="backToMain()">＜ 設定</span>
      <h2>Bluetooth</h2>
      <p>利用可能なデバイス：</p>
      <ul>
        <li>AirPods Pro</li>
        <li>Keyboard_XYZ</li>
        <li>Speaker_123</li>
      </ul>
    </div>

    <div id="display" class="subscreen">
      <span class="back" onclick="backToMain()">＜ 設定</span>
      <h2>画面表示と明るさ</h2>
      <p>ライト / ダークモードの切り替え</p>
    </div>

    <div id="sound" class="subscreen">
      <span class="back" onclick="backToMain()">＜ 設定</span>
      <h2>サウンドと触覚</h2>
      <p>着信音、通知音の調整</p>
    </div>

    <div id="notify" class="subscreen">
      <span class="back" onclick="backToMain()">＜ 設定</span>
      <h2>通知</h2>
      <p>アプリごとの通知許可</p>
    </div>

    <div id="general" class="subscreen">
      <span class="back" onclick="backToMain()">＜ 設定</span>
      <h2>一般</h2>
      <p>情報、ソフトウェアアップデートなど</p>
    </div>
  </div>

  <script>
    function openPopup(id) {
      document.getElementById(id).classList.add("active");
    }

    function closePopup(id) {
      document.getElementById(id).classList.remove("active");
      backToMain(); // 設定を閉じたら必ずメインに戻す
    }

    function openSub(id) {
      document.querySelectorAll("#settings .subscreen").forEach(s => s.classList.remove("active"));
      document.getElementById(id).classList.add("active");
    }

    function backToMain() {
      document.querySelectorAll("#settings .subscreen").forEach(s => s.classList.remove("active"));
      document.getElementById("main-settings").classList.add("active");
    }

    // シーザー暗号 (Shift 3)
    function caesar(str, shift) {
      return str.replace(/[a-z]/gi, c => {
        let base = c === c.toLowerCase() ? 97 : 65;
        return String.fromCharCode((c.charCodeAt(0) - base + shift + 26) % 26 + base);
      });
    }

    function encrypt() {
      let input = document.getElementById("cipher-input").value;
      document.getElementById("cipher-output").value = caesar(input, 3);
    }

    function decrypt() {
      let input = document.getElementById("cipher-input").value;
      document.getElementById("cipher-output").value = caesar(input, -3);
    }
  </script>
</body>
</html>
