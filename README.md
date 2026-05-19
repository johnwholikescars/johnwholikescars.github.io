<SMASH OR PASS>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smash or Pass</title>

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
    }

    .card img {
        width: 100%;
        border-radius: 15px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        user-select: none;
    }

    .buttons {
        margin-top: 20px;
    }

    button {
        padding: 15px 30px;
        font-size: 18px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        margin: 10px;
    }

    .smash {
        background: #ef4444;
        color: white;
    }

    .pass {
        background: #3b82f6;
        color: white;
    }

    .result {
        margin-top: 15px;
        font-size: 22px;
        font-weight: bold;
    }
</style>
</head>

<body>

<h1>Smash or Pass</h1>

<div class="card">
    <img id="carImage" src="" alt="Car">
</div>

<div class="buttons">
    <button class="smash" onclick="nextCar('smash')">🔨 Smash</button>
    <button class="pass" onclick="nextCar('pass')">❌ Pass</button>
</div>

<div class="result" id="result"></div>

<script>

const cars = [
  "IMG_5026.jpeg",
  "IMG_5030.jpeg",
  "IMG_5031.jpeg",
  "IMG_5032.jpeg",
];

let index = 0;

const carImage = document.getElementById("carImage");
const result = document.getElementById("result");

// load first image immediately
carImage.src = cars[index];

function nextCar(choice) {

    // show result text
    if (choice === "smash") {
        result.innerHTML = "🔨 SMASH!";
        result.style.color = "#ef4444";
    } else {
        result.innerHTML = "❌ PASS!";
        result.style.color = "#3b82f6";
    }

    // move to next image
    index++;

    if (index >= cars.length) {
        index = 0;
    }

    carImage.src = cars[index];
}

</script>

</body>
</html>