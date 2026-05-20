<SMASH OR PASS>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smash or Pass</title>

<style>
body {
    margin: 0;
    font-family: Arial;
    background: #0b1220;
    color: white;
    overflow: hidden;

    /* center everything vertically */
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

/* CARD AREA */
.card {
    width: 90%;
    max-width: 420px;
    position: relative;
    transition: transform 0.3s ease;
    touch-action: none;
}

.card img {
    width: 100%;
    max-height: 85vh;   /* prevents tall images going off screen */
    object-fit: contain;
    border-radius: 15px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}

/* OVERLAY */
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    font-size: 80px;
    font-weight: bold;
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 0;
    pointer-events: none;
    transition: 0.2s;
    z-index: 999;
}

.smash {
    background: rgba(239, 68, 68, 0.95);
    color: white;
}

.pass {
    background: rgba(59, 130, 246, 0.95);
    color: white;
}
</style>
</head>

<body>

<div class="card" id="card">
    <img id="carImage" src="">
</div>

<div class="overlay smash" id="overlay"></div>

<script>

let cars = [
  "IMG_5043.jpeg",
  "IMG_5041.jpeg",
  "IMG_5040.jpeg",
  "IMG_5038.jpeg",
  "IMG_5037.jpeg",
  "IMG_5036.jpeg",
  "IMG_5034.jpeg",
  "IMG_5032.jpeg",
  "IMG_5031.jpeg",
  "IMG_5030.jpeg",
];

// shuffle
function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}

shuffle(cars);

let index = 0;

const card = document.getElementById("card");
const img = document.getElementById("carImage");
const overlay = document.getElementById("overlay");

img.src = cars[index];

let startX = 0;
let currentX = 0;

function start(e) {
    startX = e.touches ? e.touches[0].clientX : e.clientX;
}

function move(e) {
    if (!startX) return;

    currentX = e.touches ? e.touches[0].clientX : e.clientX;
    let diff = currentX - startX;

    card.style.transform = `translateX(${diff}px) rotate(${diff * 0.05}deg)`;
}

function end() {
    let diff = currentX - startX;

    if (diff > 120) {
        swipeRight();
    } else if (diff < -120) {
        swipeLeft();
    } else {
        reset();
    }

    startX = 0;
    currentX = 0;
}

function showOverlay(type) {
    overlay.className = "overlay " + type;
    overlay.textContent = type.toUpperCase();
    overlay.style.opacity = 1;

    setTimeout(() => {
        overlay.style.opacity = 0;
    }, 600);
}

function swipeRight() {
    showOverlay("smash");
    nextCar();
}

function swipeLeft() {
    showOverlay("pass");
    nextCar();
}

function reset() {
    card.style.transform = "translateX(0)";
}

function nextCar() {
    setTimeout(() => {
        index++;

        if (index >= cars.length) {
            shuffle(cars);
            index = 0;
        }

        img.src = cars[index];
        reset();
    }, 200);
}

card.addEventListener("mousedown", start);
card.addEventListener("mousemove", move);
card.addEventListener("mouseup", end);

card.addEventListener("touchstart", start);
card.addEventListener("touchmove", move);
card.addEventListener("touchend", end);

</script>

</body>
</html>