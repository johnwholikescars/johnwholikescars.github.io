<SMASH OR PASS>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smash or Pass 🚗</title>

<style>
    body {
        margin: 0;
        font-family: Arial, sans-serif;
        background: #0b1220;
        color: white;
        text-align: center;
    }

    h1 {
        margin-top: 25px;
        font-size: 2.2em;
    }

    .car-container {
        margin-top: 20px;
    }

    img {
        width: 80%;
        max-width: 520px;
        border-radius: 15px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        transition: 0.3s ease;
    }

    .buttons {
        margin-top: 25px;
        display: flex;
        justify-content: center;
        gap: 20px;
    }

    button {
        padding: 15px 35px;
        font-size: 18px;
        border: none;
        border-radius: 12px;
        cursor: pointer;
        transition: 0.2s;
    }

    .smash {
        background: #ef4444;
        color: white;
    }

    .pass {
        background: #3b82f6;
        color: white;
    }

    button:hover {
        transform: scale(1.07);
    }

    .result {
        margin-top: 20px;
        font-size: 22px;
        font-weight: bold;
    }
</style>
</head>

<body>

<h1>Smash or Pass? 🚗</h1>

<div class="car-container">
    <img id="carImage" src="" alt="Car">
</div>

<div class="buttons">
    <button class="smash" onclick="nextCar('smash')">🔨 Smash</button>
    <button class="pass" onclick="nextCar('pass')">❌ Pass</button>
</div>

<div class="result" id="result"></div>

<script>
// 🚗 Reliable car images (Wikimedia - won’t break on GitHub Pages)
const cars = [
let index = 0;

const carImage = document.getElementById("carImage");
const result = document.getElementById("result");

// show first car immediately
carImage.src = cars[index];

function nextCar(choice) {

    if (choice === "smash") {
        result.innerHTML = "🔨 SMASH!";
        result.style.color = "#ef4444";
    } else {
        result.innerHTML = "❌ PASS!";
        result.style.color = "#3b82f6";
    }

    // move to next car
    index++;

    if (index >= cars.length) {
        index = 0;
    }

    // small delay so user sees result first
    setTimeout(() => {
        carImage.src = cars[index];
    }, 250);
}
</script>

</body>
</html>