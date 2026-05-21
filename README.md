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

  /* 🔥 MOVE EVERYTHING UP */
  justify-content: center;
  align-items: flex-start;
  padding-top: 40px;

  font-family: Arial;
  color: white;
  overflow: hidden;
}

/* optional username/title area */
#username {
  position: fixed;
  top: 10px;
  font-size: 18px;
  opacity: 0.7;
}

.card {
  width: 320px;
  height: 520px; /* 🔥 taller card for better image space */
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

/* FLASH SCREEN */
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
</style>
</head>

<body>

<!-- 🔥 username display moved up -->
<div id="username">your_username_here</div>

<!-- FLASH -->
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

// images
let images = [];

for (let i = 5076; i >= 5066; i--) {
  images.push(`IMG_${i}.jpeg`);
}

function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

images = shuffle(images);

let index = 0;
let smash = 0;
let pass = 0;

const img = document.getElementById("img");
const flashScreen = document.getElementById("flashScreen");
const flashText = document.getElementById("flashText");

function load() {
  if (index >= images.length) return;
  img.src = images[index];
}

function showFlash(text, color) {

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

document.getElementById("pass").onclick = () => swipe("left");
document.getElementById("smash").onclick = () => swipe("right");

load();

</script>

</body>
</html>