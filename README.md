<SMASH OR PASS>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smash or Pass</title>

<style>
body {
    margin: 0;
    font-family: Arial;
    background: #0b1220;
    color: white;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.subtitle {
    position: fixed;
    top: 8px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 16px;
    opacity: 0.8;
    white-space: nowrap;
}

.card {
    width: 90%;
    max-width: 420px;
    position: relative;
    transition: transform 0.3s ease;
    touch-action: none;
}

.card img {
    width: 100%;
    max-height: 85vh;
    object-fit: contain;
    border-radius: 15px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}

.overlay {
    position: fixed;
    inset: 0;
    font-size: 80px;
    font-weight: bold;
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 0;
    pointer-events: none;
    transition: 0.2s;
    z-index: 999;
}
.smash { background: rgba(239,68,68,0.95); }
.pass  { background: rgba(59,130,246,0.95); }

.endScreen {
    position: fixed;
    inset: 0;
    background: #0b1220;
    display: none;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    font-size: 28px;
    z-index: 1000;
}

button {
    margin-top: 30px;
    padding: 12px 24px;
    font-size: 18px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}
</style>
</head>

<body>

<div class="subtitle">Swipe right to smash | Swipe left to pass</div>

<div class="card" id="card">
    <img id="carImage" src="">
</div>

<div class="overlay" id="overlay"></div>

<div class="endScreen" id="endScreen">
    <div id="results"></div>
    <button onclick="playAgain()">Play Again</button>
</div>

<script>
let cars = [
  "IMG_5043.jpeg","IMG_5041.jpeg","IMG_5040.jpeg","IMG_5038.jpeg",
  "IMG_5037.jpeg","IMG_5036.jpeg","IMG_5034.jpeg","IMG_5032.jpeg",
  "IMG_5031.jpeg","IMG_5030.jpeg"
];

let smashCount = 0, passCount = 0, index = 0;

const card = document.getElementById("card");
const img = document.getElementById("carImage");
const overlay = document.getElementById("overlay");
const endScreen = document.getElementById("endScreen");
const results = document.getElementById("results");

function shuffle(a){
  for (let i=a.length-1;i>0;i--){
    const j=Math.floor(Math.random()*(i+1));
    [a[i],a[j]]=[a[j],a[i]];
  }
}

function startGame(){
  shuffle(cars);
  index=0;
  smashCount=0;
  passCount=0;
  img.src=cars[index];
  endScreen.style.display="none";
}
startGame();

let startX=0,currentX=0;

function start(e){ startX=(e.touches?e.touches[0].clientX:e.clientX); }
function move(e){
  if(!startX)return;
  currentX=(e.touches?e.touches[0].clientX:e.clientX);
  let d=currentX-startX;
  card.style.transform=`translateX(${d}px) rotate(${d*0.05}deg)`;
}
function end(){
  let d=currentX-startX;
  if(d>120) swipe("smash");
  else if(d<-120) swipe("pass");
  else card.style.transform="translateX(0)";
  startX=0; currentX=0;
}

function swipe(type){
  overlay.className="overlay "+type;
  overlay.textContent=type.toUpperCase();
  overlay.style.opacity=1;
  setTimeout(()=>overlay.style.opacity=0,600);

  if(type==="smash") smashCount++; else passCount++;

  setTimeout(()=>{
    index++;
    if(index>=cars.length) showResults();
    else{
      img.src=cars[index];
      card.style.transform="translateX(0)";
    }
  },200);
}

function showResults(){
  endScreen.style.display="flex";
  results.innerHTML=`
    <div>Results</div>
    <div style="margin-top:20px;">Smashes: ${smashCount}</div>
    <div>Passes: ${passCount}</div>
  `;
}

function playAgain(){ startGame(); }

card.addEventListener("mousedown",start);
card.addEventListener("mousemove",move);
card.addEventListener("mouseup",end);
card.addEventListener("touchstart",start);
card.addEventListener("touchmove",move);
card.addEventListener("touchend",end);
</script>

</body>
</html>