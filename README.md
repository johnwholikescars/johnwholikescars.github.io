<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Tier Picker</title>

<style>
body {
  margin: 0;
  background: #0f0f0f;
  font-family: Arial;
  color: white;
  overflow: hidden;
}

/* TOP ROW */
#topRow {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 10px;
}

#topRow button {
  padding: 8px 12px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 13px;
}

#back { background: #777; color: white; }
#skip { background: #4da6ff; color: white; }
#bookmarkBtn { background: #ffcc00; color: black; }

/* COUNTER */
#counter {
  text-align: center;
  margin-top: 8px;
  font-size: 18px;
  font-weight: bold;
}

/* APP */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* CARD */
.card {
  width: 320px;
  height: 420px;
  border-radius: 20px;
  overflow: hidden;
  background: #222;
  margin-top: 15px;
  position: relative;
}

.card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* FLASH */
.flashScreen {
  position: fixed;
  inset: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  transition: 0.2s;
  background: black;
  z-index: 999;
}

.flashShow { opacity: 1; }

#flashText {
  font-size: 50px;
  font-weight: 900;
}

/* TIER SCREEN (NEW) */
#tierScreen {
  position: fixed;
  inset: 0;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 60px;
  font-weight: 900;
  opacity: 0;
  pointer-events: none;
  transition: 0.2s;
  z-index: 1000;
}

/* BOTTOM TIERS */
#bottomRow {
  position: fixed;
  bottom: 15px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  flex-wrap: wrap;
}

#bottomRow button {
  padding: 10px 12px;
  border: none;
  border-radius: 10px;
  font-size: 13px;
  cursor: pointer;
}

/* tier colors */
#sPlus { background: gold; color: black; }
#s { background: #ff4d4d; color: white; }
#a { background: #4dff88; color: black; }
#b { background: #4da6ff; color: white; }
#c { background: #888; color: white; }

/* END SCREEN */
#endScreen {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.9);
  display: none;
  justify-content: center;
  align-items: center;
  text-align: center;
}

#endBox button {
  margin: 5px;
  padding: 10px;
}
</style>
</head>

<body>

<div id="topRow">
  <button id="back">⬅</button>
  <button id="skip">⏭</button>
  <button id="bookmarkBtn">🔖</button>
</div>

<div id="counter">1 / 1</div>

<div id="app">

  <div class="flashScreen" id="flashScreen">
    <div id="flashText"></div>
  </div>

  <div id="tierScreen"></div>

  <div class="card">
    <img id="img" src="" />
  </div>

</div>

<div id="bottomRow">
  <button id="sPlus">S+ (<span id="sPlusLeft">5</span>)</button>
  <button id="s">S</button>
  <button id="a">A</button>
  <button id="b">B</button>
  <button id="c">C</button>
</div>

<div id="endScreen">
  <div id="endBox">
    <h1>Finished</h1>
    <p id="stats"></p>
    <button onclick="showBookmarks()">Bookmarks</button>
    <button onclick="showSPlus()">S+ Photos</button>
    <button onclick="restart()">Restart</button>
  </div>
</div>

<script>

/* IMAGES */
let imagesBase = [
"IMG_5076.jpeg","IMG_5075.jpeg","IMG_5074.jpeg","IMG_5073.jpeg",

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
let index = 0;
let history = [];

let sPlus = [];
let sList = [];
let aList = [];
let bList = [];
let cList = [];

let bookmarks = [];
let skipped = [];

let sPlusUses = 0;
const sPlusLimit = 5;

/* shuffle */
function shuffle(arr){
  for(let i=arr.length-1;i>0;i--){
    let j=Math.floor(Math.random()*(i+1));
    [arr[i],arr[j]]=[arr[j],arr[i]];
  }
  return arr;
}

/* start */
function start(){
  images = shuffle([...imagesBase]);
  index = 0;

  sPlus = [];
  sList = [];
  aList = [];
  bList = [];
  cList = [];

  bookmarks = [];
  skipped = [];
  history = [];

  updateSPlusUI();
  load();
}

/* counter */
function updateCounter(){
  document.getElementById("counter").innerText =
    `${index+1} / ${images.length}`;
}

/* S+ UI */
function updateSPlusUI(){
  document.getElementById("sPlusLeft").innerText =
    Math.max(0, sPlusLimit - sPlusUses);
}

/* tier screen */
function showTierScreen(text){
  const screen = document.getElementById("tierScreen");
  screen.innerText = text;
  screen.style.opacity = 1;

  setTimeout(()=>{
    screen.style.opacity = 0;
  }, 350);
}

/* load */
function load(){

  if(index >= images.length && skipped.length){
    images = [...skipped];
    skipped = [];
    index = 0;
  }

  if(index >= images.length){
    end();
    return;
  }

  document.getElementById("img").src = images[index];
  updateCounter();
}

/* tier select */
function pickTier(tier){

  let img = images[index];
  history.push({index,tier});

  if(tier === "sPlus"){
    if(sPlusUses >= sPlusLimit){
      showTierScreen("LOCKED");
      return;
    }
    sPlusUses++;
    sPlus.push(img);
    showTierScreen("S+");
    updateSPlusUI();
  }

  if(tier === "s"){ sList.push(img); showTierScreen("S"); }
  if(tier === "a"){ aList.push(img); showTierScreen("A"); }
  if(tier === "b"){ bList.push(img); showTierScreen("B"); }
  if(tier === "c"){ cList.push(img); showTierScreen("C"); }

  setTimeout(()=>{
    index++;
    load();
  }, 350);
}

/* skip */
function skip(){
  skipped.push(images[index]);
  showTierScreen("SKIP");
  index++;
  load();
}

/* back */
function back(){
  if(!history.length) return;
  let last = history.pop();
  index = last.index;
  load();
}

/* bookmark */
function toggleBookmark(){
  let img = images[index];

  if(bookmarks.includes(img)){
    bookmarks = bookmarks.filter(i=>i!==img);
    showTierScreen("UNBOOK");
  } else {
    bookmarks.push(img);
    showTierScreen("BOOKED");
  }
}

/* end */
function end(){
  document.querySelector(".card").style.display="none";
  document.getElementById("bottomRow").style.display="none";
  document.getElementById("topRow").style.display="none";
  document.getElementById("counter").style.display="none";
  document.getElementById("endScreen").style.display="flex";

  document.getElementById("stats").innerHTML =
  `
  S+: ${sPlus.length}<br>
  S: ${sList.length}<br>
  A: ${aList.length}<br>
  B: ${bList.length}<br>
  C: ${cList.length}
  `;
}

/* views */
function showBookmarks(){
  alert(bookmarks.join("\n") || "No bookmarks");
}

function showSPlus(){
  alert(sPlus.join("\n") || "No S+ photos");
}

/* restart */
function restart(){
  location.reload();
}

/* buttons */
document.getElementById("sPlus").onclick=()=>pickTier("sPlus");
document.getElementById("s").onclick=()=>pickTier("s");
document.getElementById("a").onclick=()=>pickTier("a");
document.getElementById("b").onclick=()=>pickTier("b");
document.getElementById("c").onclick=()=>pickTier("c");

document.getElementById("skip").onclick=skip;
document.getElementById("back").onclick=back;
document.getElementById("bookmarkBtn").onclick=toggleBookmark;

start();

</script>

</body>
</html>