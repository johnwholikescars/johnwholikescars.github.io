<!DOCTYPE html>
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
  transition: transform 0.2s ease;
  touch-action: none;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* swipe feedback overlays */
.overlay {
  position: absolute;
  top: 20px;
  font-size: 40px;
  font-weight: bold;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.like { left: 20px; color: #4dff88; }
.nope { right: 20px; color: #ff4d4d; }

/* buttons */
.buttons {
  position: fixed;
  bottom: 10px;
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

#pass { background: #ff4d4d; color: white; }
#smash { background: #4dff88; color: black; }

/* end screen */
#endScreen {
  position: absolute;
  text-align: center;
  display: none;
}
</style>
</head>

<body>

<div class="card" id="card">
  <div class="overlay like" id="likeText">SMASH 🔥</div>
  <div class="overlay nope" id="nopeText">PASS 👎</div>

  <img id="img" src="" />

  <div id="endScreen"></div>
</div>

<div class="buttons">
  <button id="pass">Pass 👎</button>
  <button id="smash">Smash 🔥</button>
</div>

<script>

// =======================
// IMAGES (GITHUB ROOT)
// =======================
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

// =======================
let index = 0;
let smash = 0;
let pass = 0;

const card = document.getElementById("card");
const img = document.getElementById("img");
const likeText = document.getElementById("likeText");
const nopeText = document.getElementById("nopeText");
const endScreen = document.getElementById("endScreen");

let startX = 0;
let currentX = 0;

// =======================
// LOAD IMAGE
// =======================
function load() {
  if (index >= images.length) {
    end();
    return;
  }
  img.src = images[index];
}

// =======================
// END SCREEN
// =======================
function end() {
  img.style.display = "none";
  document.querySelector(".buttons").style.display = "none";

  endScreen.style.display = "block";
  endScreen.innerHTML = `
    <h2>Done!</h2>
    <p>🔥 Smashes: ${smash}</p>
    <p>👎 Passes: ${pass}</p>
    <button onclick="restart()">Play Again</button>
  `;
}

// =======================
// RESTART
// =======================
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

// =======================
// SWIPE ACTION
// =======================
function swipe(dir) {

  if (dir === "right") {
    smash++;
    card.style.transform = "translateX(500px) rotate(20deg)";
    likeText.style.opacity = 1;
  } else {
    pass++;
    card.style.transform = "translateX(-500px) rotate(-20deg)";
    nopeText.style.opacity = 1;
  }

  // vibration (mobile)
  if (navigator.vibrate) navigator.vibrate(80);

  setTimeout(() => {
    card.style.transition = "none";
    card.style.transform = "translateX(0px) rotate(0deg)";
    likeText.style.opacity = 0;
    nopeText.style.opacity = 0;

    index++;
    load();

    setTimeout(() => {
      card.style.transition = "transform 0.2s ease";
    }, 50);

  }, 250);
}

// =======================
// BUTTONS
// =======================
document.getElementById("pass").onclick = () => swipe("left");
document.getElementById("smash").onclick = () => swipe("right");

// =======================
// DRAG SYSTEM (REAL FEEL)
// =======================
card.addEventListener("touchstart", (e) => {
  startX = e.touches[0].clientX;
});

card.addEventListener("touchmove", (e) => {
  currentX = e.touches[0].clientX;
  let diff = currentX - startX;

  card.style.transform = `translateX(${diff}px) rotate(${diff / 20}deg)`;

  likeText.style.opacity = diff > 50 ? 1 : 0;
  nopeText.style.opacity = diff < -50 ? 1 : 0;
});

card.addEventListener("touchend", () => {
  let diff = currentX - startX;

  if (diff > 100) swipe("right");
  else if (diff < -100) swipe("left");
  else {
    card.style.transform = "translateX(0px)";
    likeText.style.opacity = 0;
    nopeText.style.opacity = 0;
  }
});

// =======================
load();

</script>

</body>
</html>