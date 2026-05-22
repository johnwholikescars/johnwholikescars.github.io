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

/* layout */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 10px;
}

/* progress counter */
#counter {
  font-size: 20px;
  margin-bottom: 10px;
  font-weight: bold;
}

/* card */
.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  position: relative;
  margin-top: 10px;
  transition: transform 0.45s ease, opacity 0.45s ease;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* fly animations */
.fly-right {
  transform: translateX(500px) rotate(20deg);
  opacity: 0;
}

.fly-left {
  transform: translateX(-500px) rotate(-20deg);
  opacity: 0;
}

/* flash */
.flashScreen {
  position: fixed;
  inset: 0;
  background: #000;
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
  white-space: nowrap;
  text-align: center;
  padding: 20px;
  text-shadow: 0 0 25px rgba(255,255,255,0.3);
}

/* buttons */
.buttons {
  position: fixed;
  bottom: 20px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 20px;
}

button {
  padding: 12px 18px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 16px;
}

#pass {
  background: #ff4d4d;
  color: white;
}

#smash {
  background: #4dff88;
  color: black;
}

/* end screen */
#endScreen {
  position: absolute;
  inset: 0;
  display: none;
  align-items: center;
  justify-content: center;
  text-align: center;
}

#endScreenContent {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
</head>

<body>

<div id="app">

  <!-- progress -->
  <div id="counter">1 / 1</div>

  <div class="flashScreen" id="flashScreen">
    <div id="flashText"></div>
  </div>

  <div class="card" id="card">
    <img id="img" src="" />

    <div id="endScreen">
      <div id="endScreenContent"></div>
    </div>
  </div>

</div>

<div class="buttons">
  <button id="pass">Pass 👎</button>
  <button id="smash">Smash 🔥</button>
</div>

<script>

// =====================
// IMAGES
// =====================
let imagesBase = [
  "IMG_5076.jpeg",
  "IMG_5075.jpeg",
  "IMG_5074.jpeg",
  "IMG_5073.jpeg",
  "IMG_5072.jpeg",
  "IMG_5071.jpeg",
  "IMG_5070.jpeg",
  "IMG_5069.jpeg",
  "IMG_5068.jpeg",
  "IMG_5067.jpeg",
  "IMG_5066.jpeg",
  "IMG_5083.jpeg",
  "IMG_5098.jpeg",
  "IMG_5097.jpeg",
  "IMG_5115.jpeg",
  "IMG_5116.jpeg",
  "IMG_5118.jpeg",
  "IMG_5117.jpeg",
  "IMG_5119.jpeg",
  "IMG_5122.jpeg",
  "IMG_5120.jpeg",
  "IMG_5121.jpeg",
  "IMG_5125.jpeg",
  "IMG_5124.jpeg",
  "IMG_5123.jpeg",
  "IMG_5128.jpeg",
  "IMG_5127.jpeg",
  "IMG_5126.jpeg",
  "IMG_5163.jpeg",
  "IMG_5162.jpeg",
  "IMG_5161.jpeg",
  "IMG_5160.jpeg",
  "IMG_5159.jpeg",
  "IMG_5158.jpeg",
  "IMG_5157.jpeg",
  "IMG_5156.jpeg",
  "IMG_5155.jpeg",
  "IMG_5154.jpeg",
  "IMG_5153.jpeg",
  "IMG_5152.jpeg",
  "IMG_5151.jpeg",
  "IMG_5150.jpeg",
  "IMG_5149.jpeg"
  
];

let images = [];

// shuffle
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

// state
let index = 0;
let smash = 0;
let pass = 0;

// preload images
function preloadImages() {
  imagesBase.forEach(src => {
    const img = new Image();
    img.src = src;
  });
}

// start round
function startRound() {

  images = shuffle([...imagesBase]);

  index = 0;
  smash = 0;
  pass = 0;

  load();
}

// update counter
function updateCounter() {
  document.getElementById("counter").innerText =
    `${index + 1} / ${images.length}`;
}

// load image
function load() {

  if (index >= images.length) {
    end();
    return;
  }

  document.getElementById("img").src = images[index];

  updateCounter();
}

// end screen
function end() {

  document.querySelector(".card img").style.display = "none";
  document.querySelector(".buttons").style.display = "none";
  document.getElementById("counter").style.display = "none";

  const endScreen = document.getElementById("endScreen");
  const endScreenContent = document.getElementById("endScreenContent");

  endScreen.style.display = "flex";

  const total = smash + pass;
  const smashPercent = Math.round((smash / total) * 100);

  endScreenContent.innerHTML = `
    <h2>Done!</h2>

    <p style="font-size:20px;">🔥 Smashes: ${smash}</p>

    <p style="font-size:20px;">👎 Passes: ${pass}</p>

    <p style="font-size:18px;">
      Smash Rate: ${smashPercent}%
    </p>

    <button onclick="restart()" 
      style="margin-top:10px; padding:10px 15px; border:none; border-radius:10px;">
      Play Again
    </button>
  `;
}

// restart
function restart() {

  document.querySelector(".card img").style.display = "block";
  document.querySelector(".buttons").style.display = "flex";
  document.getElementById("counter").style.display = "block";

  document.getElementById("endScreen").style.display = "none";

  startRound();
}

// flash effect
function showFlash(text, color) {

  const flashScreen = document.getElementById("flashScreen");
  const flashText = document.getElementById("flashText");

  flashScreen.classList.remove("flashShow");
  void flashScreen.offsetWidth;

  flashText.innerText = text;
  flashText.style.color = color;

  flashScreen.classList.add("flashShow");

  setTimeout(() => {
    flashScreen.classList.remove("flashShow");
  }, 600);
}

// swipe logic
function swipe(dir) {

  if (navigator.vibrate) navigator.vibrate(120);

  const card = document.getElementById("card");

  if (dir === "right") {

    smash++;

    card.classList.add("fly-right");

    showFlash("SMASH 🔥", "#4dff88");

  } else {

    pass++;

    card.classList.add("fly-left");

    showFlash("PASS 👎", "#ff4d4d");
  }

  setTimeout(() => {

    card.classList.remove("fly-right");
    card.classList.remove("fly-left");

    index++;

    load();

  }, 400);
}

// buttons
document.getElementById("pass").onclick = () => swipe("left");

document.getElementById("smash").onclick = () => swipe("right");

// preload
preloadImages();

// start
startRound();

</script>

</body>
</html>