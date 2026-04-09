<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>株式会社MBTI</title>
<style>
  body, html { margin:0; padding:0; height:100%; font-family: sans-serif; background:#111; color:#fff; }
  .slide-container { position:relative; width:100%; max-width:360px; margin:0 auto; height:100vh; overflow:hidden; }
  .bg-gradient { position:absolute; inset:0; transition: all 0.5s; }
  .content { position:relative; z-index:1; display:flex; flex-direction:column; justify-content:space-between; height:100%; padding:10px; background: rgba(0,0,0,0.3); }
  .title { text-align:center; margin-bottom:10px; }
  .title h1 { font-size:24px; font-weight:bold; background: linear-gradient(to right, #ff7, #f0f); -webkit-background-clip: text; color: transparent; }
  .title p { font-size:12px; color:#ccc; }
  .slide { display:flex; flex-direction:column; align-items:center; gap:10px; }
  .card { width:100%; max-width:200px; padding:10px; border-radius:10px; text-align:center; margin-bottom:5px; color:#fff; transition: transform 0.3s; }
  .card img { width:50px; height:60px; object-fit:cover; border-radius:5px; margin-bottom:5px; }
  .vs { font-size:20px; margin:5px; }
  .comments { width:100%; max-width:220px; overflow-y:auto; max-height:120px; }
  .comment { display:flex; gap:5px; margin-bottom:3px; }
  .comment div { width:24px; height:24px; border-radius:50%; overflow:hidden; }
  .comment div img { width:100%; height:100%; object-fit:cover; }
  .nav { display:flex; justify-content:space-between; margin-top:10px; }
  .nav button { padding:5px; border:none; border-radius:50%; background:#444; color:#fff; font-size:16px; }
</style>
</head>
<body>

<div class="slide-container" id="slideContainer">
  <div class="bg-gradient" id="bgGradient"></div>
  <div class="content">
    <div class="title">
      <h1>株式会社MBTI</h1>
      <p id="slideCount"></p>
    </div>
    <div class="slide">
      <div class="card" id="mainCard"></div>
      <div class="vs">VS</div>
      <div class="card" id="vsCard"></div>
    </div>
    <div class="comments" id="comments"></div>
    <div class="nav">
      <button id="prevBtn">↑</button>
      <button id="nextBtn">↓</button>
    </div>
  </div>
</div>

<script>
const slides = [
  { title:"朝礼の時間", mainChar:"INTJ", vsChar:"ISFP", emoji:"📋", mainAction:"タスクリスト表示", vsAction:"映画の話をしてる", comments:[{type:"ENTP",text:"朝礼で映画？笑"},{type:"ESTJ",text:"リストすごい…"}]},
  { title:"プロジェクト提案", mainChar:"ESTJ", vsChar:"INFP", emoji:"📊", mainAction:"計画表を3つ準備", vsAction:"大切なことを語る", comments:[{type:"ISTP",text:"計画、完璧だ"},{type:"ENFJ",text:"気持ちもわかるけど…"}]}
  // ここに残りのスライドも追加
];

const mbtiColors = { INTJ:"#FF6B6B", ENTJ:"#FF5252", INTP:"#4ECDC4", ENTP:"#45B7D1", INFJ:"#95E1D3", ENFJ:"#A8E6CF", INFP:"#FFD3B6", ENFP:"#FFAAA5", ISTJ:"#8B4789", ESTJ:"#7B3FF2", ISFJ:"#FF69B4", ESFJ:"#FF1493", ISTP:"#20B2AA", ESTP:"#00CED1", ISFP:"#FFB347", ESFP:"#FFA500"};

// 画像パス（ここもアップロード済み画像に変更）
const mbtiImages = { INTJ:"/mnt/user-data/uploads/IMG_4095.jpeg", ISFP:"/mnt/user-data/uploads/IMG_4122.jpeg", ENTP:"/mnt/user-data/uploads/IMG_4098.jpeg", ESTJ:"/mnt/user-data/uploads/IMG_4116.jpeg", ISTP:"/mnt/user-data/uploads/IMG_4113.jpeg", ENFJ:"/mnt/user-data/uploads/IMG_4115.jpeg", INFP:"/mnt/user-data/uploads/IMG_4096.jpeg" };

let currentSlide = 0;

function renderSlide(){
  const slide = slides[currentSlide];
  document.getElementById('slideCount').innerText = (currentSlide+1)+" / "+slides.length;
  document.getElementById('mainCard').innerHTML = `<div>${slide.emoji} ${slide.mainChar}</div><p>${slide.mainAction}</p>`;
  document.getElementById('vsCard').innerHTML = `<div>${slide.vsChar}</div><p>${slide.vsAction}</p>`;
  // 背景
  document.getElementById('bgGradient').style.background = `linear-gradient(135deg, ${mbtiColors[slide.mainChar]}30 0%, ${mbtiColors[slide.vsChar]}30 100%)`;
  // コメント
  const commentDiv = document.getElementById('comments');
  commentDiv.innerHTML = '';
  slide.comments.forEach(c => {
    const div = document.createElement('div');
    div.className='comment';
    div.innerHTML = `<div style="background:${mbtiColors[c.type]}"><img src="${mbtiImages[c.type]}" onerror="this.style.display='none'"></div><div>${c.type}: ${c.text}</div>`;
    commentDiv.appendChild(div);
  });
}

function nextSlide(){ currentSlide=(currentSlide+1)%slides.length; renderSlide(); }
function prevSlide(){ currentSlide=(currentSlide-1+slides.length)%slides.length; renderSlide(); }

document.getElementById('nextBtn').addEventListener('click',nextSlide);
document.getElementById('prevBtn').addEventListener('click',prevSlide);

// スワイプ対応
let touchStartY = 0;
const container = document.getElementById('slideContainer');
container.addEventListener('touchstart', e => { touchStartY = e.touches[0].clientY; });
container.addEventListener('touchend', e => {
  const touchEndY = e.changedTouches[0].clientY;
  if(touchStartY - touchEndY > 50) nextSlide();
  else if(touchEndY - touchStartY > 50) prevSlide();
});

renderSlide();
</script>

</body>
</html>
