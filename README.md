# anime.html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Ultimate Anime Battle</title>

<style>
body{
  margin:0;
  background:black;
  color:white;
  font-family:Arial;
  text-align:center;
}
h1{color:orange}

.select img{
  width:120px;
  margin:15px;
  border:3px solid transparent;
  border-radius:10px;
}
.select img:hover{
  border:3px solid orange;
}

.bar{
  width:80%;
  height:18px;
  background:#444;
  margin:8px auto;
  border-radius:10px;
}
.hp{
  height:100%;
  background:red;
  width:100%;
  border-radius:10px;
}

button{
  padding:12px 25px;
  margin:10px;
  font-size:18px;
  border:none;
  border-radius:10px;
  background:#ff5722;
  color:white;
}

#msg{font-size:18px;color:#00ffcc}
.hidden{display:none}
</style>
</head>

<body>

<h1>🔥 Ultimate Anime Battle 🔥</h1>

<!-- CHARACTER SELECT -->
<div id="select" class="select">
  <p>Select Your Character</p>
  <img src="https://i.imgur.com/JR5dY6x.png" onclick="choose('Naruto')">
  <img src="https://i.imgur.com/2yAfKzG.png" onclick="choose('Goku')">
</div>

<!-- GAME -->
<div id="game" class="hidden">
  <p id="playerName"></p>

  <p>❤️ Player HP</p>
  <div class="bar"><div id="playerHP" class="hp"></div></div>

  <p>😈 Boss HP</p>
  <div class="bar"><div id="bossHP" class="hp"></div></div>

  <p>Level: <span id="level"></span> | Score: <span id="score"></span></p>

  <button onclick="attack()">🔥 Attack</button>
  <button onclick="special()">⚡ Special</button>

  <p id="msg"></p>
</div>

<audio id="hit" src="https://www.soundjay.com/buttons/sounds/button-16.mp3"></audio>
<audio id="special" src="https://www.soundjay.com/buttons/sounds/button-10.mp3"></audio>

<script>
let player="", pHP=100, bHP=100;
let level=localStorage.getItem("lvl")||1;
let score=localStorage.getItem("scr")||0;

function choose(name){
  player=name;
  document.getElementById("select").classList.add("hidden");
  document.getElementById("game").classList.remove("hidden");
  document.getElementById("playerName").innerText="Player: "+player;
  update();
}

function update(){
  document.getElementById("playerHP").style.width=pHP+"%";
  document.getElementById("bossHP").style.width=bHP+"%";
  document.getElementById("level").innerText=level;
  document.getElementById("score").innerText=score;
}

function save(){
  localStorage.setItem("lvl",level);
  localStorage.setItem("scr",score);
}

function attack(){
  document.getElementById("hit").play();
  let dmg=Math.floor(Math.random()*15)+5;
  bHP-=dmg; score+=dmg;
  counter();
  check("🔥 Attack "+dmg);
}

function special(){
  document.getElementById("special").play();
  let dmg=Math.floor(Math.random()*30)+15;
  bHP-=dmg; score+=dmg;
  counter();
  check("⚡ Special "+dmg);
}

function counter(){
  let cdmg=Math.floor(Math.random()*10)+5;
  pHP-=cdmg;
}

function check(msg){
  if(bHP<=0){
    level++;
    bHP=100+level*20;
    pHP=100;
    msg="😈 Boss Defeated! New Boss!";
  }
  if(pHP<=0){
    msg="💀 Game Over!";
    localStorage.clear();
    level=1; score=0; bHP=100; pHP=100;
  }
  save();
  document.getElementById("msg").innerText=msg;
  update();
}

/* SWIPE CONTROLS */
let x=0;
document.addEventListener("touchstart",e=>x=e.touches[0].clientX);
document.addEventListener("touchend",e=>{
  let y=e.changedTouches[0].clientX;
  if(y-x>50) attack();
  if(x-y>50) special();
});
</script>

</body>
</html>
