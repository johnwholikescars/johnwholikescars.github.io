<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>PhotoRank</title>

<style>

body{
  margin:0;
  background:#0f0f0f;
  font-family:Arial;
  color:white;
  overflow:hidden;
}

/* TOP */

#topArea{
  display:flex;
  flex-direction:column;
  align-items:center;
  margin-top:10px;
}

#counter{
  font-size:20px;
  font-weight:bold;
  margin-bottom:8px;
}

/* mini buttons */

#miniButtons{
  display:flex;
  gap:10px;
  margin-bottom:10px;
}

#miniButtons button{
  padding:8px 12px;
  border:none;
  border-radius:8px;
  cursor:pointer;
  font-size:14px;
}

#back{
  background:#777;
  color:white;
}

#skip{
  background:#4da6ff;
  color:white;
}

/* APP */

#app{
  display:flex;
  flex-direction:column;
  align-items:center;
}

/* CARD */

.card{
  width:320px;
  height:420px;
  border-radius:20px;
  overflow:hidden;
  background:#222;
  position:relative;
  transition:transform 0.35s ease, opacity 0.35s ease;
  box-shadow:0 0 25px rgba(0,0,0,0.4);
}

.card img{
  width:100%;
  height:100%;
  object-fit:cover;
}

/* ANIMATIONS */

.fly-right{
  transform:translateX(500px) rotate(18deg);
  opacity:0;
}

.fly-left{
  transform:translateX(-500px) rotate(-18deg);
  opacity:0;
}

.fly-up{
  transform:translateY(-500px);
  opacity:0;
}

/* FLASH */

.flashScreen{
  position:fixed;
  inset:0;
  background:black;
  display:flex;
  justify-content:center;
  align-items:center;
  opacity:0;
  pointer-events:none;
  transition:opacity 0.2s ease;
  z-index:999;
}

.flashShow{
  opacity:1;
}

#flashText{
  font-size:58px;
  font-weight:900;
  text-shadow:0 0 25px rgba(255,255,255,0.2);
}

/* BOTTOM BUTTONS */

.bottomBar{
  position:fixed;
  bottom:20px;
  left:0;
  right:0;
  display:flex;
  justify-content:center;
  gap:10px;
  flex-wrap:wrap;
}

.bottomBar button{
  padding:12px 16px;
  border:none;
  border-radius:10px;
  font-size:15px;
  cursor:pointer;
  font-weight:bold;
}

/* TIER COLORS */

#sTier{
  background:#ffd700;
  color:black;
}

#aTier{
  background:#4dff88;
  color:black;
}

#bTier{
  background:#4da6ff;
  color:white;
}

#cTier{
  background:#ff4d4d;
  color:white;
}

/* END SCREEN */

#endScreen{
  position:fixed;
  inset:0;
  display:none;
  justify-content:center;
  align-items:center;
  background:rgba(0,0,0,0.92);
  text-align:center;
}

#endBox{
  background:#1b1b1b;
  padding:30px;
  border-radius:20px;
  width:300px;
}

#endBox h1{
  margin-top:0;
}

#stats{
  line-height:2;
  font-size:18px;
}

#playAgain{
  margin-top:15px;
  padding:12px 18px;
  border:none;
  border-radius:10px;
  background:white;
  cursor:pointer;
  font-weight:bold;
}

</style>
</head>

<body>

<!-- TOP -->

<div id="topArea">

  <div id="counter">
    1 / 1
  </div>

  <div id="miniButtons">
    <button id="back">⬅ Back</button>
    <button id="skip">⏭ Skip</button>
  </div>

</div>

<!-- APP -->

<div id="app">

  <div class="flashScreen" id="flashScreen">
    <div id="flashText"></div>
  </div>

  <div class="card" id="card">
    <img id="img" src="" />
  </div>

</div>

<!-- BOTTOM -->

<div class="bottomBar">

  <button id="sTier">⭐ S Tier</button>

  <button id="aTier">🟢 A Tier</button>

  <button id="bTier">🔵 B Tier</button>

  <button id="cTier">🔴 C Tier</button>

</div>

<!-- END -->

<div id="endScreen">

  <div id="endBox">

    <h1>Rankings Complete</h1>

    <div id="stats"></div>

    <button id="playAgain" onclick="restart()">
      Play Again
    </button>

  </div>

</div>

<script>

/* =========================
   YOUR IMAGES
========================= */

let imagesBase = [

  "IMG_5076.jpeg",
  "IMG_5075.jpeg",
  "IMG_5074.jpeg",

  "IMG_5073.jpeg",
  "IMG_5072.jpeg",
  "IMG_5071.jpeg"

  // ADD THE REST OF YOUR IMAGES HERE
];

/* =========================
   STATE
========================= */

let images = [];
let skipped = [];
let history = [];

let index = 0;

let sCount = 0;
let aCount = 0;
let bCount = 0;
let cCount = 0;

/* =========================
   SHUFFLE
========================= */

function shuffle(arr){

  for(let i = arr.length - 1; i > 0; i--){

    let j = Math.floor(Math.random() * (i + 1));

    [arr[i], arr[j]] = [arr[j], arr[i]];
  }

  return arr;
}

/* =========================
   START
========================= */

function start(){

  images = shuffle([...imagesBase]);

  skipped = [];
  history = [];

  index = 0;

  sCount = 0;
  aCount = 0;
  bCount = 0;
  cCount = 0;

  load();
}

/* =========================
   COUNTER
========================= */

function updateCounter(){

  document.getElementById("counter").innerText =
    `${index + 1} / ${images.length}`;
}

/* =========================
   LOAD IMAGE
========================= */

function load(){

  if(index >= images.length && skipped.length > 0){

    images = [...skipped];

    skipped = [];

    index = 0;
  }

  if(index >= images.length){

    end();

    return;
  }

  document.getElementById("img").src = images[index];

  updateCounter();
}

/* =========================
   FLASH
========================= */

function flash(text, color){

  const flashScreen = document.getElementById("flashScreen");

  const flashText = document.getElementById("flashText");

  flashText.innerText = text;

  flashText.style.color = color;

  flashScreen.classList.add("flashShow");

  setTimeout(() => {

    flashScreen.classList.remove("flashShow");

  }, 500);
}

/* =========================
   RATE
========================= */

function rate(tier){

  const card = document.getElementById("card");

  history.push({
    index:index,
    action:tier
  });

  if(tier === "s"){

    sCount++;

    card.classList.add("fly-right");

    flash("⭐ S TIER", "#ffd700");
  }

  if(tier === "a"){

    aCount++;

    card.classList.add("fly-right");

    flash("🟢 A TIER", "#4dff88");
  }

  if(tier === "b"){

    bCount++;

    card.classList.add("fly-left");

    flash("🔵 B TIER", "#4da6ff");
  }

  if(tier === "c"){

    cCount++;

    card.classList.add("fly-left");

    flash("🔴 C TIER", "#ff4d4d");
  }

  setTimeout(() => {

    card.classList.remove("fly-right");
    card.classList.remove("fly-left");

    index++;

    load();

  }, 350);
}

/* =========================
   SKIP
========================= */

function skip(){

  const card = document.getElementById("card");

  skipped.push(images[index]);

  history.push({
    index:index,
    action:"skip"
  });

  card.classList.add("fly-up");

  flash("⏭ SKIPPED", "#4da6ff");

  setTimeout(() => {

    card.classList.remove("fly-up");

    index++;

    load();

  }, 350);
}

/* =========================
   BACK
========================= */

function back(){

  if(history.length === 0) return;

  const previous = history.pop();

  if(previous.action === "s") sCount--;

  if(previous.action === "a") aCount--;

  if(previous.action === "b") bCount--;

  if(previous.action === "c") cCount--;

  if(previous.action === "skip"){
    skipped.pop();
  }

  index = previous.index;

  load();
}

/* =========================
   END SCREEN
========================= */

function end(){

  document.querySelector(".card").style.display = "none";

  document.querySelector(".bottomBar").style.display = "none";

  document.getElementById("topArea").style.display = "none";

  document.getElementById("endScreen").style.display = "flex";

  const total =
    sCount + aCount + bCount + cCount;

  document.getElementById("stats").innerHTML =

    `⭐ S Tier: ${sCount}<br>
     🟢 A Tier: ${aCount}<br>
     🔵 B Tier: ${bCount}<br>
     🔴 C Tier: ${cCount}<br><br>
     📊 Ranked: ${total}`;
}

/* =========================
   RESTART
========================= */

function restart(){

  document.querySelector(".card").style.display = "block";

  document.querySelector(".bottomBar").style.display = "flex";

  document.getElementById("topArea").style.display = "flex";

  document.getElementById("endScreen").style.display = "none";

  start();
}

/* =========================
   BUTTONS
========================= */

document.getElementById("sTier").onclick =
  () => rate("s");

document.getElementById("aTier").onclick =
  () => rate("a");

document.getElementById("bTier").onclick =
  () => rate("b");

document.getElementById("cTier").onclick =
  () => rate("c");

document.getElementById("skip").onclick =
  skip;

document.getElementById("back").onclick =
  back;

/* =========================
   START
========================= */

start();

</script>

</body>
</html>