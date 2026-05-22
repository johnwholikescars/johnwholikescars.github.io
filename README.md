<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Smash or Pass</title>

<style>
body {
  margin: 0;
  background: #0f0f0f;
  font-family: Arial;
  color: white;
  overflow: hidden;
}

/* TOP BAR */
.topBar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  background: #111;
  z-index: 1000;
}

#counter {
  font-size: 18px;
  font-weight: bold;
}

.topButtons {
  display: flex;
  gap: 8px;
}

/* APP */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 80px;
}

/* CARD */
.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  position: relative;
  margin-top: 10px;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* animations */
.fly-right {
  transform: translateX(500px) rotate(20deg);
  opacity: 0;
}

.fly-left {
  transform: translateX(-500px) rotate(-20deg);
  opacity: 0;
}

.fly-up {
  transform: translateY(-500px);
  opacity: 0;
}

/* FLASH */
.flashScreen {
  position: fixed;
  inset: 0;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s ease;
  z-index: 999;
}

.flashShow {
  opacity: 1;
}

#flashText {
  font-size: 54px;
  font-weight: 900;
}

/* BOTTOM BAR */
.bottomBar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 12px;
  display: flex;
  justify-content: center;
  gap: 10px;
  background: #111;
}

button {
  padding: 12px 14px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 14px;
}

#pass { background: #ff4d4d; color: white; }
#smash { background: #4dff88; color: black; }
#skip { background: #4da6ff; color: white; }
#back { background: #888; color: white; }

/* END SCREEN */
#endScreen {
  position: fixed;
  inset: 0;
  display: none;
  align-items: center;
  justify-content: center;
  text-align: center;
  background: rgba(0,0,0,0.9);
  z-index: 2000;
}
</style>
</head>

<body>

<!-- TOP BAR -->
<div class="topBar">

  <div id="counter">1 / 1</div>

  <div class="topButtons">
    <button id="back">⬅</button>
    <button id="skip">⏭</button>
  </div>

</div>

<div id="app">

  <div class="flashScreen" id="flashScreen">
    <div id="flashText"></div>
  </div>

  <div class="card" id="card">
    <img id="img" src="" />
  </div>

</div>

<!-- BOTTOM BAR -->
<div class="bottomBar">
  <button id="pass">Pass 👎</button>
  <button id="smash">Smash 🔥</button>
</div>

<!-- END -->
<div id="endScreen">
  <div>
    <h1>Finished!</h1>
    <p id="stats"></p>
    <button onclick="restart()">Play Again</button>
  </div>
</div>

<script>

// IMAGES (UNCHANGED)
let imagesBase = [
  "IMG_5076.jpeg","IMG_5075.jpeg","IMG_5074.jpeg","IMG_5073.jpeg",
  "IMG_5072.jpeg","IMG_5071.jpeg","IMG_5070.jpeg","IMG_5069.jpeg",
  "IMG_5068.jpeg","IMG_5067.jpeg","IMG_5066.jpeg","IMG_5083.jpeg",
  "IMG_5098.jpeg","IMG_5097.jpeg","IMG_5115.jpeg","IMG_5116.jpeg",
  "IMG_5118.jpeg","IMG_5117.jpeg","IMG_5119.jpeg","IMG_5122.jpeg",
  "IMG_5120.jpeg","IMG_5121.jpeg","IMG_5125.jpeg","IMG_5124.jpeg",
  "IMG_5123.jpeg","IMG_5128.jpeg","IMG_5127.jpeg","IMG_5126.jpeg",
  "IMG_5163.jpeg","IMG_5162.jpeg","IMG_5161.jpeg","IMG_5160.jpeg",
  "IMG_5159.jpeg","IMG_5158.jpeg","IMG_5157.jpeg","IMG_5156.jpeg",
  "IMG_5155.jpeg","IMG_5154.jpeg","IMG_5153.jpeg","IMG_5152.jpeg",
  "IMG_5151.jpeg","IMG_5150.jpeg","IMG_5149.jpeg"
];

let images = [];
let skipped = [];
let history = [];

let index = 0;
let smash = 0;
let pass = 0;

// shuffle
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

// preload
function preload() {
  imagesBase.forEach(src => {
    const img = new Image();
    img.src = src;
  });
}

// start
function start() {
  images = shuffle([...imagesBase]);
  index = 0;
  smash = 0;
  pass = 0;
  skipped = [];
  history = [];
  load();
}

// counter
function updateCounter() {
  document.getElementById("counter").innerText =
    `${index + 1} / ${images.length}`;
}

// load
function load() {

  if (index >= images.length && skipped.length > 0) {
    images = skipped;
    skipped = [];
    index = 0;
  }

  if (index >= images.length) {
    end();
    return;
  }

  document.getElementById("img").src = images[index];
  updateCounter();
}

// flash
function flash(text, color) {
  const f = document.getElementById("flashScreen");
  const t = document.getElementById("flashText");

  t.innerText = text;
  t.style.color = color;

  f.classList.add("flashShow");
  setTimeout(() => f.classList.remove("flashShow"), 500);
}

// swipe
function swipe(type) {

  const card = document.getElementById("card");
  history.push(index);

  if (type === "smash") {
    smash++;
    card.classList.add("fly-right");
    flash("SMASH 🔥", "#4dff88");
  }

  if (type === "pass") {
    pass++;
    card.classList.add("fly-left");
    flash("PASS 👎", "#ff4d4d");
  }

  setTimeout(() => {
    card.classList.remove("fly-right", "fly-left");
    index++;
    load();
  }, 350);
}

// skip
function skip() {
  const card = document.getElementById("card");

  skipped.push(images[index]);
  history.push(index);

  card.classList.add("fly-up");
  flash("SKIP ⏭", "#4da6ff");

  setTimeout(() => {
    card.classList.remove("fly-up");
    index++;
    load();
  }, 350);
}

// back
function back() {
  if (history.length === 0) return;
  index = history.pop();
  load();
}

// end
function end() {

  document.querySelector(".card").style.display = "none";
  document.querySelector(".bottomBar").style.display = "none";
  document.querySelector(".topBar").style.display = "none";

  document.getElementById("endScreen").style.display = "flex";

  const total = smash + pass;
  const percent = total ? Math.round((smash / total) * 100) : 0;

  document.getElementById("stats").innerHTML = `
    🔥 Smashes: ${smash}<br>
    👎 Passes: ${pass}<br>
    📊 Smash Rate: ${percent}%
  `;
}

// restart
function restart() {
  document.querySelector(".card").style.display = "block";
  document.querySelector(".bottomBar").style.display = "flex";
  document.querySelector(".topBar").style.display = "flex";
  document.getElementById("endScreen").style.display = "none";
  start();
}

// buttons
document.getElementById("smash").onclick = () => swipe("smash");
document.getElementById("pass").onclick = () => swipe("pass");
document.getElementById("skip").onclick = skip;
document.getElementById("back").onclick = back;

// start
preload();
start();

</script>

</body>
</html>