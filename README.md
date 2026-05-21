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
  transition: transform 0.25s ease;
  touch-action: none;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* 🔥 BIGGER + CLEARER OVERLAYS */
.overlay {
  position: absolute;
  top: 40px;
  font-size: 60px;
  font-weight: 900;
  opacity: 0;
  transition: opacity 0.2s ease;
  text-shadow: 0 0 15px rgba(0,0,0,0.8);
}

.like {
  left: 20px;
  color: #4dff88;
}

.nope {
  right: 20px;
  color: #ff4d4d;
}

/* buttons lower */
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

#pass { background: #ff4d4d; color: white; }
#smash { background: #4dff88; color: black; }

/* 📊 FIXED END SCREEN CENTERING */
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
  justify-content: center;
}

/* smoother swipe exit */
.swipe-right {
  transform: translateX(520px) rotate(25deg);
  opacity: 0;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

.swipe-left {
  transform: translateX(-520px) rotate(-25deg);
  opacity: 0;
  transition: transform 0.4s ease, opacity 0.4s ease;
}
</style>
</head>

<body>

<div class="card" id="card">

  <div class="overlay like" id="likeText">SMASH 🔥</div>
  <div class="overlay nope" id="nopeText">PASS 👎</div>

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
// IMAGES (ROOT)
// =====================
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

// =====================
let index = 0;
let smash = 0;
let pass = 0;

const card = document.getElementById("card");
const img = document.getElementById("img");
const likeText = document.getElementById("likeText");
const nopeText = document.getElementById("nopeText");
const endScreen = document.getElementById("endScreen");
const endScreenContent = document.getElementById("endScreenContent");

let startX = 0;
let currentX = 0;

// =====================
function load() {
  if (index >= images.length) {
    end();
    return;
  }
  img.src = images[index];
}

// =====================
// END SCREEN FIXED
// =====================
function end() {
  img.style.display = "none";
  document.querySelector(".buttons").style.display = "none";

  endScreen.style.display = "flex";

  endScreenContent.innerHTML = `
    <h2>Done!</h2>
    <p style="font-size:20px;">🔥 Smashes: ${smash}</p>
    <p style="font-size:20px;">👎 Passes: ${pass}</p>
    <button onclick="restart()" style="margin-top:10px; padding:10px 15px; border:none; border-radius:10px;">
      Play Again
    </button>
  `;
}

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
// SWIPE (LONGER + CLEARER EFFECT)
// =====================
function swipe(dir) {

  if (dir === "right") {
    smash++;
    card.classList.add("swipe-right");
    likeText.style.opacity = 1;
  } else {
    pass++;
    card.classList.add("swipe-left");
    nopeText.style.opacity = 1;
  }

  // longer visible effect
  setTimeout(() => {
    if (navigator.vibrate) navigator.vibrate(120);

    setTimeout(() => {
      card.classList.remove("swipe-right", "swipe-left");
      likeText.style.opacity = 0;
      nopeText.style.opacity = 0;

      index++;
      load();
    }, 250);

  }, 150);
}

// buttons
document.getElementById("pass").onclick = () => swipe("left");
document.getElementById("smash").onclick = () => swipe("right");

// =====================
// DRAG SWIPE
// =====================
card.addEventListener("touchstart", e => {
  startX = e.touches[0].clientX;
});

card.addEventListener("touchmove", e => {
  currentX = e.touches[0].clientX;
  let diff = currentX - startX;

  card.style.transform = `translateX(${diff}px) rotate(${diff / 18}deg)`;

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

load();

</script>

</body>
</html>