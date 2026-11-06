

<!DOCTYPE html>

<html lang="ja">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>ヨット</title>

<style>

*{box-sizing:border-box}

body{margin:0;padding:20px;font-family:Arial,"Noto Sans JP",sans-serif;background:#edf1f5;color:#222}

.container{max-width:1000px;margin:auto;background:white;padding:24px;border-radius:16px;box-shadow:0 4px 15px #0001}

h1{text-align:center;color:#24364b}

h2{margin-top:25px}

.hidden{display:none!important}

button,select,input{font-size:16px;padding:10px;border-radius:8px}

button{border:0;background:#315b83;color:white;cursor:pointer;margin:4px}

button:disabled{background:#aaa;cursor:not-allowed}

input,select{border:1px solid #bbb;background:white;color:#222}

.primary{background:#16804a}

.danger{background:#b83b3b}

#nameInputs{display:flex;flex-wrap:wrap;justify-content:center;gap:10px;margin:12px 0}

.name-row{display:block}

.name-row label{display:block;margin-bottom:5px;font-size:14px}

.name-row input{

 width:150px;height:44px;display:block;padding:10px;

 border:1px solid #888;background:#fff;color:#222;

 pointer-events:auto;user-select:text;-webkit-user-select:text;

 touch-action:manipulation;

}

.setup-section{margin:20px 0;text-align:center}

#roundDisplay{text-align:center;font-size:24px;font-weight:bold;margin:15px}

#currentPlayer{text-align:center;font-size:22px;font-weight:bold;color:#315b83}

#diceArea{display:flex;justify-content:center;flex-wrap:wrap;gap:12px;margin:25px 0}

.die{width:76px;height:76px;background:white;border:2px solid #555;border-radius:12px;display:grid;grid-template-columns:repeat(3,1fr);grid-template-rows:repeat(3,1fr);padding:9px;cursor:pointer;box-shadow:0 3px 5px #0002}

.die.held{border:3px solid #e4ad19;background:#fff5cf;transform:translateY(-6px)}

.pip{width:11px;height:11px;background:#222;border-radius:50%;align-self:center;justify-self:center}

.die.rolling{animation:shake .12s infinite alternate}

@keyframes shake{from{transform:rotate(-5deg)}to{transform:rotate(5deg)}}

#controls{text-align:center;margin:20px 0}

#rollInfo{text-align:center;font-size:17px;margin:12px}

#scoreTable{width:100%;border-collapse:collapse;margin-top:15px;font-size:14px}

#scoreTable th,#scoreTable td{border:1px solid #ccc;padding:8px 5px;text-align:center}

#scoreTable th{background:#e8eef5}

#scoreTable td:first-child{font-weight:bold;background:#f5f7fa}

.score-button{padding:7px;margin:2px;font-size:13px;background:#16804a}

#stopPanel{margin-top:25px;padding:15px;background:#f4f4f4;border-radius:10px;text-align:center}

#stopStatus{margin-top:10px;font-weight:bold}

#results{text-align:center}

.result-item{padding:12px;margin:8px auto;max-width:450px;background:#f0f3f7;border-radius:8px;font-size:19px}

#scoreInstruction{text-align:center;color:#16804a;font-weight:bold}

@media(max-width:600px){

 body{padding:8px}

 .container{padding:12px}

 .die{width:60px;height:60px;padding:7px}

 .pip{width:9px;height:9px}

 #scoreTable{font-size:11px}

 #scoreTable th,#scoreTable td{padding:5px 2px}

 .score-button{font-size:11px;padding:5px 2px}

}

</style>

</head>

<body>

<div class="container">

<section id="setup">

<h1>ヨット</h1>

<div class="setup-section">

<h2>プレイヤー人数</h2>

<select id="playerCount">

<option value="1">1人</option>

<option value="2" selected>2人</option>

<option value="3">3人</option>

<option value="4">4人</option>

</select>

</div>

<form id="setupForm">

<div class="setup-section">

<h2>プレイヤー名</h2>

<div id="nameInputs">

<div class="name-row" id="nameRow1">

<label for="name1">プレイヤー1</label>

<input type="text" id="name1" name="player1" value="A" maxlength="12" autocomplete="off">

</div>

<div class="name-row" id="nameRow2">

<label for="name2">プレイヤー2</label>

<input type="text" id="name2" name="player2" value="B" maxlength="12" autocomplete="off">

</div>

<div class="name-row hidden" id="nameRow3">

<label for="name3">プレイヤー3</label>

<input type="text" id="name3" name="player3" value="C" maxlength="12" autocomplete="off">

</div>

<div class="name-row hidden" id="nameRow4">

<label for="name4">プレイヤー4</label>

<input type="text" id="name4" name="player4" value="D" maxlength="12" autocomplete="off">

</div>

</div>

</div>

<div class="setup-section">

<button type="submit" id="startBtn" class="primary">ゲーム開始</button>

</div>

</form>

</section>

<section id="game" class="hidden">

<h1>ヨット</h1>

<div id="roundDisplay">第1ラウンド</div>

<div id="currentPlayer"></div>

<div id="rollInfo">残り3回振れます</div>

<h2 style="text-align:center">サイコロ</h2>

<div id="diceArea"></div>

<div id="controls">

<button id="rollBtn" class="primary">サイコロを振る</button>

<button id="releaseBtn">すべて解除</button>

</div>

<p style="text-align:center;font-size:13px">サイコロを押すと固定・解除できます。</p>

<p id="scoreInstruction">サイコロを振った後、スコアシートの役を選択してください。</p>

<h2>スコアシート</h2>

<div style="overflow-x:auto">

<table id="scoreTable"></table>

</div>

<div id="stopPanel">

<button id="stopGameBtn" class="danger">試合を途中終了する</button>

<div id="stopStatus"></div>

</div>

</section>

<section id="results" class="hidden">

<h1>ヨット・試合結果</h1>

<h2 id="resultText"></h2>

<div id="ranking"></div>

<button id="restartBtn" class="primary">もう一度遊ぶ</button>

</section>

</div>

<script>

const $=id=>document.getElementById(id);

const categories=[

{id:"one",name:"エース（1）"},

{id:"two",name:"デュース（2）"},

{id:"three",name:"トレイ（3）"},

{id:"four",name:"フォー（4）"},

{id:"five",name:"ファイブ（5）"},

{id:"six",name:"シックス（6）"},

{id:"choice",name:"チョイス"},

{id:"threeKind",name:"スリーダイス"},

{id:"fourKind",name:"フォーダイス"},

{id:"fullHouse",name:"フルハウス"},

{id:"small",name:"S.ストレート"},

{id:"large",name:"B.ストレート"},

{id:"yacht",name:"ヨット"}

];

let players=[];

let current=0;

let dice=[];

let held=[];

let rolls=0;

let gameOver=false;

let isRolling=false;

let animationTimer=null;

let rollTimeout=null;

let roundNumber=1;

let roundOrder=[];

let roundIndex=0;

let stopAfterRound=false;

/* 名前記憶機能 */

const NAME_STORAGE_KEY="yachtPlayerNames";

function loadSavedNames(){

 try{

  const saved=JSON.parse(localStorage.getItem(NAME_STORAGE_KEY));

  if(saved&&typeof saved==="object"){

   for(let i=1;i<=4;i++){

    if(typeof saved[i]==="string"&&saved[i].trim()){

     $("name"+i).value=saved[i];

    }

   }

  }

 }catch(e){

  console.warn("保存された名前を読み込めませんでした。",e);

 }

}

function saveNames(){

 const saved={};

 for(let i=1;i<=4;i++){

  saved[i]=$("name"+i).value.trim()||String.fromCharCode(64+i);

 }

 try{

  localStorage.setItem(NAME_STORAGE_KEY,JSON.stringify(saved));

 }catch(e){

  console.warn("名前を保存できませんでした。",e);

 }

}

/* 人数に応じて名前欄を表示 */

function updateNameInputs(){

 const count=Number($("playerCount").value);

 for(let i=1;i<=4;i++){

  $("nameRow"+i).classList.toggle("hidden",i>count);

 }

}

$("playerCount").addEventListener("change",updateNameInputs);

/* フォームから名前を取得してゲーム開始 */

function startGame(event){

 if(event)event.preventDefault();

 saveNames();

 players=[];

 const count=Number($("playerCount").value);

 const formData=new FormData($("setupForm"));

 for(let i=1;i<=count;i++){

  const enteredName=String(formData.get("player"+i)||"").trim();

  const name=enteredName||String.fromCharCode(64+i);

  players.push({

   name:name,

   scores:{},

   finished:false

  });

 }

 current=0;

 roundNumber=1;

 roundOrder=players.map((p,i)=>i);

 roundIndex=0;

 stopAfterRound=false;

 gameOver=false;

 $("setup").classList.add("hidden");

 $("results").classList.add("hidden");

 $("game").classList.remove("hidden");

 $("stopStatus").textContent="";

 $("stopGameBtn").textContent="試合を途中終了する";

 $("stopGameBtn").disabled=false;

 newTurn();

 renderScoreTable();

}

function newTurn(){

 dice=[1,1,1,1,1];

 held=[false,false,false,false,false];

 rolls=0;

 isRolling=false;

 $("roundDisplay").textContent=`第${roundNumber}ラウンド`;

 $("currentPlayer").textContent=`${players[current].name} のターン`;

 $("scoreInstruction").textContent="サイコロを振った後、スコアシートの役を選択してください。";

 updateDice();

 updateControls();

}

function updateDice(){

 const area=$("diceArea");

 area.innerHTML="";

 dice.forEach((value,index)=>{

  const die=document.createElement("div");

  die.className="die";

  if(held[index])die.classList.add("held");

  if(isRolling)die.classList.add("rolling");

  const patterns={

   1:[4],2:[0,8],3:[0,4,8],

   4:[0,2,6,8],5:[0,2,4,6,8],

   6:[0,2,3,5,6,8]

  };

  for(let i=0;i<9;i++){

   const cell=document.createElement("div");

   if(patterns[value].includes(i)){

    const pip=document.createElement("div");

    pip.className="pip";

    cell.appendChild(pip);

   }

   die.appendChild(cell);

  }

  die.addEventListener("click",()=>{

   if(isRolling||rolls===0||gameOver)return;

   held[index]=!held[index];

   updateDice();

  });

  area.appendChild(die);

 });

}

function updateControls(){

 $("rollBtn").disabled=gameOver||isRolling||rolls>=3;

 $("releaseBtn").disabled=gameOver||isRolling||!held.some(Boolean);

 $("rollInfo").textContent=`残り${3-rolls}回振れます`;

}

function rollDice(){

 if(gameOver||isRolling||rolls>=3)return;

 if(rolls===0)held=[false,false,false,false,false];

 isRolling=true;

 updateControls();

 animationTimer=setInterval(()=>{

  dice=dice.map((value,i)=>held[i]?value:Math.floor(Math.random()*6)+1);

  updateDice();

 },70);

 rollTimeout=setTimeout(()=>{

  clearInterval(animationTimer);

  animationTimer=null;

  rollTimeout=null;

  dice=dice.map((value,i)=>held[i]?value:Math.floor(Math.random()*6)+1);

  rolls++;

  isRolling=false;

  updateDice();

  updateControls();

  renderScoreTable();

  $("scoreInstruction").textContent="記入する役をスコアシートから選択できます。";

 },1000);

}

function releaseAll(){

 if(gameOver||isRolling)return;

 held=[false,false,false,false,false];

 updateDice();

 updateControls();

}

function scoreFor(id,d){

 const sum=d.reduce((a,b)=>a+b,0);

 const counts=[0,0,0,0,0,0,0];

 d.forEach(n=>counts[n]++);

 const faceIds={one:1,two:2,three:3,four:4,five:5,six:6};

 if(faceIds[id]){

  const n=faceIds[id];

  return counts[n]*n;

 }

 if(id==="choice")return sum;

 if(id==="threeKind")return counts.some(c=>c>=3)?sum:0;

 if(id==="fourKind")return counts.some(c=>c>=4)?sum:0;

 if(id==="fullHouse")return counts.includes(3)&&counts.includes(2)?sum:0;

 const unique=[...new Set(d)].sort((a,b)=>a-b);

 const sequence=unique.join("");

 if(id==="small"){

  if(sequence.includes("1234")||sequence.includes("2345")||sequence.includes("3456"))return 15;

  return 0;

 }

 if(id==="large"){

  if(sequence==="12345"||sequence==="23456")return 30;

  return 0;

 }

 if(id==="yacht")return counts.some(c=>c===5)?50:0;

 return 0;

}

function upperTotal(player){

 return ["one","two","three","four","five","six"]

 .reduce((sum,id)=>sum+(player.scores[id]??0),0);

}

function totalScore(player){

 const subtotal=Object.values(player.scores).reduce((a,b)=>a+b,0);

 return subtotal+(upperTotal(player)>=63?35:0);

}

function renderScoreTable(){

 const table=$("scoreTable");

 table.innerHTML="";

 const header=document.createElement("tr");

 header.innerHTML="<th>役</th>";

 players.forEach(p=>{

  const th=document.createElement("th");

  th.textContent=p.name;

  header.appendChild(th);

 });

 table.appendChild(header);

 categories.forEach(category=>{

  const tr=document.createElement("tr");

  const nameCell=document.createElement("td");

  nameCell.textContent=category.name;

  tr.appendChild(nameCell);

  players.forEach((p,i)=>{

   const td=document.createElement("td");

   if(Object.hasOwn(p.scores,category.id)){

    td.textContent=p.scores[category.id];

   }else if(i===current&&!gameOver&&rolls>0&&!isRolling){

    const button=document.createElement("button");

    button.className="score-button";

    button.textContent=`${scoreFor(category.id,dice)}点`;

    button.addEventListener("click",()=>chooseCategory(category.id));

    td.appendChild(button);

   }else{

    td.textContent="－";

   }

   tr.appendChild(td);

  });

  table.appendChild(tr);

 });

 const upperRow=document.createElement("tr");

 upperRow.innerHTML="<td>上段合計</td>";

 players.forEach(p=>{

  const td=document.createElement("td");

  td.textContent=upperTotal(p);

  upperRow.appendChild(td);

 });

 table.appendChild(upperRow);

 const bonusRow=document.createElement("tr");

 bonusRow.innerHTML="<td>ボーナス</td>";

 players.forEach(p=>{

  const td=document.createElement("td");

  td.textContent=upperTotal(p)>=63?35:0;

  bonusRow.appendChild(td);

 });

 table.appendChild(bonusRow);

 const totalRow=document.createElement("tr");

 totalRow.innerHTML="<td><strong>合計</strong></td>";

 players.forEach(p=>{

  const td=document.createElement("td");

  td.innerHTML=`<strong>${totalScore(p)}</strong>`;

  totalRow.appendChild(td);

 });

 table.appendChild(totalRow);

}

function chooseCategory(id){

 if(gameOver||isRolling||rolls===0)return;

 const player=players[current];

 if(Object.hasOwn(player.scores,id))return;

 player.scores[id]=scoreFor(id,dice);

 if(Object.keys(player.scores).length===13){

  player.finished=true;

 }

 renderScoreTable();

 advanceTurn();

}

function advanceTurn(){

 roundIndex++;

 if(roundIndex>=roundOrder.length){

  if(stopAfterRound){

   finishGame(true);

   return;

  }

  if(players.every(p=>p.finished)){

   finishGame(false);

   return;

  }

  roundNumber++;

  roundOrder=players.map((p,i)=>p.finished?-1:i).filter(i=>i!==-1);

  roundIndex=0;

 }

 current=roundOrder[roundIndex];

 newTurn();

 renderScoreTable();

}

/* 途中終了予約・解除 */

function requestStopGame(){

 if(gameOver)return;

 if(!stopAfterRound){

  stopAfterRound=true;

  $("stopStatus").textContent="終了予約済み：全員が現在のラウンドを終えると試合終了します。";

  $("stopGameBtn").textContent="途中終了を解除";

  $("stopGameBtn").classList.remove("danger");

  $("stopGameBtn").classList.add("primary");

 }else{

  stopAfterRound=false;

  $("stopStatus").textContent="途中終了の予約を解除しました。通常どおり試合を続行します。";

  $("stopGameBtn").textContent="試合を途中終了する";

  $("stopGameBtn").classList.remove("primary");

  $("stopGameBtn").classList.add("danger");

 }

}

/* 同点は同順位にする */

function finishGame(early){

 clearInterval(animationTimer);

 clearTimeout(rollTimeout);

 animationTimer=null;

 rollTimeout=null;

 isRolling=false;

 gameOver=true;

 $("game").classList.add("hidden");

 $("results").classList.remove("hidden");

 $("resultText").textContent=early

  ?`第${roundNumber}ラウンド終了時に試合終了`

  :"全プレイヤーが全13項目を記録しました";

 const ranking=$("ranking");

 ranking.innerHTML="";

 const sorted=[...players].sort((a,b)=>totalScore(b)-totalScore(a));

 let previousScore=null;

 let previousRank=0;

 sorted.forEach((p,i)=>{

  const score=totalScore(p);

  const rank=score===previousScore?previousRank:i+1;

  previousScore=score;

  previousRank=rank;

  const div=document.createElement("div");

  div.className="result-item";

  div.textContent=`${rank}位　${p.name}　${score}点`;

  ranking.appendChild(div);

 });

}

function restartGame(){

 clearInterval(animationTimer);

 clearTimeout(rollTimeout);

 animationTimer=null;

 rollTimeout=null;

 players=[];

 current=0;

 dice=[];

 held=[];

 rolls=0;

 gameOver=false;

 isRolling=false;

 roundNumber=1;

 roundOrder=[];

 roundIndex=0;

 stopAfterRound=false;

 $("results").classList.add("hidden");

 $("game").classList.add("hidden");

 $("setup").classList.remove("hidden");

 $("stopStatus").textContent="";

 $("stopGameBtn").textContent="試合を途中終了する";

 $("stopGameBtn").disabled=false;

 $("stopGameBtn").classList.remove("primary");

 $("stopGameBtn").classList.add("danger");

 $("scoreTable").innerHTML="";

 $("diceArea").innerHTML="";

 /* 保存済みの名前を維持 */

 loadSavedNames();

 $("playerCount").value="2";

 updateNameInputs();

}

$("setupForm").addEventListener("submit",startGame);

$("rollBtn").addEventListener("click",rollDice);

$("releaseBtn").addEventListener("click",releaseAll);

$("stopGameBtn").addEventListener("click",requestStopGame);

$("restartBtn").addEventListener("click",restartGame);

loadSavedNames();

updateNameInputs();

</script>

</body>

</html>