
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>ダミーTorサイト</title>
<style>
body {
  background-color: #0d0d0d;
  color: #00ff99;
  font-family: monospace;
  text-align: center;
  padding: 20px;
}
.container {
  border: 2px solid #00ff99;
  border-radius: 8px;
  padding: 20px;
  display: inline-block;
  background-color: #111;
  box-shadow: 0 0 20px #00ff99;
  min-width: 300px;
}
h1 { margin-bottom: 10px; }
.warning { color: #ff3333; margin: 20px 0; font-weight: bold; }
a { color: #00ccff; text-decoration: none; cursor: pointer; display: block; margin:5px 0;}
a:hover { text-decoration: underline; }
input, button { margin:5px; padding:8px; border-radius:5px; border:none;}
button { background:#00ff99; color:#000; font-weight:bold; cursor:pointer; }
.download-btn { display:inline-block; margin:5px; padding:8px 15px; background:#00ccff; color:#000; font-weight:bold; text-decoration:none; border-radius:8px; box-shadow:0 0 10px #00ccff; transition:0.2s; }
.download-btn:hover { background:#00ff99; box-shadow:0 0 15px #00ff99; }
.secret-box { border:1px dashed #00ff99; margin:15px; padding:10px; text-align:left; display:none; }
.secret-title { color:#ffcc00; font-weight:bold; }
#terminal { font-family: monospace; color:#00ff99; text-align:left; background:#000; padding:10px; margin:20px 0; height:150px; overflow-y:auto; border-radius:5px; display:none; }
.cursor { display:inline-block; width:10px; background-color:#00ff99; animation:blink 1s infinite; }
@keyframes blink {0%{background-color:#00ff99;}50%{background-color:transparent;}100%{background-color:#00ff99;}}
table { width:100%; text-align:left; border-collapse:collapse; color:#00ff99;}
table th, table td { padding:5px; border-bottom:1px solid #00ff99; }
.progress-bar { background:#003300; border:1px solid #00ff99; height:6px; width:100px; border-radius:3px; margin-top:4px; overflow:hidden;}
.progress { background:#00ff99; width:0%; height:100%; }
.nav-btn { display:inline-block; margin:5px; padding:5px 10px; background:#111; color:#00ff99; border:1px solid #00ff99; cursor:pointer; border-radius:5px;}
.nav-btn:hover { background:#00ff99; color:#000; }
</style>
</head>
<body>
<div class="container">
  <h1>ダミーTorサイト</h1>
  <p class="warning">⚠️ このサイトは教育用ダミーです。本物ではありません。</p>

  <div>
    <span class="nav-btn" onclick="showPage('home')">Home</span>
    <span class="nav-btn" onclick="showPage('login')">Login</span>
    <span class="nav-btn" onclick="showPage('register')">Register</span>
    <span class="nav-btn" onclick="showPage('about')">About</span>
    <span class="nav-btn" onclick="showPage('dashboard')">Dashboard</span>
  </div>

  <!-- Home -->
  <div id="home" class="page">
    <p>Welcome! トップページです。</p>
  </div>

  <!-- Login -->
  <div id="login" class="page" style="display:none;">
    <input type="text" placeholder="Username"><br>
    <input type="password" placeholder="Password"><br>
    <button onclick="alert('ダミーです！ログインはありません。'); showPage('dashboard')">Login</button>
  </div>

  <!-- Register -->
  <div id="register" class="page" style="display:none;">
    <input type="text" placeholder="Username"><br>
    <input type="email" placeholder="Email"><br>
    <input type="password" placeholder="Password"><br>
    <button onclick="alert('ダミーです！登録はできません。')">Register</button>
  </div>

  <!-- About -->
  <div id="about" class="page" style="display:none;">
    <p>このサイトは <strong>教育用ダミー</strong> です。Torとは関係ありません。</p>
  </div>

  <!-- Dashboard -->
  <div id="dashboard" class="page" style="display:none;">
    <div id="terminal"></div>

    <div class="secret-box" style="display:block;">
      <p class="secret-title">[1] Hidden Links</p>
      <ul>
        <li>http://xxxxxxx.onion/market (ダミー)</li>
        <li>http://xxxxxxx.onion/forum (ダミー)</li>
        <li>http://xxxxxxx.onion/files (ダミー)</li>
      </ul>
    </div>

    <div class="secret-box" style="display:block;">
      <p class="secret-title">[2] Messages</p>
      <p>ここに秘密メッセージが流れる…かも。</p>
    </div>

    <div class="secret-box" style="display:block;">
      <p class="secret-title">[3] Status</p>
      <p>サーバー状態: ONLINE ✅</p>
      <p>接続ノード: 3</p>
    </div>

    <div class="secret-box" style="display:block;">
      <p class="secret-title">[4] Downloads</p>
      <table>
        <tr><th>ファイル名</th><th>サイズ</th><th>更新日</th><th>DL数</th><th>操作</th></tr>
        <tr>
          <td>report.pdf</td><td>2.4 MB</td><td>2025-09-01</td><td>143</td>
          <td><a href="#" class="download-btn" onclick="fakeDownload(this)">Download</a>
            <div class="progress-bar"><div class="progress"></div></div>
          </td>
        </tr>
        <tr>
          <td>keys.zip</td><td>7.8 MB</td><td>2025-08-20</td><td>89</td>
          <td><a href="#" class="download-btn" onclick="fakeDownload(this)">Download</a>
            <div class="progress-bar"><div class="progress"></div></div>
          </td>
        </tr>
        <tr>
          <td>data.txt</td><td>512 KB</td><td>2025-07-15</td><td>321</td>
          <td><a href="#" class="download-btn" onclick="fakeDownload(this)">Download</a>
            <div class="progress-bar"><div class="progress"></div></div>
          </td>
        </tr>
      </table>
    </div>
  </div>

</div>

<script>
// ページ切替
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.style.display='none');
  document.getElementById(id).style.display='block';
}

// ターミナル風アニメーション
const terminal = document.getElementById("terminal");
const lines = ["接続中...","暗号化ハンドシェイクを確立...","ノード: jp01 → us03 → eu12","認証完了。","アクセス許可: GRANTED ✅"];
let lineIndex=0, charIndex=0;
let cursor = document.createElement("span"); cursor.classList.add("cursor"); terminal.appendChild(cursor);
function typeLine(){
  if(lineIndex<lines.length){
    if(charIndex<lines[lineIndex].length){
      cursor.insertAdjacentText("beforebegin",lines[lineIndex][charIndex]); charIndex++;
      setTimeout(typeLine,50);
    } else { cursor.insertAdjacentHTML("beforebegin","<br>"); lineIndex++; charIndex=0; setTimeout(typeLine,300); }
  }
}
typeLine();

// 偽ダウンロード
function fakeDownload(el){
  const progress = el.nextElementSibling.querySelector('.progress');
  let width=0;
  const interval=setInterval(()=>{
    if(width>=100){ clearInterval(interval); alert("⚠️ ダミーです！"); }
    else { width+=10; progress.style.width=width+"%"; }
  },200);
}
</script>
</body>
</html>
