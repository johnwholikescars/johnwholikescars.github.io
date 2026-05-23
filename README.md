<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Smash or Pass</title>

<style>
body {
  margin: 0;
  background: #0f0f0f;
  font-family: Arial;
  color: white;
  overflow: hidden;
}

/* TOP AREA */
#topArea {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
}

#counter {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 8px;
}

/* skip/back row under counter */
#miniButtons {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

#miniButtons button {
  padding: 8px 12px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

#back { background: #888; color: white; }
#skip { background: #4da6ff; color: white; }

/* card */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  position: relative;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* animations */
.fly-right { transform: translateX(500px) rotate(20deg); opacity: 0; }
.fly-left { transform: translateX(-500px) rotate(-20deg); opacity: 0; }
.fly-up { transform: translateY(-500px); opacity: 0; }

/* flash */
.flashScreen {
  position: fixed;
  inset: 0;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.2s ease;
  z-index: 999;
}

.flashShow { opacity: 1; }

#flashText {
  font-size: 54px;
  font-weight: 900;
}

/* bottom buttons */
.bottomBar {
  position: fixed;
  bottom: 20px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 10px;
}

button {
  padding: 12px 14px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 14px;
}

#pass { background: #ff4d4d; color: white; }
#smash { background: #4dff88; color: black; }

/* end screen */
#endScreen {
  position: fixed;
  inset: 0;
  display: none;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,0.9);
  text-align: center;
}
</style>
</head>

<body>

<!-- TOP AREA -->
<div id="topArea">
  <div id="counter">1 / 1</div>

  <div id="miniButtons">
    <button id="back">⬅ Back</button>
    <button id="skip">⏭ Skip</button>
  </div>
</div>

<div id="app">
  <div class="flashScreen" id="flashScreen">
    <div id="flashText"></div>
  </div>

  <div class="card" id="card">
    <img id="img" src="" />
  </div>
</div>

<!-- BOTTOM -->
<div class="bottomBar">
  <button id="pass">Pass 👎</button>
  <button id="smash">Smash 🔥</button>
</div>

<!-- END -->
<div id="endScreen">
  <div>
    <h1>Finished!</h1>
    <p id="stats"></p>
    <button onclick="restart()">Play Again</button>
  </div>
</div>

<script>

// images
let imagesBase = [
"IMG_5076.jpeg","IMG_5075.jpeg","IMG_5074.jpeg","IMG_5073.jpeg",
"IMG_5072.jpeg","IMG_5071.jpeg","IMG_5070.jpeg","IMG_5069.jpeg",
"IMG_5068.jpeg","IMG_5067.jpeg","IMG_5066.jpeg","IMG_5083.jpeg",
"IMG_5098.jpeg","IMG_5097.jpeg","IMG_5115.jpeg","IMG_5116.jpeg",
"IMG_5118.jpeg","IMG_5117.jpeg","IMG_5119.jpeg","IMG_5122.jpeg",
"IMG_5120.jpeg","IMG_5121.jpeg","IMG_5125.jpeg","IMG_5124.jpeg",
"IMG_5123.jpeg","IMG_5128.jpeg","IMG_5127.jpeg","IMG_5126.jpeg",
"IMG_5163.jpeg","IMG_5162.jpeg","IMG_5161.jpeg","IMG_5160.jpeg",
"IMG_5159.jpeg","IMG_5158.jpeg","IMG_5157.jpeg","IMG_5156.jpeg",
"IMG_5155.jpeg","IMG_5154.jpeg","IMG_5153.jpeg","IMG_5152.jpeg",
"IMG_5151.jpeg","IMG_5150.jpeg","IMG_5149.jpeg","IMG_5182.jpeg",
"IMG_5183.jpeg",
"IMG_5184.jpeg",
"IMG_5185.jpeg",
"IMG_5187.jpeg",
"IMG_5188.jpeg",
"IMG_5189.jpeg",
"IMG_5190.jpeg",
"IMG_5191.jpeg",
"IMG_5192.jpeg",
"IMG_5193.jpeg",
"IMG_5194.jpeg",
"IMG_5195.jpeg",
"IMG_5196.jpeg",
"IMG_5197.jpeg",
"IMG_5198.jpeg",
"IMG_5199.jpeg",
"IMG_5200.jpeg",
"IMG_5201.jpeg",
"IMG_5202.jpeg",
"IMG_5203.jpeg",
"IMG_5204.jpeg",
"IMG_5205.jpeg",
"IMG_5206.jpeg",
"IMG_5207.jpeg",
"IMG_5208.jpeg",
"IMG_5209.jpeg",
"IMG_5210.jpeg",
"IMG_5211.jpeg",
"IMG_5212.jpeg",
"IMG_5265.jpeg",
"IMG_5264.jpeg",
"IMG_5263.jpeg",
"IMG_5262.jpeg",
"IMG_5261.jpeg",
"IMG_5260.jpeg",
"IMG_5259.jpeg",
"IMG_5258.jpeg",
"IMG_5257.jpeg",
"IMG_5256.jpeg",
"IMG_5255.jpeg",
"IMG_5254.jpeg",
"IMG_5253.jpeg",
"IMG_5252.jpeg",
"IMG_5251.jpeg",
"IMG_5229.jpeg",
"IMG_5228.jpeg",
"IMG_5227.jpeg",
"IMG_5226.jpeg",
"IMG_5225.jpeg",
"IMG_5224.jpeg",
"IMG_5223.jpeg",
"IMG_5222.jpeg",
"IMG_5221.jpeg",
"IMG_5220.jpeg"
];
let images = [];
let skipped = [];
let history = [];

let index = 0;
let smash = 0;
let pass = 0;

function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

function start() {
  images = shuffle([...imagesBase]);
  index = 0;
  smash = 0;
  pass = 0;
  skipped = [];
  history = [];
  load();
}

function updateCounter() {
  document.getElementById("counter").innerText =
    `${index + 1} / ${images.length}`;
}

function load() {

  if (index >= images.length && skipped.length > 0) {
    images = skipped;
    skipped = [];
    index = 0;
  }

  if (index >= images.length) {
    end();
    return;
  }

  document.getElementById("img").src = images[index];
  updateCounter();
}

function flash(text, color) {
  const f = document.getElementById("flashScreen");
  const t = document.getElementById("flashText");

  t.innerText = text;
  t.style.color = color;

  f.classList.add("flashShow");
  setTimeout(() => f.classList.remove("flashShow"), 500);
}

function swipe(type) {
  const card = document.getElementById("card");

  history.push(index);

  if (type === "smash") {
    smash++;
    card.classList.add("fly-right");
    flash("SMASH 🔥", "#4dff88");
  } else {
    pass++;
    card.classList.add("fly-left");
    flash("PASS 👎", "#ff4d4d");
  }

  setTimeout(() => {
    card.classList.remove("fly-right", "fly-left");
    index++;
    load();
  }, 350);
}

function skip() {
  const card = document.getElementById("card");

  skipped.push(images[index]);
  history.push(index);

  card.classList.add("fly-up");
  flash("SKIP ⏭", "#4da6ff");

  setTimeout(() => {
    card.classList.remove("fly-up");
    index++;
    load();
  }, 350);
}

function back() {
  if (history.length === 0) return;
  index = history.pop();
  load();
}

function end() {

  document.querySelector(".card").style.display = "none";
  document.querySelector(".bottomBar").style.display = "none";
  document.getElementById("topArea").style.display = "none";

  document.getElementById("endScreen").style.display = "flex";

  const total = smash + pass;
  const percent = total ? Math.round((smash / total) * 100) : 0;

  document.getElementById("stats").innerHTML =
    `🔥 Smashes: ${smash}<br>👎 Passes: ${pass}<br>📊 Smash Rate: ${percent}%`;
}

function restart() {
  document.querySelector(".card").style.display = "block";
  document.querySelector(".bottomBar").style.display = "flex";
  document.getElementById("topArea").style.display = "flex";
  document.getElementById("endScreen").style.display = "none";
  start();
}

document.getElementById("smash").onclick = () => swipe("smash");
document.getElementById("pass").onclick = () => swipe("pass");
document.getElementById("skip").onclick = skip;
document.getElementById("back").onclick = back;

start();

</script>

</body>
</html>