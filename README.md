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

    /* 起動アニメーション画面 */
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
      animation-delay: 5s; /* 5秒後にフェードアウト */
    }

    /* ノイズ風アニメーション */
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

    /* リンゴ風マーク */
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
      animation-delay: 6s; /* 起動後に表示 */
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
      <div class="icon"><div>⚡</div><div class="label">System Boot</div></div>
      <div class="icon"><div>⚠️</div><div class="label">System Alert</div></div>
      <div class="icon"><div>🔒</div><div class="label">Cipher Tool</div></div>
      <div class="icon"><div>📂</div><div class="label">Extras</div></div>
    </div>
    <div class="dock">
      <div class="icon"><div>⚙️</div></div>
      <div class="icon"><div>🌐</div></div>
      <div class="icon"><div>💬</div></div>
      <div class="icon"><div>⏻</div></div>
    </div>
  </div>
</body>
</html>
