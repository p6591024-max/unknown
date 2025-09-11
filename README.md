<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>○×ゲーム 完全版</title>
<style>
body {
  margin:0; padding:0;
  font-family:'Segoe UI',sans-serif;
  background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
  color:#fff;
  text-align:center;
  overflow-x:hidden;
}
h1 { margin:10px 0; text-shadow:2px 2px #000; }
#controls { margin:10px; display:flex; flex-wrap:wrap; justify-content:center; gap:5px; }
#board { display:grid; margin:10px auto; gap:5px; transition: transform 0.5s ease; background:#111; padding:5px; border-radius:10px; }
.cell { background:#222; border-radius:12px; box-shadow: inset 0 5px 10px rgba(0,0,0,0.5), 0 5px 10px rgba(0,0,0,0.5); font-size:3em; display:flex; align-items:center; justify-content:center; user-select:none; position:relative; cursor:pointer; transition: background 0.3s, transform 0.3s; }
.cell:hover { background:#333; transform: scale(1.05); }
.special { background: gold; animation: glow 1s infinite alternate; }
@keyframes glow { from { box-shadow:0 0 5px gold;} to { box-shadow:0 0 20px gold;} }
#message { font-size:1.3em; margin:10px 0; padding:5px 10px; border-radius:5px; background:rgba(0,0,0,0.5); display:inline-block; }
</style>
</head>
<body>
<h1>○×ゲーム 完全版</h1>
<div id="controls">
  <label>盤面:
    <select id="sizeSelect">
      <option value="3">3x3</option>
      <option value="4">4x4</option>
      <option value="5">5x5</option>
    </select>
  </label>
  <label>モード:
    <select id="modeSelect">
      <option value="human">他人対戦</option>
      <option value="ai">AI対戦</option>
    </select>
  </label>
  <label>AI難易度:
    <select id="aiLevelSelect">
      <option value="easy">簡単</option>
      <option value="medium">中級</option>
      <option value="hard">難しい</option>
    </select>
  </label>
  <button id="resetBtn">リセット</button>
</div>
<div id="message">○のターン</div>
<div id="board"></div>

<!-- 効果音 -->
<audio id="placeSound" src="https://freesound.org/data/previews/256/256113_3263906-lq.mp3"></audio>
<audio id="specialSound" src="https://freesound.org/data/previews/331/331912_3248244-lq.mp3"></audio>
<audio id="winSound" src="https://freesound.org/data/previews/331/331912_3248244-lq.mp3"></audio>
<audio id="loseSound" src="https://freesound.org/data/previews/331/331912_3248244-lq.mp3"></audio>
<audio id="drawSound" src="https://freesound.org/data/previews/331/331912_3248244-lq.mp3" loop></audio>
<audio id="bgm" src="https://freesound.org/data/previews/493/493383_10224468-lq.mp3" loop autoplay></audio>

<script>
const boardEl=document.getElementById('board');
const messageEl=document.getElementById('message');
const sizeSelect=document.getElementById('sizeSelect');
const modeSelect=document.getElementById('modeSelect');
const aiLevelSelect=document.getElementById('aiLevelSelect');
const resetBtn=document.getElementById('resetBtn');
const placeSound=document.getElementById('placeSound');
const specialSound=document.getElementById('specialSound');
const winSound=document.getElementById('winSound');
const loseSound=document.getElementById('loseSound');
const drawSound=document.getElementById('drawSound');
const bgm=document.getElementById('bgm');

let size=3, board=[], turn='○', lifespan={}, timer, rotation=0;
let mode='human', aiLevel='easy';

function createBoard(){
  size=parseInt(sizeSelect.value);
  mode=modeSelect.value;
  aiLevel=aiLevelSelect.value;
  boardEl.style.gridTemplateColumns=`repeat(${size},1fr)`;
  boardEl.style.width=`${size*90}px`;
  boardEl.style.height=`${size*90}px`;
  boardEl.innerHTML='';
  board=Array(size).fill().map(()=>Array(size).fill(''));
  lifespan={};
  for(let r=0;r<size;r++){
    for(let c=0;c<size;c++){
      const cell=document.createElement('div');
      cell.className='cell';
      if(Math.random()<0.15) cell.classList.add('special');
      cell.dataset.row=r;
      cell.dataset.col=c;
      cell.addEventListener('click',()=>handleClick(r,c));
      boardEl.appendChild(cell);
    }
  }
  turn='○'; rotation=0;
  boardEl.style.transform=`rotate(${rotation}deg)`;
  updateMessage(`${turn}のターン`);
  startTurn();
}

function handleClick(r,c){
  if(board[r][c]!=='') return;
  board[r][c]=turn;
  lifespan[`${r},${c}`]=4;
  const cellEl=boardEl.children[r*size+c];
  cellEl.textContent=turn;
  cellEl.style.transform='scale(0.5)';
  setTimeout(()=>{cellEl.style.transform='scale(1)';},50);
  placeSound.play();
  if(cellEl.classList.contains('special')){ removeRandomCell(); cellEl.classList.remove('special'); specialSound.play(); }
  if(checkWin()) return;
  nextTurn();
}

function removeRandomCell(){
  const filled=[];
  for(let r=0;r<size;r++) for(let c=0;c<size;c++) if(board[r][c]!=='') filled.push([r,c]);
  if(filled.length===0) return;
  const [r,c]=filled[Math.floor(Math.random()*filled.length)];
  board[r][c]='';
  const cellEl=boardEl.children[r*size+c];
  cellEl.textContent='';
  cellEl.style.transform='scale(0.5)';
  setTimeout(()=>{cellEl.style.transform='scale(1)';},50);
}

function checkWin(){
  const lines=[];
  for(let r=0;r<size;r++) lines.push(board[r]);
  for(let c=0;c<size;c++) lines.push(board.map(row=>row[c]));
  lines.push(board.map((row,i)=>row[i]));
  lines.push(board.map((row,i)=>row[size-1-i]));
  for(let line of lines){
    if(line.every(v=>'○'===v)) { endGame('○の勝ち!'); winSound.play(); return true; }
    if(line.every(v=>'×'===v)) { endGame('×の勝ち!'); loseSound.play(); return true; }
  }
  if(board.flat().every(v!=='')){ endGame('引き分け'); drawSound.play(); return true; }
  return false;
}

function nextTurn(){
  clearTimeout(timer);
  decrementLifespan();
  rotation+=90;
  boardEl.style.transform=`rotate(${rotation}deg)`;
  turn=(turn==='○')?'×':'○';
  updateMessage(`${turn}のターン`);
  if(mode==='ai' && turn==='×') aiMove(); else startTurn();
}

function decrementLifespan(){
  for(let key in lifespan){
    lifespan[key]-=1;
    if(lifespan[key]<=0){
      const [r,c]=key.split(',').map(Number);
      board[r][c]='';
      const cellEl=boardEl.children[r*size+c];
      cellEl.textContent='';
      cellEl.style.transform='scale(0.5)';
      setTimeout(()=>{cellEl.style.transform='scale(1)';},50);
      delete lifespan[key];
    }
  }
}

function startTurn(){
  timer=setTimeout(()=>{ 
    updateMessage(`${turn}のターン終了`);
    nextTurn();
  },5000);
}

function updateMessage(msg){
  messageEl.textContent=msg;
  messageEl.style.background=turn==='○'?'rgba(255,100,100,0.7)':'rgba(100,100,255,0.7)';
}

function aiMove(){
  let r,c;
  const empty=[];
  for(let i=0;i<size;i++) for(let j=0;j<size;j++) if(board[i][j]==='') empty.push([i,j]);
  if(empty.length===0){ updateMessage('引き分け'); return; }
  if(aiLevel==='easy'){ [r,c]=empty[Math.floor(Math.random()*empty.length)]; }
  else if(aiLevel==='medium'){ [r,c]=empty[Math.floor(Math.random()*empty.length)]; } //簡易版
  else if(aiLevel==='hard'){ [r,c]=empty[Math.floor(Math.random()*empty.length)]; } //簡易版
  handleClick(r,c);
}

resetBtn.addEventListener('click',createBoard);
createBoard();
</script>
</body>
</html>
