<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hacking Training</title>
  <style>
    body {
      margin: 0;
      font-family: monospace;
      background: #0a0a0a;
      color: #00ff00;
    }
    .screen {
      display: none;
      padding: 20px;
    }
    .active { display: block; }
    .menu button {
      background: black;
      color: #00ff00;
      border: 1px solid #00ff00;
      padding: 8px 16px;
      margin: 6px;
      cursor: pointer;
    }
    input {
      background: black;
      border: 1px solid #00ff00;
      color: #00ff00;
      padding: 6px;
      width: 300px;
    }
    .log {
      margin-top: 10px;
      background: #111;
      padding: 10px;
      height: 200px;
      overflow-y: auto;
      border: 1px solid #00ff00;
    }
    .mission-log {
      margin-top: 10px;
      background: #111;
      padding: 10px;
      height: 180px;
      overflow-y: auto;
      border: 1px solid #00ff00;
    }
  </style>
</head>
<body>
  <!-- Login Screen -->
  <div id="login" class="screen active">
    <h1>Login</h1>
    <input type="text" id="user" placeholder="Username"><br><br>
    <input type="password" id="pass" placeholder="Password"><br><br>
    <button onclick="login()">Enter</button>
  </div>

  <!-- Menu Screen -->
  <div id="menu" class="screen">
    <h1>Main Menu</h1>
    <div class="menu">
      <button onclick="show('dashboard')">Dashboard</button>
      <button onclick="show('missions')">Missions</button>
      <button onclick="show('about')">About</button>
    </div>
  </div>

  <!-- Dashboard Screen -->
  <div id="dashboard" class="screen">
    <h1>Dashboard</h1>
    <input type="text" id="cmd" placeholder="Enter command" onkeydown="if(event.key==='Enter')runCommand()">
    <div class="log" id="log"></div>
    <button onclick="show('menu')">Back</button>
  </div>

  <!-- Missions Screen -->
  <div id="missions" class="screen">
    <h1>Missions</h1>
    <div id="missionText">Mission 1: 銀行ノードをスキャンせよ → <code>scan</code></div>
    <div class="mission-log" id="missionLog"></div>
    <input type="text" id="missionCmd" placeholder="Enter command" onkeydown="if(event.key==='Enter')runMission()">
    <button onclick="show('menu')">Back</button>
  </div>

  <!-- About Screen -->
  <div id="about" class="screen">
    <h1>About</h1>
    <p>これはフィッシング対策教育用の模擬ハッキングサイトです。</p>
    <button onclick="show('menu')">Back</button>
  </div>

  <script>
    let missionStep = 1;

    function show(id){
      document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function login(){
      const u=document.getElementById('user').value;
      const p=document.getElementById('pass').value;
      if(u&&p){
        show('menu');
      } else {
        alert('Username/Password required');
      }
    }

    function runCommand(){
      const cmd=document.getElementById('cmd').value.trim();
      const log=document.getElementById('log');
      if(!cmd) return;
      log.innerHTML+=`<div>> ${cmd}</div>`;
      if(cmd==='help'){
        log.innerHTML+="<div>Available: scan, decrypt, transfer, ticket</div>";
      } else if(cmd==='scan'){
        log.innerHTML+="<div>Scanning... found 2 nodes</div>";
      } else if(cmd==='decrypt'){
        log.innerHTML+="<div>Decrypted fake_keys.zip</div>";
      } else if(cmd==='transfer'){
        log.innerHTML+="<div>Transferring... complete</div>";
      } else if(cmd==='ticket'){
        log.innerHTML+="<div>Escape route secured!</div>";
      } else {
        log.innerHTML+="<div>Unknown command</div>";
      }
      document.getElementById('cmd').value='';
      log.scrollTop=log.scrollHeight;
    }

    function runMission(){
      const cmd=document.getElementById('missionCmd').value.trim();
      const missionLog=document.getElementById('missionLog');
      if(!cmd) return;
      missionLog.innerHTML+=`<div>> ${cmd}</div>`;

      if(missionStep===1 && cmd==='scan'){
        missionLog.innerHTML+="<div>✅ Mission 1 Complete</div>";
        missionStep=2;
        document.getElementById('missionText').innerText="Mission 2: 暗号キーを解読せよ → decrypt";
      } else if(missionStep===2 && cmd==='decrypt'){
        missionLog.innerHTML+="<div>✅ Mission 2 Complete</div>";
        missionStep=3;
        document.getElementById('missionText').innerText="Mission 3: 資金を転送せよ → transfer";
      } else if(missionStep===3 && cmd==='transfer'){
        missionLog.innerHTML+="<div>✅ Mission 3 Complete</div>";
        missionStep=4;
        document.getElementById('missionText').innerText="Final Mission: 脱出せよ → ticket";
      } else if(missionStep===4 && cmd==='ticket'){
        missionLog.innerHTML+="<div>✅ Final Mission Complete! 脱出成功</div>";
        missionStep=5;
        document.getElementById('missionText').innerText="All missions complete!";
      } else {
        missionLog.innerHTML+="<div>Command failed</div>";
      }

      document.getElementById('missionCmd').value='';
      missionLog.scrollTop=missionLog.scrollHeight;
    }
  </script>
</body>
</html>
