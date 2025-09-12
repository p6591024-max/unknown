<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>裏iPhone風ホーム画面</title>
<style>
body {
  margin:0;
  background:#000;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
  font-family:-apple-system,BlinkMacSystemFont,"Helvetica Neue",sans-serif;
  overflow:hidden;
}

/* iPhone本体 */
.iphone {
  width:375px;
  height:812px;
  border-radius:40px;
  overflow:hidden;
  position:relative;
  box-shadow:0 0 30px rgba(0,0,0,0.7);
  background:url('https://i.ibb.co/wYV4zLb/ios-wallpaper.jpg') no-repeat center/cover;
}

/* ステータスバー */
.status-bar {
  height:24px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:0 10px;
  color:white;
  font-size:12px;
  text-shadow:0 1px 2px black;
}

.status-icons {
  display:flex;
  gap:6px;
}

/* ホーム画面 */
.home {
  display:grid;
  grid-template-columns:repeat(4,1fr);
  grid-template-rows:repeat(6,1fr);
  gap:15px 0;
  padding:40px 10px 100px 10px;
  box-sizing:border-box;
  height:calc(100% - 24px);
}

.app {
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
}

.app img {
  width:60px;
  height:60px;
  border-radius:18px;
  box-shadow:0 2px 5px rgba(0,0,0,0.3);
}

.label {
  font-size:11px;
  margin-top:4px;
  color:white;
  text-align:center;
  text-shadow:0 1px 2px black;
}

/* Dock */
.dock {
  position:absolute;
  bottom:15px;
  left:50%;
  transform:translateX(-50%);
  width:90%;
  height:80px;
  background:rgba(255,255,255,0.2);
  border-radius:30px;
  display:flex;
  justify-content:space-around;
  align-items:center;
  backdrop-filter:blur(15px);
}

.dock .app img { width:55px; height:55px; }

/* 電源ボタン */
.power-btn {
  position:absolute;
  top:20px;
  right:-40px;
  width:20px;
  height:60px;
  background:#444;
  border-radius:10px;
  cursor:pointer;
}

/* 裏起動ノイズ画面 */
#boot {
  position:absolute;
  top:0;
  left:0;
  width:100%;
  height:100%;
  background:black;
  color:#0f0;
  font-family:monospace;
  display:flex;
  justify-content:center;
  align-items:center;
  flex-direction:column;
  z-index:100;
  overflow:hidden;
  opacity:1;
  transition:opacity 1s ease;
}

#boot.hide { opacity:0; pointer-events:none; }

#error {
  position:absolute;
  top:20%;
  left:50%;
  transform:translateX(-50%);
  color:red;
  font-weight:bold;
  background:rgba(0,0,0,0.7);
  padding:10px;
  border-radius:5px;
  display:none;
  z-index:200;
}
</style>
</head>
<body>

<div class="iphone">
  <div class="status-bar">
    <div id="time">9:41</div>
    <div class="status-icons"><span>📶</span><span>📡</span><span>🔋100%</span></div>
  </div>

  <div class="home">
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/phone.png"><div class="label">Phone</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/apple-mail.png"><div class="label">Mail</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/camera.png"><div class="label">Camera</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/apple-music.png"><div class="label">Music</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/internet.png"><div class="label">Browser</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/tv.png"><div class="label">TV</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/notepad.png"><div class="label">Notes</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/controller.png"><div class="label">Games</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/folder-invoices.png"><div class="label">Files</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/settings.png"><div class="label">Settings</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/shopping-cart.png"><div class="label">Store</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/flash-on.png"><div class="label">Flashlight</div></div>
  </div>

  <div class="dock">
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/phone.png"><div class="label">Phone</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/apple-mail.png"><div class="label">Mail</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/internet.png"><div class="label">Browser</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/apple-music.png"><div class="label">Music</div></div>
  </div>

  <div class="power-btn" onclick="window.close()"></div>
</div>

<!-- 裏起動アニメーション -->
<div id="boot">BOOTING...<br><span id="noise"></span></div>
<div id="error">システム侵入検知！</div>

<script>
// 時刻表示
function updateTime(){
  const now=new Date();
  let h=now.getHours();
  let m=now.getMinutes();
  if(m<10) m="0"+m;
  document.getElementById("time").textContent = h+":"+m;
}
setInterval(updateTime,1000);
updateTime();

// 裏起動ノイズ
const boot=document.getElementById('boot');
const noise=document.getElementById('noise');
let chars="01!@#$%^&*(){}[]<>?/\\|";
function randomNoise(){ return Array(80).fill(0).map(()=>chars[Math.floor(Math.random()*chars.length)]).join('');}
let count=0;
const noiseInterval=setInterval(()=>{
  noise.textContent=randomNoise();
  count++;
  if(count>50){ clearInterval(noiseInterval); boot.classList.add('hide'); setTimeout(()=>showError(),500);}
},50);

// フェイクエラー
function showError(){
  const e=document.getElementById('error');
  e.style.display='block';
  setTimeout(()=>{ e.style.display='none'; },3000);
}

// 簡単暗号化/複合化機能
function encrypt(text,shift){ return text.split('').map(c=>String.fromCharCode(c.charCodeAt(0)+shift)).join(''); }
function decrypt(text,shift){ return text.split('').map(c=>String.fromCharCode(c.charCodeAt(0)-shift)).join(''); }
// 使い方例：
// console.log(encrypt("HELLO",3));
// console.log(decrypt("KHOOR",3));
</script>

</body>
</html>
