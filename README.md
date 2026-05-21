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

/* card */
.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  position: relative;
  margin-top: 10px;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
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
  font-size: 64px;
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

  <div class="flashScreen" id="flashScreen">
    <div id="flashText"></div>
  </div>

  <div class="card">
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
// IMAGE SETUP
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
  "IMG_5066.jpeg"
];

let images = [];

// =====================
// SHUFFLE
// =====================
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

// =====================
// STATE
// =====================
let index = 0;
let smash = 0;
let pass = 0;

// =====================
// START ROUND (NO REPEATS)
// =====================
function startRound() {
  images = shuffle([...imagesBase]); // fresh shuffle copy
  index = 0;
  smash = 0;
  pass = 0;
  load();
}

// =====================
// LOAD IMAGE
// =====================
function load() {
  if (index >= images.length) {
    end();
    return;
  }
  document.getElementById("img").src = images[index];
}

// =====================
// END SCREEN
// =====================
function end() {
  document.querySelector(".card img").style.display = "none";
  document.querySelector(".buttons").style.display = "none";

  const endScreen = document.getElementById("endScreen");
  const endScreenContent = document.getElementById("endScreenContent");

  endScreen.style.display = "flex";

  endScreenContent.innerHTML = `
    <h2>Done!</h2>
    <p style="font-size:20px;">🔥 Smashes: ${smash}</p>
    <p style="font-size:20px;">👎 Passes: ${pass}</p>
    <button onclick="restart()" 
      style="margin-top:10px; padding:10px 15px; border:none; border-radius:10px;">
      Play Again
    </button>
  `;
}

// =====================
// RESTART (RESHUFFLE)
// =====================
function restart() {
  document.querySelector(".card img").style.display = "block";
  document.querySelector(".buttons").style.display = "flex";
  document.getElementById("endScreen").style.display = "none";

  startRound();
}

// =====================
// FLASH
// =====================
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
  }, 700);

  index++;
  load();
}

// =====================
// SWIPE
// =====================
function swipe(dir) {

  if (navigator.vibrate) navigator.vibrate(120);

  if (dir === "right") {
    smash++;
    showFlash("SMASH 🔥", "#4dff88");
  } else {
    pass++;
    showFlash("PASS 👎", "#ff4d4d");
  }
}

// =====================
// BUTTONS
// =====================
document.getElementById("pass").onclick = () => swipe("left");
document.getElementById("smash").onclick = () => swipe("right");

// =====================
startRound();

</script>

</body>
</html>