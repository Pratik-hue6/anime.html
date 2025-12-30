
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
