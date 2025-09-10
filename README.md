<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title> 詐欺模擬体験サイト</title>
  <style>
    body {
      font-family: Arial, Helvetica, sans-serif;
      margin: 0;
      background: #f2f2f2;
      color: #111;
    }
    header.fake-url {
      background: #e6e6e6;
      padding: 8px 16px;
      font-size: 14px;
      border-bottom: 1px solid #ccc;
      color: #555;
    }
    nav.fake-nav {
      background: #232f3e;
      color: #fff;
      padding: 10px 16px;
      font-size: 16px;
      font-weight: bold;
    }
    .container {
      max-width: 420px;
      margin: 40px auto;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
      background: #fff;
      box-shadow: 0 2px 6px rgba(0,0,0,0.15);
    }
    h1, h2 {
      font-size: 18px;
      margin-bottom: 14px;
      color: #111;
    }
    button {
      display: block;
      width: 100%;
      padding: 12px;
      margin-top: 12px;
      background: #ff9900;
      color: #111;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.2s;
    }
    button:hover {
      background: #e68a00;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }
    input {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border: 1px solid #aaa;
      border-radius: 6px;
      font-size: 14px;
    }
    .loading {
      text-align: center;
      font-size: 15px;
      color: #333;
    }
    .warning {
      background: #ffeaea;
      border: 1px solid #dd4444;
      padding: 16px;
      border-radius: 8px;
      color: #b20000;
      font-weight: bold;
      margin-bottom: 16px;
    }
    .summary {
      font-size: 14px;
      line-height: 1.6;
    }
    a {
      color: #0066c0;
      text-decoration: none;
    }
    a:hover {
      text-decoration: underline;
    }
  </style>
</head>
<body>

  <header class="fake-url" id="fakeUrlBar">
    https://www.amaz0n-co-jp.com/claim
  </header>
  <nav class="fake-nav">Amazen ポイントキャンペーン</nav>

  <div class="container" id="page"></div>

  <script>
    const page = document.getElementById("page");

    function showPage(step) {
      page.innerHTML = "";

      if (step === 1) {
        page.innerHTML = `
          <h1>詐欺模擬体験サイト</h1>
          <p>このサイトは <strong>安全に詐欺体験を学ぶための教材</strong> です。<br>
          実際の入力内容は保存も送信もされません。</p>
          <button onclick="showPage(2)">体験を開始する</button>
        `;
      }

      if (step === 2) {
        page.innerHTML = `
          <html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>強迫的時計</title>
<style>
  body {
    background-color: black;
    color: red;
    font-family: 'Courier New', monospace;
    font-size: 5em;
    text-align: center;
    margin-top: 20vh;
    user-select: none;
  }
</style>
</head>
<body>

<div id="clock"></div>

<script>
  function updateClock() {
    const now = new Date();
    const hours = String(now.getHours()).padStart(2,'0');
    const minutes = String(now.getMinutes()).padStart(2,'0');
    const seconds = String(now.getSeconds()).padStart(2,'0');
    document.getElementById('clock').textContent = `${hours}:${minutes}:${seconds}`;
  }

  setInterval(updateClock, 1000);
  updateClock(); // 最初に即表示
</script>

</body>
</html>
          <p>「おめでとうございます！<br>
          今回のお買い物で <strong>10000円分のポイント</strong> を獲得できます。」</p>
          <button onclick="showPage(3)">ポイントを受け取る</button>
        `;
      }

      if (step === 3) {
        page.innerHTML = `
          <h2>ログイン</h2>
          <form id="loginForm">
            <input type="email" id="email" placeholder="Eメールアドレス" required />
            <input type="password" id="password" placeholder="パスワード" required minlength="4"/>
            <button type="submit">ログイン</button>
          </form>
        `;
        document.getElementById("loginForm").addEventListener("submit", function(e) {
          e.preventDefault();
          const email = document.getElementById("email").value.trim();
          const pass = document.getElementById("password").value.trim();
          if (!email || !pass) {
            alert("入力内容を確認してください。");
            return;
          }
          showPage(4);
        });
      }

      if (step === 4) {
        page.innerHTML = `
          <div class="loading">
            <p>ログインありがとうございます。</p>
            <p>ただいまご注文内容を確認中ですので、しばらくお待ちください…</p>
            <p id="dots">●</p>
          </div>
        `;
        let dots = document.getElementById("dots");
        let count = 1;
        const interval = setInterval(() => {
          dots.textContent = "●".repeat((count % 5) + 1);
          count++;
        }, 500);
        setTimeout(() => {
          clearInterval(interval);
          showPage(5);
        }, 3000);
      }
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


      if (step === 5) {
        page.innerHTML = `
          <div class="warning">
            ⚠️ 何か入力！
          </div>
          <div class="summary">
            <p>何か入力：</p>
            <ul>
              <li>何か入力</li>
              <li>何か入力</li>
              <li>何か入力</li>
            </ul>
          </div>
          <button onclick="showPage(6)">次へ</button>
        `;
      }

      if (step === 6) {
        page.innerHTML = `
          <h2>学習のまとめ</h2>
          <p>詐欺を見抜くには、<strong>URL・内容・要求情報</strong> を冷静に確認することが大切です。</p>
          <p>詳しくはこちらもご確認ください：</p>
          <ul>
            <li><a href="https://www.npa.go.jp/bureau/cyber/" target="_blank">警察庁サイバー犯罪対策</a></li>
            <li><a href="https://www.antiphishing.jp/" target="_blank">フィッシング対策協議会</a></li>
          </ul>
          <p>ご体験ありがとうございました。</p>
        `;
      }
    }

    showPage(1);
  </script>
</body>
</html>
