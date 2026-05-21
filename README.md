<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Smash or Pass Swipe</title>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #0f0f0f;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
  transition: transform 0.4s ease, opacity 0.4s ease;
  background: #222;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.swipe-right {
  transform: translateX(450px) rotate(18deg);
  opacity: 0;
}

.swipe-left {
  transform: translateX(-450px) rotate(-18deg);
  opacity: 0;
}

.buttons {
  display: flex;
  gap: 15px;
}

button {
  padding: 12px 18px;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  cursor: pointer;
}

#passBtn {
  background: #ff4d4d;
  color: white;
}

#smashBtn {
  background: #4dff88;
  color: black;
}
</style>
</head>

<body>

<div class="app">
  <div class="card" id="card">
    <img id="img" src="" />
  </div>

  <div class="buttons">
    <button id="passBtn">👎 Pass</button>
    <button id="smashBtn">🔥 Smash</button>
  </div>
</div>

<script>
// -------------------------
// IMAGE ARRAY (YOUR FILES)
// -------------------------
let images = [
  "IMG_5043.jpeg",
  "IMG_37.jpeg",
  "IMG_31.jpeg",
  "IMG_41.jpeg",
  "IMG_36.jpeg",
  "IMG_30.jpeg",
  "IMG_40.jpeg",
  "IMG_34.jpeg"
];

// -------------------------
// SHUFFLE FUNCTION
// -------------------------
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}

images = shuffle(images);

// -------------------------
let index = 0;

const card = document.getElementById("card");
const img = document.getElementById("img");

// -------------------------
function loadCard() {
  if (index >= images.length) {
    card.innerHTML = "<h2>No more images</h2>";
    return;
  }

  img.src = "images/" + images[index];
}

// -------------------------
function swipe(dir) {
  if (dir === "right") {
    card.classList.add("swipe-right");
  } else {
    card.classList.add("swipe-left");
  }

  setTimeout(() => {
    card.classList.remove("swipe-right", "swipe-left");
    index++;
    loadCard();
  }, 400);
}

// -------------------------
// BUTTONS
// -------------------------
document.getElementById("passBtn").onclick = () => swipe("left");
document.getElementById("smashBtn").onclick = () => swipe("right");

// -------------------------
// TOUCH SWIPE
// -------------------------
let startX = 0;

card.addEventListener("touchstart", (e) => {
  startX = e.touches[0].clientX;
});

card.addEventListener("touchend", (e) => {
  let endX = e.changedTouches[0].clientX;

  if (endX - startX > 80) swipe("right");
  else if (startX - endX > 80) swipe("left");
});

// -------------------------
loadCard();
</script>

</body>
</html>