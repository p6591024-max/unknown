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
}
h1 { margin:10px 0; text-shadow:2px 2px #000; font-size:2em; }
#controls { display:flex; justify-content:center; gap:10px; margin-bottom:10px; flex-wrap:wrap; }
button, select { padding:8px 12px; font-size:1em; border:none; border-radius:10px; cursor:pointer; font-weight:bold; color:#fff; background:linear-gradient(145deg,#444,#666); box-shadow:0 5px #222; transition: all 0.2s; }
button:hover, select:hover { transform: translateY(-2px); box-shadow:0 7px #222; }
button:active, select:active { transform: translateY(1px); box-shadow:0 3px #222; }
#board { display:grid; margin:0 auto; gap:5px; background:#111; padding:5px; border-radius:15px; transition: all 0.3s; }
.cell { background:#222; border-radius:12px; box-shadow: inset 0 5px 10px rgba(0,0,0,0.5),0 5px 10px rgba(0,0,0,0.5); font-size:4em; display:flex; align-items:center; justify-content:center; cursor:pointer; transition: transform 0.2s, background 0.3s; position:relative; }
.cell:hover { background:#333; transform:scale(1.05); }
.cell.pop { animation:pop 0.2s ease; }
@keyframes pop { 0%{transform:scale(0.5);} 50%{transform:scale(1.2);} 100%{transform:scale(1);} }
#message { margin:10px 0; font-size:1.3em; padding:5px 10px; border-radius:10px; background:rgba(0,0,0,0.5); display:inline-block; text-shadow:1px 1px #000; }
</style>
</head>
<body>
<h1>○×ゲーム 完全版</h1>
<div id="controls">
  <label>モード:
    <select id="modeSelect">
      <option value="human">他人対戦</option>
      <option value="ai">AI対戦</option>
    </select>
  </label>
  <label>AIレベル:
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

<script>
const boardEl=document.getElementById('board');
const messageEl=document.getElementById('message');
const modeSelect=document.getElementById('modeSelect');
const aiLevelSelect=document.getElementById('aiLevelSelect');
const resetBtn=document.getElementById('resetBtn');

let board=[], turn='○', mode='human', aiLevel='easy';
const SIZE=3;

function createBoard(){
  board=Array(SIZE).fill().map(()=>Array(SIZE).fill(''));
  boardEl.innerHTML='';
  boardEl.style.gridTemplateColumns=`repeat(${SIZE},1fr)`;
  boardEl.style.width=`${SIZE*100}px`;
  boardEl.style.height=`${SIZE*100}px`;
  for(let r=0;r<SIZE;r++){
    for(let c=0;c<SIZE;c++){
      const cell=document.createElement('div');
      cell.className='cell';
      cell.dataset.row=r;
      cell.dataset.col=c;
      cell.addEventListener('click',()=>handleClick(r,c));
      boardEl.appendChild(cell);
    }
  }
  turn='○';
  mode=modeSelect.value;
  aiLevel=aiLevelSelect.value;
  updateMessage('○のターン');
}

function handleClick(r,c){
  if(board[r][c]!=='') return;
  if(mode==='ai' && turn==='×') return;
  board[r][c]=turn;
  animateCell(r,c);
  if(checkWin()) return;
  nextTurn();
}

function animateCell(r,c){
  const cell=boardEl.children[r*SIZE+c];
  cell.classList.add('pop');
  setTimeout(()=>cell.classList.remove('pop'),200);
  renderBoard();
}

function renderBoard(){
  for(let r=0;r<SIZE;r++){
    for(let c=0;c<SIZE;c++){
      boardEl.children[r*SIZE+c].textContent=board[r][c];
    }
  }
}

function nextTurn(){
  turn=(turn==='○')?'×':'○';
  if(mode==='ai' && turn==='×'){
    updateMessage('AIのターン (×)');
    setTimeout(aiMove,300);
  } else {
    updateMessage(`${turn}のターン`);
  }
}

function aiMove(){
  const empty=[];
  for(let i=0;i<SIZE;i++) for(let j=0;j<SIZE;j++) if(board[i][j]==='') empty.push([i,j]);
  if(empty.length===0) return;
  let r,c;
  if(aiLevel==='easy'){
    [r,c]=empty[Math.floor(Math.random()*empty.length)];
  } else if(aiLevel==='medium'){
    [r,c]=mediumAI(empty);
  } else if(aiLevel==='hard'){
    [r,c]=hardAI(empty);
  }
  board[r][c]='×';
  animateCell(r,c);
  if(checkWin()) return;
  nextTurn();
}

function mediumAI(empty){
  for(let [r,c] of empty){
    board[r][c]='○';
    if(checkPotentialWin('○')) { board[r][c]=''; return [r,c]; }
    board[r][c]='';
  }
  return empty[Math.floor(Math.random()*empty.length)];
}

function hardAI(empty){
  for(let [r,c] of empty){
    board[r][c]='×';
    if(checkPotentialWin('×')) { board[r][c]=''; return [r,c]; }
    board[r][c]='○';
    if(checkPotentialWin('○')) { board[r][c]=''; return [r,c]; }
    board[r][c]='';
  }
  return empty[Math.floor(Math.random()*empty.length)];
}

function checkPotentialWin(player){
  for(let i=0;i<SIZE;i++){
    if(board[i][0]===player && board[i][1]===player && board[i][2]==='') return true;
    if(board[i][0]===player && board[i][2]===player && board[i][1]==='') return true;
    if(board[i][1]===player && board[i][2]===player && board[i][0]==='') return true;
    if(board[0][i]===player && board[1][i]===player && board[2][i]==='') return true;
    if(board[0][i]===player && board[2][i]===player && board[1][i]==='') return true;
    if(board[1][i]===player && board[2][i]===player && board[0][i]==='') return true;
  }
  if(board[0][0]===player && board[1][1]===player && board[2][2]==='') return true;
  if(board[0][0]===player && board[2][2]===player && board[1][1]==='') return true;
  if(board[1][1]===player && board[2][2]===player && board[0][0]==='') return true;
  if(board[0][2]===player && board[1][1]===player && board[2][0]==='') return true;
  if(board[0][2]===player && board[2][0]===player && board[1][1]==='') return true;
  if(board[1][1]===player && board[2][0]===player && board[0][2]==='') return true;
  return false;
}

function checkWin(){
  for(let i=0;i<SIZE;i++){
    if(board[i][0] && board[i][0]===board[i][1] && board[i][1]===board[i][2]) return endGame(`${board[i][0]}の勝ち!`);
    if(board[0][i] && board[0][i]===board[1][i] && board[1][i]===board[2][i]) return endGame(`${board[0][i]}の勝ち!`);
  }
  if(board[0][0] && board[0][0]===board[1][1] && board[1][1]===board[2][2]) return endGame(`${board[0][0]}の勝ち!`);
  if(board[0][2] && board[0][2]===board[1][1] && board[1][1]===board[2][0]) return endGame(`${board[0][2]}の勝ち!`);
  if(board.flat().every(v=>v!=='')) return endGame('引き分け');
  return false;
}

function endGame(msg){
  updateMessage(msg);
  turn=null;
}

function updateMessage(msg){
  messageEl.textContent=msg;
}

resetBtn.addEventListener('click',createBoard);
createBoard();
</script>
</body>
</html>
