<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Swipe Test</title>

<style>
body {
  margin: 0;
  background: #111;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-family: Arial;
  color: white;
}

.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.swipe-right {
  transform: translateX(400px) rotate(15deg);
  opacity: 0;
}

.swipe-left {
  transform: translateX(-400px) rotate(-15deg);
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
  font-size: 16px;
  cursor: pointer;
}

#left { background: #ff4d4d; color: white; }
#right { background: #4dff88; color: black; }
</style>
</head>

<body>

<div class="card" id="card">
  <img id="img" src="" />
</div>

<div class="buttons">
  <button id="left">Pass 👎</button>
  <button id="right">Smash 🔥</button>
</div>

<script>
/* -----------------------
   IMAGE TEST (ONLY 1)
------------------------*/

// CHANGE THIS IF NEEDED
let images = ["IMG_5076.jpeg"];

let index = 0;

const card = document.getElementById("card");
const img = document.getElementById("img");

function loadImage() {
  console.log("Loading:", images[index]);

  if (!images[index]) return;

  img.src = "images/" + images[index];
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
    loadImage();
  }, 400);
}

// buttons
document.getElementById("left").onclick = () => swipe("left");
document.getElementById("right").onclick = () => swipe("right");

/* -----------------------
   TOUCH SWIPE
------------------------*/
let startX = 0;

card.addEventListener("touchstart", (e) => {
  startX = e.touches[0].clientX;
});

card.addEventListener("touchend", (e) => {
  let endX = e.changedTouches[0].clientX;

  if (endX - startX > 80) swipe("right");
  else if (startX - endX > 80) swipe("left");
});

/* -----------------------
   INIT
------------------------*/
loadImage();
</script>

</body>
</html>