
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>Sona New System</title>
<style>
body { background-color:#0d0d0d; color:#00ff99; font-family:monospace; text-align:center; padding:20px;}
.container { border:2px solid #00ff99; border-radius:8px; padding:20px; display:inline-block; background:#111; box-shadow:0 0 20px #00ff99; min-width:400px;}
h1 { margin-bottom:10px; }
.warning { color:#ff3333; font-weight:bold; margin:20px 0; }
.nav-btn { color:#00ccff; text-decoration:none; cursor:pointer; display:inline-block; margin:5px; padding:5px 10px; border:1px solid #00ccff; border-radius:5px;}
.nav-btn:hover { background:#00ff99; color:#000; }
input { margin:5px; padding:8px; border-radius:5px; border:none; width:80%; }
#terminal { font-family:monospace; color:#00ff99; text-align:left; background:#000; padding:10px; margin:20px 0; height:300px; overflow-y:auto; border-radius:5px;}
.progress-bar { background:#003300; border:1px solid #00ff99; height:6px; width:200px; border-radius:3px; margin:4px 0; overflow:hidden;}
.progress { background:#00ff99; width:0%; height:100%; }
</style>
</head>
<body>
<div class="container">
  <h1>Sona New System</h1>
  <p class="warning">Connection completed</p>

  <div>
    <span class="nav-btn" onclick="showPage('home')">Home</span>
    <span class="nav-btn" onclick="showPage('login')">Login</span>
    <span class="nav-btn" onclick="showPage('register')">Register</span>
    <span class="nav-btn" onclick="showPage('about')">About</span>
    <span class="nav-btn" onclick="showPage('dashboard')">Dashboard</span>
  </div>

  <div id="home" class="page">Welcome! </div>
  <div id="login" class="page" style="display:none;">
    <input type="text" placeholder="Username"><br>
    <input type="password" placeholder="Password"><br>
    <button onclick="alert('Login successful'); showPage('dashboard')">Login</button>
  </div>
  <div id="register" class="page" style="display:none;">
    <input type="text" placeholder="Username"><br>
    <input type="email" placeholder="Email"><br>
    <input type="password" placeholder="Password"><br>
    <button onclick="alert('successful')">Register</button>
  </div>
  <div id="about" class="page" style="display:none;">
    Connection completed
  </div>

  <div id="dashboard" class="page" style="display:none;">
    <div id="terminal"></div>
    <input type="text" id="cmdInput" placeholder="コマンド: scan / decrypt / access / transfer / ticket">
  </div>

</div>

<script>
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.style.display='none');
  document.getElementById(id).style.display='block';
  if(id==='dashboard') startDashboard();
}

let dashboardStarted=false;
function startDashboard(){
  if(dashboardStarted) return;
  dashboardStarted=true;

  const terminal = document.getElementById("terminal");

  // 初期自動ログ
  const autoLines = [
    "接続中... bank-server.fake ...",
    "暗号化ハンドシェイク確立...",
    "ノード: jp01 → us03 → eu12",
    "認証完了。アクセス権: GRANTED ✅"
  ];
  autoLines.forEach((line,i)=>setTimeout(()=>{
    const div = document.createElement("div");
    div.textContent=line;
    terminal.appendChild(div);
    terminal.scrollTop=terminal.scrollHeight;
  }, i*600));

  // 偽ハッキング自動流れ
  const hackLines = [
    "ノードスキャン中... 192.168.0.*",
    "暗号化キー解析中... ██████",
    "SSH接続試行... 成功",
    "ユーザー認証突破... OK",
    "データベースアクセス中...",
    "ファイル取得中... 42%",
    "通信パケット解析中..."
  ];
  let hackIndex=0;
  setInterval(()=>{
    const div = document.createElement("div");
    div.textContent=hackLines[hackIndex];
    terminal.appendChild(div);
    hackIndex=(hackIndex+1)%hackLines.length;
    terminal.scrollTop=terminal.scrollHeight;
  },1200);

  // コマンド操作
  const cmdInput = document.getElementById("cmdInput");
  cmdInput.addEventListener("keydown", function(e){
    if(e.key==="Enter"){
      const cmd = cmdInput.value.trim();
      const div = document.createElement("div");
      div.textContent="> "+cmd;
      terminal.appendChild(div);

      let output="";
      if(cmd==="scan"){
        output="日本銀行ノードスキャン完了: 5 ノード検出";
      } else if(cmd==="decrypt"){
        output="暗号解読成功: fake_keys.zip を取得";
      } else if(cmd==="access"){
        const fakeData=[];
        for(let i=0;i<5;i++){
          const name=["Bob Smith","Mikail　Fikret","Hussein　Aydin","Nadir　Nail","Misaki Takahashi"][i];
          const acc=Math.floor(Math.random()*90000000+10000000);
          const bal="¥"+(Math.floor(Math.random()*5000000)+100000);
          fakeData.push(`${name} | ${acc} | ${bal}`);
        }
        output="口座データ取得成功:\n"+fakeData.join("\n");
      } else if(cmd==="transfer"){
        const overseasBanks=["Bank of Atlantis","New Pacific Bank","Oceanic Bank"];
        const bank=overseasBanks[Math.floor(Math.random()*overseasBanks.length)];
        const amount="¥"+(Math.floor(Math.random()*1000000)+100000);
        // 進捗バー演出
        const progDiv = document.createElement("div");
        const bar=document.createElement("div"); bar.className="progress"; progDiv.className="progress-bar";
        progDiv.appendChild(bar);
        terminal.appendChild(progDiv);
        let w=0;
        const interval=setInterval(()=>{
          if(w>=100){ clearInterval(interval); 
            const outDiv=document.createElement("div"); 
            outDiv.textContent=`資金移動完了: ${250,000,023,04} が ${Jour Ferie la BANK} に送金されました`;
            terminal.appendChild(outDiv);
            terminal.scrollTop=terminal.scrollHeight;
          } else { w+=10; bar.style.width=w+"%"; terminal.scrollTop=terminal.scrollHeight; }
        },200);
      } else if(cmd==="ticket"){
        const passportNum="P"+Math.floor(Math.random()*9000000+1000000);
        const flightNum="FL"+Math.floor(Math.random()*900+100);
        const dest=["Tokyo","London","Paris","Dubai","Sydney"][Math.floor(Math.random()*5)];
        output=`航空券生成完了:\n名前: 山田 太郎\nパスポート: ${passportNum}\n便名: ${flightNum}\n行先: ${dest}`;
      } else {
        output="コマンドが存在しません";
      }

      if(output){
        const outDiv=document.createElement("div"); 
        outDiv.textContent=output;
        terminal.appendChild(outDiv);
        terminal.scrollTop=terminal.scrollHeight;
      }
      cmdInput.value="";
    }
  });
}
</script>
</body>
</html>
