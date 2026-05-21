<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Smash or Pass</title>

<style>
body {
  margin: 0;
  background: #111;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: Arial;
  color: white;
}

.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  position: relative;
  transition: transform 0.4s ease, opacity 0.4s ease;
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
  position: fixed;
  bottom: 40px;
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
</style>
</head>

<body>

<div class="card" id="card">
  <img id="img" src="" />
</div>

<div class="buttons">
  <button id="pass">Pass 👎</button>
  <button id="smash">Smash 🔥</button>
</div>

<script>

// 🔥 YOUR IMAGES (ROOT LEVEL — NO "images/" FOLDER)
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

let index = 0;

const img = document.getElementById("img");
const card = document.getElementById("card");

function load() {
  if (index >= images.length) {
    card.innerHTML = "<h2>No more images</h2>";
    return;
  }

  // ✅ IMPORTANT FIX: NO "images/" PREFIX
  img.src = images[index];
}

function swipe(dir) {
  if (dir === "right") {
    card.classList.add("swipe-right");
  } else {
    card.classList.add("swipe-left");
  }

  setTimeout(() => {
    card.classList.remove("swipe-right", "swipe-left");
    index++;
    load();
  }, 400);
}

// buttons
document.getElementById("pass").onclick = () => swipe("left");
document.getElementById("smash").onclick = () => swipe("right");

// touch swipe
let startX = 0;

card.addEventListener("touchstart", e => {
  startX = e.touches[0].clientX;
});

card.addEventListener("touchend", e => {
  let endX = e.changedTouches[0].clientX;

  if (endX - startX > 80) swipe("right");
  else if (startX - endX > 80) swipe("left");
});

load();

</script>

</body>
</html>