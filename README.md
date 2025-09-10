<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>詐欺模擬体験サイト</title>
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
/* step2: Amazon風の時計 */
#clock-container {
  background-color: #fff;
  color: #111;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 1.5em;
  text-align: center;
  padding: 12px 0;
  border-radius: 6px;
  border: 1px solid #ccc;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  margin-bottom: 20px;
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
    <div id="clock-container"></div>
    <p>「おめでとうございます！<br>
    今回のお買い物で <strong>10000円分のポイント</strong> を獲得できます。」</p>
    <button onclick="showPage(3)">ポイントを受け取る</button>
  `;
  
  // 時計スクリプトをここで実行
  function updateClock() {
    const now = new Date();
    const days = ['日', '月', '火', '水', '木', '金', '土'];
    const year = now.getFullYear();
    const month = String(now.getMonth() + 1).padStart(2,'0');
    const date = String(now.getDate()).padStart(2,'0');
    const day = days[now.getDay()];
    const hours = String(now.getHours()).padStart(2,'0');
    const minutes = String(now.getMinutes()).padStart(2,'0');
    const seconds = String(now.getSeconds()).padStart(2,'0');

    document.getElementById('clock-container').textContent =
      `${year}年${month}月${date}日 (${day}) ${hours}:${minutes}:${seconds}`;
  }

  setInterval(updateClock, 1000);
  updateClock();
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

     if (step === 5) {
  page.innerHTML = `
    <div class="warning">
      ⚠️ 情報が外部に送信されました！
    </div>
    <div class="summary">
      <p>個人情報の漏洩のフィードバックを送信してください。：</p>
      <ul>
        <li>City: Tokyo</li>
        <li>State: Tokyo</li>
        <li>Country: Japan</li>
        <li>Postal: 101-8656</li>
        <li>Local time: <span id="tokyo-time"></span></li>
        <li>Timezone: Asia/Tokyo</li>
        <li>Coordinates: 35.6895,139.6917</li>
      </ul>
    </div>
    <button onclick="showPage(6)">フィードバックを送信する</button>
  `;

  // 東京の現在日時を「~PM, ~day, September ~, 2025」形式で表示
  function updateTokyoTime() {
    const now = new Date();

    // 曜日リスト
    const weekdays = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
    const day = weekdays[now.toLocaleString('en-US', { timeZone: 'Asia/Tokyo', weekday: 'short' })];

    // 時間
    let hours = now.toLocaleString('en-US', { timeZone: 'Asia/Tokyo', hour: 'numeric', hour12: true }).split(' ')[0];
    const ampm = now.toLocaleString('en-US', { timeZone: 'Asia/Tokyo', hour: 'numeric', hour12: true }).split(' ')[1];

    const date = now.toLocaleString('en-US', { timeZone: 'Asia/Tokyo', day: 'numeric' });
    const month = now.toLocaleString('en-US', { timeZone: 'Asia/Tokyo', month: 'long' });
    const year = now.toLocaleString('en-US', { timeZone: 'Asia/Tokyo', year: 'numeric' });

    document.getElementById('tokyo-time').textContent =
      `${hours}${ampm}, ${day}, ${month} ${date}, ${year}`;
  }

  setInterval(updateTokyoTime, 1000);
  updateTokyoTime();
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
