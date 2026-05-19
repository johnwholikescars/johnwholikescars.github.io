# johnwholikescars.github.io
<!DOCTYPE html>
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
    }

    .car-container {
        margin-top: 20px;
    }

    img {
        width: 80%;
        max-width: 520px;
        border-radius: 15px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        transition: 0.3s;
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
const cars = [
    "https://upload.wikimedia.org/wikipedia/commons/3/3f/Ford_Mustang_GT_2015.jpg",
    "https://upload.wikimedia.org/wikipedia/commons/1/1e/2018_Dodge_Challenger_SRT_Hellcat.jpg",
    "https://upload.wikimedia.org/wikipedia/commons/9/9b/2017_Chevrolet_Camaro_SS.jpg",
    "https://upload.wikimedia.org/wikipedia/commons/7/7e/2015_BMW_M4_%2819902743480%29.jpg",
    "https://upload.wikimedia.org/wikipedia/commons/2/2d/Lamborghini_Huracan_LP610-4_2015.jpg"
];

let index = 0;

const carImage = document.getElementById("carImage");
const result = document.getElementById("result");

// show first car
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
        index = 0; // loop back
    }

    setTimeout(() => {
        carImage.src = cars[index];
    }, 300);
}
</script>

</body>
</html>