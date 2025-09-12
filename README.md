<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Hidden iOS Boot</title>
<style>
body {
  margin: 0;
  background: #000;
  font-family: -apple-system,BlinkMacSystemFont,"Helvetica Neue",sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}

/* iPhoneコンテナ共通 */
#password-screen,
#boot-screen,
.iphone {
  position: relative;
  width: 100vw;   /* 横幅いっぱい */
  height: 100vh;  /* 高さいっぱい */
  border-radius: 40px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0 0 40px rgba(0,0,0,0.7);
  background-size: cover;
  background-position: center;
}

/* パスワード画面 */
#password-screen {
  background: #111;
  color: white;
}

#password-screen h1 {
  margin-bottom:40px;
  font-size:24px;
  text-align:center;
}

#pass-display {
  display:flex;
  gap:15px;
  margin-bottom:30px;
}

.pass-dot {
  width:15px;
  height:15px;
  border-radius:50%;
  border:2px solid white;
  background:#111;
}

.keypad {
  display:grid;
  grid-template-columns:repeat(3,80px);
  gap:15px;
}

.keypad button {
  width:80px;
  height:80px;
  border-radius:50%;
  border:none;
  font-size:24px;
  color:white;
  background:#333;
  box-shadow:0 4px 8px rgba(0,0,0,0.5);
  cursor:pointer;
}

.keypad button:active {
  background:#555;
}

/* 起動アニメーション */
#boot-screen {
  background:black;
  color:#0f0;
  font-family:monospace;
  display:none;
  justify-content:center;
  align-items:center;
  flex-direction:column;
  z-index:100;
}

#boot-screen .noise {
  font-size:14px;
  margin-bottom:20px;
}

#boot-screen .apple {
  font-size:60px;
}

/* ホーム画面 */
.iphone {
  display:none;
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

/* ホームアイコン */
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
  background: rgba(0,0,0,0.25);
  padding:5px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.5);
}

.label {
  font-size:11px;
  margin-top:4px;
  color:white;
  text-align:center;
  text-shadow:0 1px 2px rgba(0,0,0,0.7);
}

/* Dock */
.dock {
  position:absolute;
  bottom:15px;
  left:50%;
  transform:translateX(-50%);
  width:90%;
  height:80px;
  background:rgba(255,255,255,0.15);
  backdrop-filter:blur(20px);
  border-radius:30px;
  display:flex;
  justify-content:space-around;
  align-items:center;
  box-shadow:0 0 20px rgba(0,0,0,0.4);
}

.dock .app img { width:55px; height:55px; }

</style>
</head>
<body>

<!-- パスワード画面 -->
<div id="password-screen">
  <h1>パスコードを入力</h1>
  <div id="pass-display">
    <div class="pass-dot" id="dot1"></div>
    <div class="pass-dot" id="dot2"></div>
    <div class="pass-dot" id="dot3"></div>
    <div class="pass-dot" id="dot4"></div>
  </div>
  <div class="keypad">
    <button onclick="addDigit('1')">1</button>
    <button onclick="addDigit('2')">2</button>
    <button onclick="addDigit('3')">3</button>
    <button onclick="addDigit('4')">4</button>
    <button onclick="addDigit('5')">5</button>
    <button onclick="addDigit('6')">6</button>
    <button onclick="addDigit('7')">7</button>
    <button onclick="addDigit('8')">8</button>
    <button onclick="addDigit('9')">9</button>
    <button onclick="clearDigit()">C</button>
    <button onclick="addDigit('0')">0</button>
    <button onclick="delDigit()">⌫</button>
  </div>
</div>

<!-- 起動アニメーション -->
<div id="boot-screen">
  <div class="noise">[ 起動シーケンス開始... ]</div>
  <div class="apple"></div>
</div>

<!-- ホーム画面 -->
<div class="iphone">
  <div class="status-bar">
    <div id="time">9:41</div>
    <div class="status-icons"><span>📶</span><span>📡</span><span>🔋89%</span></div>
  </div>

  <div class="home">
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/mac.png"><div class="label">Apple</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/phone.png"><div class="label">Phone</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/apple-mail.png"><div class="label">Mail</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/camera.png"><div class="label">Camera</div></div>
  </div>

  <div class="dock">
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/phone.png"><div class="label">Phone</div></div>
    <div class="app"><img src="https://img.icons8.com/ios-filled/50/ffffff/apple-mail.png"><div class="label">Mail</div></div>
  </div>
</div>

<script>
// パスワード管理
const correctPwd = "4246";
let entered = "";

function addDigit(d){
  if(entered.length>=4) return;
  entered += d;
  updateDots();
  if(entered.length===4) setTimeout(checkPassword,200);
}

function delDigit(){
  entered = entered.slice(0,-1);
  updateDots();
}

function clearDigit(){
  entered = "";
  updateDots();
}

function updateDots(){
  for(let i=1;i<=4;i++){
    const dot = document.getElementById('dot'+i);
    if(i<=entered.length){
      dot.style.background="white";
    } else {
      dot.style.background="#111";
    }
  }
}

function checkPassword(){
  if(entered===correctPwd){
    document.getElementById('password-screen').style.display='none';
    startBoot();
  } else {
    shakeScreen();
    entered="";
    updateDots();
  }
}

// 画面揺れ（誤入力）
function shakeScreen(){
  const screen = document.getElementById('password-screen');
  screen.style.transition='0.05s';
  let i=0;
  const interval=setInterval(()=>{
    screen.style.transform=`translateX(${i%2===0?10:-10}px)`;
    i++;
    if(i>5){
      clearInterval(interval);
      screen.style.transform='translateX(0)';
    }
  },50);
}

// 起動アニメーション
function startBoot(){
  const boot=document.getElementById('boot-screen');
  boot.style.display='flex';
  let count=0;
  const chars="01!@#$%^&*(){}[]<>?/\\|";
  const noise=boot.querySelector('.noise');
  const interval=setInterval(()=>{
    noise.textContent=Array(80).fill(0).map(()=>chars[Math.floor(Math.random()*chars.length)]).join('');
    count++;
    if(count>50){
      clearInterval(interval);
      boot.style.display='none';
      showHome();
    }
  },50);
}

// ホーム表示
function showHome(){
  document.querySelector('.iphone').style.display='block';
}

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
</script>

</body>
</html>
