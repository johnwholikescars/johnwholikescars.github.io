<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Smash or Pass</title>

<style>
body {
  margin: 0;
  background: #0f0f0f;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: Arial;
  color: white;
  overflow: hidden;
}

.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  position: relative;
  touch-action: none;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* 🔥 FULL SCREEN FLASH */
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

/* 🔥 SMALLER TEXT FIX */
#flashText {
  font-size: 64px; /* 👈 smaller so it stays on screen */
  font-weight: 900;
  white-space: nowrap;
  transform: scale(0.9);
  transition: transform 0.2s ease;
  text-shadow: 0 0 25px rgba(255,255,255,0.3);
  text-align: center;
  padding: 20px;
  max-width: 90vw;
}

.flashShow #flashText {
  transform: scale(1);
}

/* buttons */
.buttons {
  position: fixed;
  bottom: 15px;
  display: flex;
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

<!-- 🔥 FLASH SCREEN -->
<div class="flashScreen" id="flashScreen">
  <div id="flashText"></div>
</div>

<div class="card">

  <img id="img" src="" />

  <div id="endScreen">
    <div id="endScreenContent"></div>
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
let images = [];

for (let i = 5076; i >= 5066; i--) {
  images.push(`IMG_${i}.jpeg`);
}

// shuffle
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

images = shuffle(images);

// =====================
let index = 0;
let smash = 0;
let pass = 0;

const img = document.getElementById("img");

const flashScreen = document.getElementById("flashScreen");
const flashText = document.getElementById("flashText");

const endScreen = document.getElementById("endScreen");
const endScreenContent = document.getElementById("endScreenContent");

let startX = 0;
let currentX = 0;

// =====================
// LOAD IMAGE
// =====================
function load() {

  if (index >= images.length) {
    end();
    return;
  }

  img.src = images[index];
}

// =====================
// END SCREEN
// =====================
function end() {

  img.style.display = "none";
  document.querySelector(".buttons").style.display = "none";

  endScreen.style.display = "flex";

  endScreenContent.innerHTML = `
    <h2>Done!</h2>

    <p style="font-size:20px;">
      🔥 Smashes: ${smash}
    </p>

    <p style="font-size:20px;">
      👎 Passes: ${pass}
    </p>

    <button onclick="restart()"
      style="
      margin-top:10px;
      padding:10px 15px;
      border:none;
      border-radius:10px;
      cursor:pointer;
    ">
      Play Again
    </button>
  `;
}

// =====================
// RESTART
// =====================
function restart() {

  index = 0;
  smash = 0;
  pass = 0;

  images = shuffle(images);

  img.style.display = "block";
  document.querySelector(".buttons").style.display = "flex";
  endScreen.style.display = "none";

  load();
}

// =====================
// 🔥 FLASH EFFECT
// =====================
function showFlash(text, color) {

  // reset animation
  flashScreen.classList.remove("flashShow");
  void flashScreen.offsetWidth;

  flashText.innerText = text;
  flashText.style.color = color;

  flashScreen.classList.add("flashShow");

  // load next image while hidden
  index++;
  load();

  setTimeout(() => {
    flashScreen.classList.remove("flashShow");
  }, 1000);
}

// =====================
// SWIPE
// =====================
function swipe(dir) {

  if (navigator.vibrate) {
    navigator.vibrate(120);
  }

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
// TOUCH SWIPE
// =====================
document.querySelector(".card").addEventListener("touchstart", e => {
  startX = e.touches[0].clientX;
});

document.querySelector(".card").addEventListener("touchend", e => {

  currentX = e.changedTouches[0].clientX;

  let diff = currentX - startX;

  if (diff > 100) {
    swipe("right");
  }

  else if (diff < -100) {
    swipe("left");
  }
});

// =====================
load();

</script>

</body>
</html>