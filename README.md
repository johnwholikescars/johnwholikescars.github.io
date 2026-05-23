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

/* skip/back row */
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

<div class="bottomBar">
  <button id="pass">Pass 👎</button>
  <button id="smash">Smash 🔥</button>
</div>

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
  "IMG_5068.jpeg","IMG_5067.jpeg","IMG_5066.jpeg",
  "IMG_5083.jpeg","IMG_5098.jpeg","IMG_5097.jpeg",
  "IMG_5115.jpeg","IMG_5116.jpeg","IMG_5118.jpeg",
  "IMG_5117.jpeg","IMG_5119.jpeg","IMG_5122.jpeg",
  "IMG_5120.jpeg","IMG_5121.jpeg","IMG_5125.jpeg",
  "IMG_5124.jpeg","IMG_5123.jpeg","IMG_5128.jpeg",
  "IMG_5127.jpeg","IMG_5126.jpeg",

  "IMG_5163.jpeg","IMG_5162.jpeg","IMG_5161.jpeg",
  "IMG_5160.jpeg","IMG_5159.jpeg","IMG_5158.jpeg",
  "IMG_5157.jpeg","IMG_5156.jpeg","IMG_5155.jpeg",
  "IMG_5154.jpeg","IMG_5153.jpeg","IMG_5152.jpeg",
  "IMG_5151.jpeg","IMG_5150.jpeg","IMG_5149.jpeg",

  "IMG_5182.jpeg","IMG_5183.jpeg","IMG_5184.jpeg",
  "IMG_5185.jpeg","IMG_5187.jpeg","IMG_5188.jpeg",
  "IMG_5189.jpeg","IMG_5190.jpeg","IMG_5191.jpeg",
  "IMG_5192.jpeg","IMG_5193.jpeg","IMG_5194.jpeg",
  "IMG_5195.jpeg","IMG_5196.jpeg","IMG_5197.jpeg",
  "IMG_5198.jpeg","IMG_5199.jpeg","IMG_5200.jpeg",
  "IMG_5201.jpeg","IMG_5202.jpeg","IMG_5203.jpeg",
  "IMG_5204.jpeg","IMG_5205.jpeg","IMG_5206.jpeg",
  "IMG_5207.jpeg","IMG_5208.jpeg","IMG_5209.jpeg",
  "IMG_5210.jpeg","IMG_5211.jpeg","IMG_5212.jpeg",

  "IMG_5265.jpeg","IMG_5264.jpeg","IMG_5263.jpeg",
  "IMG_5262.jpeg","IMG_5261.jpeg","IMG_5260.jpeg",
  "IMG_5259.jpeg","IMG_5258.jpeg","IMG_5257.jpeg",
  "IMG_5256.jpeg","IMG_5255.jpeg","IMG_5254.jpeg",
  "IMG_5253.jpeg","IMG_5252.jpeg","IMG_5251.jpeg",

  "IMG_5229.jpeg","IMG_5228.jpeg","IMG_5227.jpeg",
  "IMG_5226.jpeg","IMG_5225.jpeg","IMG_5224.jpeg",
  "IMG_5223.jpeg","IMG_5222.jpeg","IMG_5221.jpeg",
  "IMG_5220.jpeg",

  "IMG_5325.jpeg","IMG_5324.jpeg","IMG_5323.jpeg",
  "IMG_5322.jpeg","IMG_5321.jpeg","IMG_5320.jpeg",
  "IMG_5319.jpeg","IMG_5318.jpeg","IMG_5317.jpeg",
  "IMG_5316.jpeg","IMG_5315.jpeg","IMG_5314.jpeg",
  "IMG_5313.jpeg","IMG_5312.jpeg","IMG_5311.jpeg",
  "IMG_5310.jpeg","IMG_5309.jpeg","IMG_5308.jpeg",
  "IMG_5307.jpeg","IMG_5306.jpeg","IMG_5305.jpeg",
  "IMG_5304.jpeg","IMG_5303.jpeg","IMG_5302.jpeg",
  "IMG_5301.jpeg","IMG_5300.jpeg","IMG_5299.jpeg",
  "IMG_5298.jpeg","IMG_5297.jpeg","IMG_5296.jpeg",
  "IMG_5295.jpeg","IMG_5294.jpeg","IMG_5293.jpeg",
  "IMG_5292.jpeg","IMG_5291.jpeg","IMG_5290.jpeg",
  "IMG_5289.jpeg","IMG_5288.jpeg","IMG_5287.jpeg",
  "IMG_5286.jpeg","IMG_5285.jpeg","IMG_5284.jpeg",
  "IMG_5283.jpeg","IMG_5282.jpeg","IMG_5281.jpeg",
  "IMG_5280.jpeg","IMG_5279.jpeg","IMG_5278.jpeg",
  "IMG_5277.jpeg","IMG_5276.jpeg","IMG_5275.jpeg",
  "IMG_5274.jpeg","IMG_5273.jpeg","IMG_5272.jpeg",
  "IMG_5271.jpeg","IMG_5270.jpeg","IMG_5269.jpeg",
  "IMG_5268.jpeg",
  "IMG_5371.jpeg",
"IMG_5370.jpeg",
"IMG_5369.jpeg",
"IMG_5368.jpeg",
"IMG_5367.jpeg",
"IMG_5366.jpeg",
"IMG_5365.jpeg",
"IMG_5364.jpeg",
"IMG_5363.jpeg",
"IMG_5362.jpeg",
"IMG_5361.jpeg",
"IMG_5359.jpeg",
"IMG_5358.jpeg",
"IMG_5357.jpeg",
"IMG_5356.jpeg",
"IMG_5355.jpeg",
"IMG_5354.jpeg",
"IMG_5353.jpeg",
"IMG_5352.jpeg",
"IMG_5351.jpeg",
"IMG_5350.jpeg",
"IMG_5349.jpeg",
"IMG_5348.jpeg",
"IMG_5347.jpeg",
"IMG_5346.jpeg",
"IMG_5345.jpeg",
"IMG_5344.jpeg",
"IMG_5343.jpeg",
"IMG_5342.jpeg",
"IMG_5341.jpeg",
"IMG_5339.jpeg",
"IMG_5338.jpeg",
"IMG_5337.jpeg",
"IMG_5336.jpeg",
"IMG_5335.jpeg",
"IMG_5334.jpeg",
"IMG_5333.jpeg",
"IMG_5332.jpeg",
"IMG_5331.jpeg",
"IMG_5330.jpeg",
"IMG_5518.jpeg",
"IMG_5517.jpeg",
"IMG_5516.jpeg",
"IMG_5515.jpeg",
"IMG_5514.jpeg",
"IMG_5513.jpeg",
"IMG_5512.jpeg",
"IMG_5511.jpeg",
"IMG_5510.jpeg",
"IMG_5509.jpeg",
"IMG_5508.jpeg",
"IMG_5507.jpeg",
"IMG_5506.jpeg",
"IMG_5505.jpeg",
"IMG_5504.jpeg",
"IMG_5503.jpeg",
"IMG_5502.jpeg",
"IMG_5501.jpeg",
"IMG_5500.jpeg",
"IMG_5498.jpeg",
"IMG_5497.jpeg",
"IMG_5496.jpeg",
"IMG_5495.jpeg",
"IMG_5493.jpeg",
"IMG_5492.jpeg",
"IMG_5491.jpeg",
"IMG_5490.jpeg",
"IMG_5489.jpeg",
"IMG_5488.jpeg",
"IMG_5487.jpeg",
"IMG_5486.jpeg",
"IMG_5485.jpeg",
"IMG_5484.jpeg",
"IMG_5483.jpeg",
"IMG_5482.jpeg",
"IMG_5481.jpeg",
"IMG_5480.jpeg",
"IMG_5479.jpeg",
"IMG_5478.jpeg",
"IMG_5477.jpeg",
"IMG_5476.jpeg",
"IMG_5475.jpeg",
"IMG_5474.jpeg",
"IMG_5473.jpeg",
"IMG_5472.jpeg",
"IMG_5471.jpeg",
"IMG_5470.jpeg",
"IMG_5469.jpeg",
"IMG_5468.jpeg",
"IMG_5467.jpeg",
"IMG_5466.jpeg",
"IMG_5465.jpeg",
"IMG_5464.jpeg",
"IMG_5463.jpeg",
"IMG_5462.jpeg",
"IMG_5461.jpeg",
"IMG_5460.jpeg",
"IMG_5459.jpeg",
"IMG_5458.jpeg",
"IMG_5457.jpeg",
"IMG_5456.jpeg",
"IMG_5455.jpeg",
"IMG_5454.jpeg",
"IMG_5453.jpeg",
"IMG_5452.jpeg",
"IMG_5451.jpeg",
"IMG_5450.jpeg",
"IMG_5449.jpeg",
"IMG_5448.jpeg",
"IMG_5447.jpeg",
"IMG_5446.jpeg",
"IMG_5445.jpeg",
"IMG_5444.jpeg",
"IMG_5443.jpeg",
"IMG_5442.jpeg",
"IMG_5441.jpeg",
"IMG_5440.jpeg",
"IMG_5439.jpeg",
"IMG_5438.jpeg",
"IMG_5437.jpeg",
"IMG_5435.jpeg",
"IMG_5434.jpeg",
"IMG_5433.jpeg",
"IMG_5432.jpeg",
"IMG_5431.jpeg",
"IMG_5430.jpeg",
"IMG_5428.jpeg",
"IMG_5426.jpeg",
"IMG_5425.jpeg",
"IMG_5424.jpeg",
"IMG_5423.jpeg",
"IMG_5422.jpeg",
"IMG_5421.jpeg",
"IMG_5420.jpeg",
"IMG_5419.jpeg",
"IMG_5417.jpeg",
"IMG_5416.jpeg",
"IMG_5415.jpeg",
"IMG_5414.jpeg",
"IMG_5413.jpeg",
"IMG_5412.jpeg",
"IMG_5411.jpeg",
"IMG_5410.jpeg",
"IMG_5409.jpeg",
"IMG_5408.jpeg",
"IMG_5407.jpeg",
"IMG_5406.jpeg",
"IMG_5405.jpeg",
"IMG_5404.jpeg",
"IMG_5403.jpeg",
"IMG_5402.jpeg",
"IMG_5401.jpeg",
"IMG_5400.jpeg",
"IMG_5399.jpeg",
"IMG_5398.jpeg",
"IMG_5397.jpeg",
"IMG_5396.jpeg",
"IMG_5395.jpeg",
"IMG_5394.jpeg",
"IMG_5393.jpeg",
"IMG_5392.jpeg",
"IMG_5391.jpeg",
"IMG_5390.jpeg",
"IMG_5389.jpeg",
"IMG_5388.jpeg",
"IMG_5387.jpeg",
"IMG_5386.jpeg",
"IMG_5385.jpeg",
"IMG_5384.jpeg",
"IMG_5383.jpeg",
"IMG_5382.jpeg",
"IMG_5381.jpeg",
"IMG_5380.jpeg",
"IMG_5379.jpeg",
"IMG_5378.jpeg",
"IMG_5377.jpeg",
"IMG_5376.jpeg"
];

let images = [];
let skipped = [];
let history = [];

let index = 0;
let smash = 0;
let pass = 0;

// shuffle
function shuffle(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

// start
function start() {
  images = shuffle([...imagesBase]);
  index = 0;
  smash = 0;
  pass = 0;
  skipped = [];
  history = [];
  load();
}

// counter
function updateCounter() {
  document.getElementById("counter").innerText =
    `${index + 1} / ${images.length}`;
}

// load image
function load() {

  if (index >= images.length && skipped.length > 0) {
    images = [...skipped];
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

// flash
function flash(text, color) {

  const f = document.getElementById("flashScreen");
  const t = document.getElementById("flashText");

  t.innerText = text;
  t.style.color = color;

  f.classList.add("flashShow");

  setTimeout(() => {
    f.classList.remove("flashShow");
  }, 500);
}

// swipe
function swipe(type) {

  const card = document.getElementById("card");

  history.push({
    index: index,
    action: type
  });

  if (type === "smash") {
    smash++;
    card.classList.add("fly-right");
    flash("SMASH 🔥", "#4dff88");
  }

  if (type === "pass") {
    pass++;
    card.classList.add("fly-left");
    flash("PASS 👎", "#ff4d4d");
  }

  setTimeout(() => {

    card.classList.remove("fly-right");
    card.classList.remove("fly-left");

    index++;
    load();

  }, 350);
}

// skip
function skip() {

  const card = document.getElementById("card");

  skipped.push(images[index]);

  history.push({
    index: index,
    action: "skip"
  });

  card.classList.add("fly-up");

  flash("SKIP ⏭", "#4da6ff");

  setTimeout(() => {

    card.classList.remove("fly-up");

    index++;
    load();

  }, 350);
}

// back FIXED
function back() {

  if (history.length === 0) return;

  const previous = history.pop();

  if (previous.action === "smash") {
    smash--;
  }

  if (previous.action === "pass") {
    pass--;
  }

  if (previous.action === "skip") {
    skipped.pop();
  }

  index = previous.index;

  load();
}

// end
function end() {

  document.querySelector(".card").style.display = "none";
  document.querySelector(".bottomBar").style.display = "none";
  document.getElementById("topArea").style.display = "none";

  document.getElementById("endScreen").style.display = "flex";

  const total = smash + pass;
  const percent = total
    ? Math.round((smash / total) * 100)
    : 0;

  document.getElementById("stats").innerHTML =
    `🔥 Smashes: ${smash}<br>
     👎 Passes: ${pass}<br>
     📊 Smash Rate: ${percent}%`;
}

// restart
function restart() {

  document.querySelector(".card").style.display = "block";
  document.querySelector(".bottomBar").style.display = "flex";
  document.getElementById("topArea").style.display = "flex";
  document.getElementById("endScreen").style.display = "none";

  start();
}

// buttons
document.getElementById("smash").onclick = () => swipe("smash");
document.getElementById("pass").onclick = () => swipe("pass");
document.getElementById("skip").onclick = skip;
document.getElementById("back").onclick = back;

// start
start();

</script>

</body>
</html>