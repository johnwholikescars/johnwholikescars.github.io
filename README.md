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
    text-align: center;
    overflow: hidden;
}

h1 {
    margin-top: 20px;
}

.subtitle {
    margin-top: 5px;
    font-size: 16px;
    opacity: 0.8;
}

.card {
    width: 85%;
    max-width: 420px;
    margin: 40px auto;
    position: relative;
    transition: transform 0.3s ease;
    touch-action: none;
}

.card img {
    width: 100%;
    border-radius: 15px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}

.label {
    position: absolute;
    top: 20px;
    font-size: 30px;
    font-weight: bold;
    opacity: 0;
}

.like {
    right: 20px;
    color: #22c55e;
}

.nope {
    left: 20px;
    color: #ef4444;
}

/* 🔥 OVERLAY SCREEN */
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 80px;
    font-weight: bold;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.2s ease;
    z-index: 999;
}

.overlay.smash {
    background: rgba(239, 68, 68, 0.9);
    color: white;
}

.overlay.pass {
    background: rgba(59, 130, 246, 0.9);
    color: white;
}

/* footer */
.footer {
    position: fixed;
    bottom: 10px;
    width: 100%;
    font-size: 12px;
    opacity: 0.5;
}
</style>
</head>

<body>

<h1>Smash or Pass</h1>
<div class="subtitle">Swipe right to smash | Swipe left to pass</div>

<div class="card" id="card">
    <div class="label like" id="like">SMASH</div>
    <div class="label nope" id="nope">PASS</div>
    <img id="carImage" src="" alt="Car">
</div>

<!-- 🔥 OVERLAY -->
<div class="overlay smash" id="overlay">SMASH</div>

<div class="footer">idea by Archer</div>

<script>

let cars = [
  "IMG_5034.jpeg",
  "IMG_5030.jpeg",
  "IMG_5031.jpeg",
  "IMG_5032.jpeg"
];

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
const like = document.getElementById("like");
const nope = document.getElementById("nope");
const overlay = document.getElementById("overlay");

img.src = cars[index];

let startX = 0;
let currentX = 0;

function showOverlay(type) {
    overlay.className = "overlay " + type;
    overlay.textContent = type.toUpperCase();
    overlay.style.opacity = 1;

    setTimeout(() => {
        overlay.style.opacity = 0;
    }, 900);
}

function start(e) {
    startX = e.touches ? e.touches[0].clientX : e.clientX;
}

function move(e) {
    if (!startX) return;

    currentX = e.touches ? e.touches[0].clientX : e.clientX;
    let diff = currentX - startX;

    card.style.transform = `translateX(${diff}px) rotate(${diff * 0.05}deg)`;

    if (diff > 80) {
        like.style.opacity = 1;
        nope.style.opacity = 0;
    } else if (diff < -80) {
        like.style.opacity = 0;
        nope.style.opacity = 1;
    } else {
        like.style.opacity = 0;
        nope.style.opacity = 0;
    }
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

function swipeRight() {
    showOverlay("smash");
    card.style.transform = "translateX(500px) rotate(20deg)";
    nextCar();
}

function swipeLeft() {
    showOverlay("pass");
    card.style.transform = "translateX(-500px) rotate(-20deg)";
    nextCar();
}

function reset() {
    card.style.transform = "translateX(0)";
    like.style.opacity = 0;
    nope.style.opacity = 0;
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