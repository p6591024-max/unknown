<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>教育用詐欺体験サイト</title>
  <style>
    body {
      font-family: Arial, Helvetica, sans-serif;
      margin: 0;
      padding: 0;
      background: #f8f9fa;
    }
    .page {
      display: none;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
    }
    .active { display: flex; }
    .container {
      background: #fff;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      max-width: 420px;
      width: 100%;
    }
    h1, h2, p {
      margin-bottom: 16px;
      color: #222;
    }
    button {
      width: 100%;
      padding: 12px;
      background: #ffa41c;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      font-weight: bold;
      color: #111;
      cursor: pointer;
      margin-top: 12px;
    }
    button:hover { background: #ff8c00; }
    input {
      width: 100%;
      padding: 10px;
      margin-bottom: 12px;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 14px;
    }
    .fake-urlbar {
      background: #e9ecef;
      padding: 6px 12px;
      font-size: 14px;
      border-bottom: 1px solid #ccc;
      color: #444;
    }
    .notifications {
      position: fixed;
      top: 16px;
      right: 16px;
      display: flex;
      flex-direction: column;
      gap: 8px;
      z-index: 999;
    }
    .notice {
      padding: 12px 16px;
      border-radius: 10px;
      color: #fff;
      font-size: 14px;
      min-width: 220px;
      box-shadow: 0 3px 6px rgba(0,0,0,0.2);
      animation: slideIn 0.3s ease-out;
    }
    .success { background: #28a745; }
    .error { background: #dc3545; }
    .info { background: #007bff; }
    @keyframes slideIn {
      from { opacity: 0; transform: translateX(50px); }
      to { opacity: 1; transform: translateX(0); }
    }
    .loading {
      text-align: center;
      font-size: 16px;
      color: #333;
    }
    .spinner {
      border: 4px solid #f3f3f3;
      border-top: 4px solid #ffa41c;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      margin: 20px auto;
      animation: spin 1s linear infinite;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
  </style>
</head>
<body>
  <!-- 説明ページ -->
  <div id="page-intro" class="page active">
    <div class="container">
      <h1>教育用詐欺体験サイト</h1>
      <p>これは安全な体験サイトです。<br>
        実際の詐欺手口を疑似的に体験し、だまされない力を身につけましょう。</p>
      <button onclick="goPage('page-mail')">体験を開始する</button>
    </div>
  </div>

  <!-- 偽メールページ -->
  <div id="page-mail" class="page">
    <div class="container">
      <h2>📩 新着メール</h2>
      <p><b>差出人：</b> Amazon ポイントキャンペーン<br>
         <b>件名：</b> 買い物で10000円分ポイント獲得！</p>
      <p>お客様限定で特別なポイントを進呈します。<br>
         下のボタンを押してログインし、受け取り手続きをしてください。</p>
      <button onclick="goPage('page-login')">ポイントを受け取る</button>
    </div>
  </div>

  <!-- 偽ログインページ -->
  <div id="page-login" class="page">
    <div class="container">
      <div class="fake-urlbar">https://www.amaz0n.co-jp.com/claim</div>
      <h2>Amazon ログイン</h2>
      <input type="text" id="username" placeholder="メールアドレスを入力" />
      <input type="password" id="password" placeholder="パスワードを入力" />
      <button onclick="handleLogin()">ログイン</button>
    </div>
  </div>

  <!-- 偽認証ページ -->
  <div id="page-auth" class="page">
    <div class="container">
      <div class="fake-urlbar">https://www.amaz0n.co-jp.com/auth</div>
      <h2>追加認証が必要です</h2>
      <p>セキュリティのため、SMSで送信された6桁のコードを入力してください。</p>
      <input type="text" id="authcode" placeholder="6桁のコードを入力" maxlength="6"/>
      <button onclick="handleAuth()">認証する</button>
    </div>
  </div>

  <!-- ローディングページ -->
  <div id="page-loading" class="page">
    <div class="container loading">
      <div class="fake-urlbar">https://www.amaz0n.co-jp.com/auth</div>
      <div class="spinner"></div>
      <p>認証中… 少々お待ちください</p>
    </div>
  </div>

  <!-- ネタばらしページ -->
  <div id="page-reveal" class="page">
    <div class="container">
      <h2>⚠️ これは詐欺でした！</h2>
      <p>もし本当に入力していたら、情報が盗まれていた可能性があります。</p>
      <ul>
        <li>URLが本物と違う（amaz0nなど偽ドメイン）</li>
        <li>「10000円ポイント」など甘い誘い</li>
        <li>公式では絶対にメールでログイン要求はしない</li>
      </ul>
      <button onclick="goPage('page-end')">学びをまとめる</button>
    </div>
  </div>

  <!-- 終了ページ -->
  <div id="page-end" class="page">
    <div class="container">
      <h2>体験お疲れさまでした</h2>
      <p>今回のような詐欺に気づけるようになれば安心です。<br>
        詳しい情報は以下の公式サイトを参考にしてください。</p>
      <ul>
        <li><a href="https://www.npa.go.jp/" target="_blank">警察庁</a></li>
        <li><a href="https://www.antiphishing.jp/" target="_blank">フィッシング対策協議会</a></li>
      </ul>
    </div>
  </div>

  <!-- 通知表示 -->
  <div class="notifications" id="notifications"></div>

  <script>
    function goPage(id) {
      document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
      document.getElementById(id).classList.add("active");
    }

    function pushNotice(type, message) {
      const container = document.getElementById("notifications");
      const notice = document.createElement("div");
      notice.className = "notice " + type;
      notice.textContent = message;
      container.appendChild(notice);
      setTimeout(() => {
        notice.style.opacity = "0";
        notice.style.transition = "opacity 0.5s";
        setTimeout(() => container.removeChild(notice), 500);
      }, 3500);
    }

    function handleLogin() {
      const username = document.getElementById("username").value.trim();
      const password = document.getElementById("password").value.trim();

      if (!username || username.length < 5 || !password || password.length < 6) {
        pushNotice("error", "入力内容を確認してください。");
        return;
      }

      pushNotice("success", `ログインありがとうございます、${username} さん！`);
      setTimeout(() => {
        goPage("page-auth");
      }, 1500);
    }

    function handleAuth() {
      const code = document.getElementById("authcode").value.trim();
      if (code.length !== 6 || isNaN(code)) {
        pushNotice("error", "6桁の数字を入力してください。");
        return;
      }

      pushNotice("info", "コードを確認中です…");
      goPage("page-loading");

      setTimeout(() => {
        goPage("page-reveal");
      }, 3000);
    }
  </script>
</body>
</html>
