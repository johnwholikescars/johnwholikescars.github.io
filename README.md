<SMASH OR PASS>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smash or Pass?</title>

<style>
    body {
        margin: 0;
        font-family: Arial, sans-serif;
        background: #0b1220;
        color: white;
        text-align: center;
        overflow: hidden;
    }

    h1 {
        margin-top: 20px;
    }

    .card {
        width: 85%;
        max-width: 420px;
        margin: 40px auto;
        position: relative;
        transition: transform 0.4s ease, opacity 0.4s ease;
        cursor: grab;
    }

    .card img {
        width: 100%;
        border-radius: 15px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.5);
    }

    .label {
        position: absolute;
        top: 20px;
        font-size: 32px;
        font-weight: bold;
        opacity: 0;
        transition: 0.2s;
    }

    .like {
        right: 20px;
        color: #22c55e;
    }

    .nope {
        left: 20px;
        color: #ef4444;
    }
</style>
</head>

<body>

<h1>Smash or Pass?</h1>
<p>Swipe right = Smash 🔨 | Swipe left = Pass ❌</p>

<div class="card" id="card">
    <div class="label like" id="like">SMASH 🔨</div>
    <div class="label nope" id="nope">PASS ❌</div>
    <img id="carImage" src="" alt="Car">
</div>

<script>

// 🚗 LOCAL IMAGES (your folder)
const cars = [
    "IMG_5026.jpeg"
    "IMG_5030.jpeg"
    "IMG_5031.jpeg"
    "IMG_5032.jpeg"
];

let index = 0;

const card = document.getElementById("card");
const img = document.getElementById("carImage");

const like = document.getElementById("like");
const nope = document.getElementById("nope");

// load first car
img.src = cars[index];

let startX = 0;
let currentX = 0;

// touch + mouse start
function startDrag(e) {
    startX = e.type.includes("mouse") ? e.clientX : e.touches[0].clientX;
}

function moveDrag(e) {
    if (!startX) return;

    currentX = e.type.includes("mouse") ? e.clientX : e.touches[0].clientX;
    let diff = currentX - startX;

    card.style.transform = `translateX(${diff}px) rotate(${diff * 0.05}deg)`;

    // show labels
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

function endDrag() {
    let diff = currentX - startX;

    if (diff > 120) {
        swipeRight(); // smash
    } else if (diff < -120) {
        swipeLeft(); // pass
    } else {
        resetCard();
    }

    startX = 0;
    currentX = 0;
}

function swipeRight() {
    card.style.transform = "translateX(600px) rotate(30deg)";
    nextCar();
}

function swipeLeft() {
    card.style.transform = "translateX(-600px) rotate(-30deg)";
    nextCar();
}

function resetCard() {
    card.style.transform = "translateX(0)";
    like.style.opacity = 0;
    nope.style.opacity = 0;
}

function nextCar() {
    setTimeout(() => {
        index++;
        if (index >= cars.length) index = 0;

        img.src = cars[index];
        resetCard();
    }, 300);
}

// events
card.addEventListener("mousedown", startDrag);
card.addEventListener("mousemove", moveDrag);
card.addEventListener("mouseup", endDrag);

card.addEventListener("touchstart", startDrag);
card.addEventListener("touchmove", moveDrag);
card.addEventListener("touchend", endDrag);

</script>

</body>
</html>