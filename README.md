
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
<title>过桥米线 · 词语闯关</title>
<style>
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  html,body{margin:0;padding:0}
  body{
    min-height:100vh;
    font-family:"PingFang SC","Hiragino Sans GB","Microsoft YaHei",system-ui,sans-serif;
    color:#3b2a20;
    background:
      radial-gradient(circle at 8% 0%,#ffe7c7 0%,transparent 45%),
      radial-gradient(circle at 95% 8%,#ffd9d0 0%,transparent 40%),
      #fff6ea;
    padding:18px 14px 40px;
    display:flex;justify-content:center;
  }
  .wrap{width:100%;max-width:520px}
  .screen{display:none}
  .screen.active{display:block;animation:fade .32s ease}
  @keyframes fade{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:none}}

  /* ---------- 开始页 ---------- */
  .hero{text-align:center;padding-top:10px}
  .hero-art svg{width:190px;height:190px;filter:drop-shadow(0 8px 14px rgba(234,88,12,.18))}
  h1{font-size:27px;margin:6px 0 4px;color:#c2410c;letter-spacing:1px}
  .sub{margin:0 0 10px;color:#9a3412;font-size:15px;font-weight:600}
  .tips{font-size:13.5px;color:#a8865f;line-height:1.7;margin:0 0 18px}
  .name-input{
    width:100%;padding:15px 18px;border-radius:16px;border:3px solid #fcd34d;
    background:#fff;font-size:17px;color:#3b2a20;outline:none;margin-bottom:14px;
    text-align:center;font-weight:600;
  }
  .name-input:focus{border-color:#fb923c}
  .name-input::placeholder{color:#c8b39a;font-weight:400}

  .btn-primary{
    width:100%;padding:17px;border:none;border-radius:18px;
    background:linear-gradient(180deg,#fb923c,#f97316);
    color:#fff;font-size:20px;font-weight:800;letter-spacing:2px;
    box-shadow:0 6px 0 #c2410c,0 12px 22px rgba(249,115,22,.28);
    cursor:pointer;transition:.12s;margin-bottom:14px;
  }
  .btn-primary:active{transform:translateY(4px);box-shadow:0 2px 0 #c2410c}
  .btn-ghost{
    width:100%;padding:13px;border-radius:16px;border:2.5px dashed #fdba74;
    background:#fffdf8;color:#c2410c;font-size:16px;font-weight:700;
    cursor:pointer;margin-bottom:10px;transition:.15s;
  }
  .btn-ghost:active{background:#fff0e0}

  /* ---------- 答题页 ---------- */
  .hud{display:flex;justify-content:space-between;font-weight:800;color:#9a3412;font-size:15px;margin-bottom:8px}
  #timerText{font-variant-numeric:tabular-nums}
  .progress-bar{height:12px;background:#ffe4c4;border-radius:99px;overflow:hidden;margin-bottom:18px}
  .progress-bar>div{height:100%;background:linear-gradient(90deg,#fbbf24,#f97316);border-radius:99px;transition:width .35s ease}

  .art-card{
    background:#fff;border-radius:26px;padding:10px;display:flex;justify-content:center;
    box-shadow:inset 0 0 0 3px #ffe4b5,0 8px 20px rgba(234,88,12,.1);margin-bottom:16px;
  }
  .art-card svg{width:200px;height:200px;max-width:100%}

  .sentence-card{
    background:#fff;border-radius:22px;padding:26px 20px;font-size:21px;line-height:1.9;
    font-weight:600;border:3px dashed #fdba74;margin-bottom:16px;
    box-shadow:0 8px 20px rgba(234,88,12,.08);
  }
  .blank{color:#f97316;font-weight:900}

  .q-prompt{text-align:center;font-size:16px;font-weight:700;color:#9a3412;margin:0 0 14px}

  .options{display:flex;flex-direction:column;gap:12px}
  .option{
    width:100%;padding:17px;border-radius:18px;border:3px solid #fcd34d;background:#fff;
    font-size:22px;font-weight:800;color:#3b2a20;cursor:pointer;
    box-shadow:0 4px 0 #fde68a;transition:.13s;font-family:inherit;
  }
  .option:active{transform:translateY(3px);box-shadow:0 1px 0 #fde68a}
  .option.correct{background:#dcfce7;border-color:#22c55e;color:#15803d;box-shadow:0 4px 0 #86efac}
  .option.wrong{background:#fee2e2;border-color:#ef4444;color:#b91c1c;box-shadow:0 4px 0 #fca5a5}
  .option.disabled{pointer-events:none}

  /* ---------- 结果页 ---------- */
  .result-card{background:#fff;border-radius:28px;padding:26px 20px 22px;text-align:center;box-shadow:0 12px 30px rgba(234,88,12,.13)}
  .medal{font-size:56px;line-height:1}
  .result-card h2{margin:8px 0 20px;color:#c2410c;font-size:23px}
  .stats{display:flex;gap:10px;margin-bottom:16px}
  .stat{flex:1;background:#fff7ed;border-radius:16px;padding:14px 6px;border:2px solid #ffe4c4}
  .stat-num{display:block;font-size:22px;font-weight:900;color:#ea580c;font-variant-numeric:tabular-nums}
  .stat-label{display:block;font-size:12.5px;color:#b58a5e;margin-top:4px;font-weight:600}
  .score-box{
    display:flex;align-items:center;justify-content:center;gap:12px;
    background:linear-gradient(180deg,#fff3e0,#ffe7cc);border-radius:18px;padding:14px;margin-bottom:22px;
  }
  .score-box span{font-size:16px;font-weight:700;color:#9a3412}
  .score-box strong{font-size:38px;color:#ea580c;font-weight:900;line-height:1}

  /* ---------- 排行榜 ---------- */
  .board-title{text-align:center;color:#c2410c;font-size:23px;margin:10px 0 20px}
  .board-list{margin-bottom:16px}
  .row{
    display:flex;align-items:center;gap:12px;background:#fff;border-radius:16px;
    padding:12px 14px;margin-bottom:10px;box-shadow:0 4px 12px rgba(234,88,12,.08);
  }
  .row.me{background:#fff7ed;box-shadow:inset 0 0 0 2.5px #fb923c}
  .rank{width:34px;text-align:center;font-size:18px;font-weight:900;color:#b58a5e;flex:none}
  .rmain{flex:1;min-width:0}
  .rname{font-weight:800;font-size:16px;color:#3b2a20;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .rdetail{font-size:12.5px;color:#a8865f;margin-top:3px}
  .rscore{font-size:20px;font-weight:900;color:#ea580c;flex:none}
  .empty{text-align:center;color:#b58a5e;padding:30px 0;font-size:15px}

  /* ---------- 提示条 ---------- */
  .toast{
    position:fixed;left:50%;bottom:34px;transform:translate(-50%,20px);
    background:rgba(59,42,32,.94);color:#fff;padding:13px 24px;border-radius:99px;
    font-size:16px;font-weight:700;opacity:0;pointer-events:none;transition:.25s;
    z-index:99;max-width:88vw;text-align:center;
  }
  .toast.show{opacity:1;transform:translate(-50%,0)}
  .toast.ok{background:#16a34a}
  .toast.no{background:#dc2626}
</style>
</head>
<body>
<div class="wrap">

  <!-- ========== 开始页 ========== -->
  <section id="screen-start" class="screen active">
    <div class="hero">
      <div class="hero-art" id="heroArt"></div>
      <h1>过桥米线 · 词语闯关</h1>
      <p class="sub">看图选词语 · 读句子选词语</p>
      <p class="tips">共 <b>9</b> 关　答对 +100 分，答错 −50 分<br>用时越短，分数越高哦！</p>
      <input id="nameInput" class="name-input" maxlength="8" placeholder="输入你的名字（用于排行榜）">
      <button class="btn-primary" id="startBtn">开 始 闯 关</button>
      <button class="btn-ghost" id="toBoardBtn">🏆 查看排行榜</button>
    </div>
  </section>

  <!-- ========== 答题页 ========== -->
  <section id="screen-quiz" class="screen">
    <div class="hud">
      <span id="progressText">第 1 / 9 关</span>
      <span id="timerText">00:00</span>
    </div>
    <div class="progress-bar"><div id="progressFill" style="width:0%"></div></div>
    <div id="questionArea"></div>
  </section>

  <!-- ========== 结果页 ========== -->
  <section id="screen-result" class="screen">
    <div class="result-card">
      <div class="medal" id="medal">🎉</div>
      <h2 id="resultTitle">闯关完成！</h2>
      <div class="stats">
        <div class="stat"><span class="stat-num" id="statTime">00:00</span><span class="stat-label">用时</span></div>
        <div class="stat"><span class="stat-num" id="statCorrect">0</span><span class="stat-label">答对</span></div>
        <div class="stat"><span class="stat-num" id="statWrong">0</span><span class="stat-label">答错</span></div>
      </div>
      <div class="score-box"><span>得分</span><strong id="statScore">0</strong></div>
      <button class="btn-primary" id="againBtn">再 玩 一 次</button>
      <button class="btn-ghost" id="toBoardBtn2">🏆 查看排行榜</button>
    </div>
  </section>

  <!-- ========== 排行榜 ========== -->
  <section id="screen-board" class="screen">
    <h2 class="board-title">🏆 闯关排行榜</h2>
    <div id="boardList" class="board-list"></div>
    <button class="btn-ghost" id="backBtn">← 返回</button>
    <button class="btn-ghost" id="clearBtn">🗑 清空本机排行榜</button>
  </section>

</div>
<div id="toast" class="toast"></div>

<script>
/* =========================================================
   ① 可选：成绩提交地址（留空则只保存在本机浏览器）
   填上后，每位同学的成绩会自动 POST 一份 JSON 到该地址
   例如：const SUBMIT_URL = "https://你的接口地址";
   ========================================================= */
const SUBMIT_URL = "";

/* =========================================================
   ② 卡通图形素材（纯 SVG，可自行替换成 <img> 图片）
   ========================================================= */
const ART = {
  /* 米线 */
  bowl: `
  <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
    <path d="M78 62 q9 -11 0 -22" stroke="#f0c9a0" stroke-width="6" fill="none" stroke-linecap="round"/>
    <path d="M100 55 q9 -11 0 -22" stroke="#f0c9a0" stroke-width="6" fill="none" stroke-linecap="round"/>
    <path d="M122 62 q9 -11 0 -22" stroke="#f0c9a0" stroke-width="6" fill="none" stroke-linecap="round"/>
    <path d="M28 95 Q30 168 100 168 Q170 168 172 95 Z" fill="#ffffff" stroke="#e7cfa5" stroke-width="4"/>
    <ellipse cx="100" cy="95" rx="66" ry="19" fill="#f6b93b"/>
    <path d="M46 96 q14 -10 28 0 t28 0 t28 0 t28 0" stroke="#fffdf7" stroke-width="6" fill="none" stroke-linecap="round"/>
    <path d="M52 105 q14 -10 28 0 t28 0 t28 0" stroke="#fffdf7" stroke-width="6" fill="none" stroke-linecap="round"/>
    <ellipse cx="100" cy="95" rx="72" ry="22" fill="none" stroke="#e7cfa5" stroke-width="5"/>
    <path d="M46 128 q54 24 108 0" stroke="#ef4444" stroke-width="6" fill="none" stroke-linecap="round" opacity=".85"/>
    <path d="M62 148 q38 14 76 0" stroke="#fbbf24" stroke-width="6" fill="none" stroke-linecap="round" opacity=".9"/>
  </svg>`,

  /* 桥 */
  bridge: `
  <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
    <rect width="200" height="200" fill="#e0f2fe" rx="22"/>
    <circle cx="162" cy="42" r="16" fill="#fde047"/>
    <rect y="150" width="200" height="50" fill="#7dd3fc"/>
    <path d="M8 168 q12 -8 24 0 t24 0 t24 0 t24 0 t24 0 t24 0" stroke="#38bdf8" stroke-width="4" fill="none"/>
    <path d="M8 186 q12 -8 24 0 t24 0 t24 0 t24 0 t24 0 t24 0" stroke="#38bdf8" stroke-width="4" fill="none"/>
    <path d="M45 130 Q100 180 155 130 Z" fill="#fcd9a8" stroke="#a16207" stroke-width="5"/>
    <rect x="6" y="112" width="188" height="20" rx="9" fill="#d97706"/>
    <rect x="6" y="112" width="188" height="9" rx="4" fill="#f59e0b"/>
    <rect x="34" y="86" width="7" height="28" rx="3" fill="#a16207"/>
    <rect x="96" y="86" width="7" height="28" rx="3" fill="#a16207"/>
    <rect x="158" y="86" width="7" height="28" rx="3" fill="#a16207"/>
    <rect x="18" y="84" width="164" height="9" rx="4" fill="#ca8a04"/>
  </svg>`,

  /* 菊花 */
  chrysanthemum: `
  <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
    <rect width="200" height="200" fill="#fef9e7" rx="22"/>
    <path d="M100 110 L100 188" stroke="#4d7c0f" stroke-width="8" stroke-linecap="round"/>
    <path d="M100 158 q-30 -6 -42 -32 q30 0 42 32" fill="#65a30d"/>
    <path d="M100 145 q30 -6 42 -32 q-30 0 -42 32" fill="#84cc16"/>
    <g transform="translate(100,95)">
      ${[0,30,60,90,120,150,180,210,240,270,300,330].map(a=>`<ellipse cx="0" cy="-42" rx="13" ry="28" fill="#fbbf24" transform="rotate(${a})"/>`).join('')}
      ${[15,45,75,105,135,165,195,225,255,285,315,345].map(a=>`<ellipse cx="0" cy="-30" rx="11" ry="20" fill="#fcd34d" transform="rotate(${a})"/>`).join('')}
      <circle r="16" fill="#f59e0b"/>
      <circle r="9" fill="#d97706"/>
    </g>
  </svg>`,

  /* 蔬菜 */
  veg: `
  <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
    <rect width="200" height="200" fill="#f0fdf4" rx="22"/>
    <ellipse cx="70" cy="88" rx="30" ry="48" fill="#4ade80" transform="rotate(-25 70 88)"/>
    <ellipse cx="110" cy="76" rx="30" ry="50" fill="#22c55e" transform="rotate(8 110 76)"/>
    <ellipse cx="142" cy="98" rx="26" ry="42" fill="#86efac" transform="rotate(32 142 98)"/>
    <path d="M100 118 q-6 34 4 58" stroke="#15803d" stroke-width="8" fill="none" stroke-linecap="round"/>
    <path d="M56 122 L86 180 L30 164 Z" fill="#f97316" stroke="#ea580c" stroke-width="4" stroke-linejoin="round"/>
    <path d="M56 122 l-10 -16 M56 122 l4 -20 M56 122 l16 -14" stroke="#22c55e" stroke-width="6" stroke-linecap="round"/>
    <circle cx="136" cy="150" r="22" fill="#ef4444"/>
    <path d="M136 128 l-9 -11 M136 128 l9 -11 M136 128 l0 -13" stroke="#22c55e" stroke-width="5" stroke-linecap="round"/>
  </svg>`
};

/* =========================================================
   ③ 题目数据（想改题目/换图片，只改这里就行）
   ========================================================= */
const QUESTIONS = [
  /* --- 看图选词语 --- */
  { type:'image', svg:'bowl',            prompt:'看图选词语：这是什么？', answer:'米线', options:['米线','桥','菊花','蔬菜'] },
  { type:'image', svg:'bridge',          prompt:'看图选词语：这是什么？', answer:'桥',   options:['米线','桥','菊花','蔬菜'] },
  { type:'image', svg:'chrysanthemum',   prompt:'看图选词语：这是什么花？', answer:'菊花', options:['米线','桥','菊花','蔬菜'] },
  { type:'image', svg:'veg',             prompt:'看图选词语：这是什么？', answer:'蔬菜', options:['米线','桥','菊花','蔬菜'] },

  /* --- 读句子选词语 --- */
  { type:'text', prompt:'读句子，选词语填空。', sentence:'鸡汤上的油可以（　），让汤不容易变凉。',           answer:'保温', options:['保温','盖子','桥'] },
  { type:'text', prompt:'读句子，选词语填空。', sentence:'鸡汤上的油像（　）一样，把热气盖在下面。',         answer:'盖子', options:['盖子','保温','岛'] },
  { type:'text', prompt:'读句子，选词语填空。', sentence:'妻子每天都要过（　）去岛上给丈夫送饭。',           answer:'桥',   options:['桥','岛','菊花'] },
  { type:'text', prompt:'读句子，选词语填空。', sentence:'妻子每天都要过桥去（　）上给丈夫送饭。',           answer:'岛',   options:['岛','桥','蔬菜'] },
  { type:'text', prompt:'读句子，选词语填空。', sentence:'过桥米线可以加很多（　），如鸡肉、鱼肉等。',       answer:'配料', options:['配料','保温','盖子'] }
];

/* =========================================================
   ④ 游戏逻辑
   ========================================================= */
const $ = id => document.getElementById(id);
const KEY = 'gm_leaderboard_v1';

let state = { name:'', idx:0, correct:0, wrong:0, locked:false, startTime:0, timerId:null, lastTs:0 };

/* ---------- 工具 ---------- */
function shuffle(arr){
  const a = arr.slice();
  for(let i=a.length-1;i>0;i--){
    const j = Math.floor(Math.random()*(i+1));
    [a[i],a[j]] = [a[j],a[i]];
  }
  return a;
}
function fmt(sec){
  const m = String(Math.floor(sec/60)).padStart(2,'0');
  const s = String(sec%60).padStart(2,'0');
  return m + ':' + s;
}
function escapeHtml(s){
  return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}
function showScreen(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  $(id).classList.add('active');
  window.scrollTo({top:0, behavior:'smooth'});
}
let toastTimer = null;
function toast(msg, ok){
  const t = $('toast');
  t.textContent = msg;
  t.className = 'toast show ' + (ok === true ? 'ok' : ok === false ? 'no' : '');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(()=>{ t.className = 'toast'; }, 1500);
}

/* ---------- 开始页 ---------- */
$('heroArt').innerHTML = ART.bowl;

$('startBtn').onclick = () => {
  const name = ($('nameInput').value || '').trim() || '匿名同学';
  state = { name, idx:0, correct:0, wrong:0, locked:false, startTime:Date.now(), timerId:null, lastTs:0 };
  state.timerId = setInterval(tick, 250);
  $('timerText').textContent = '00:00';
  showScreen('screen-quiz');
  renderQuestion();
};
$('nameInput').addEventListener('keydown', e => { if(e.key === 'Enter') $('startBtn').click(); });

function tick(){
  $('timerText').textContent = fmt(Math.floor((Date.now() - state.startTime)/1000));
}

/* ---------- 渲染题目 ---------- */
function renderQuestion(){
  const q = QUESTIONS[state.idx];
  state.locked = false;

  $('progressText').textContent = `第 ${state.idx + 1} / ${QUESTIONS.length} 关`;
  $('progressFill').style.width = ((state.idx) / QUESTIONS.length * 100) + '%';

  let html = '';
  if(q.type === 'image'){
    html += `<div class="art-card">${ART[q.svg]}</div>`;
    html += `<p class="q-prompt">👀 ${q.prompt}</p>`;
  }else{
    html += `<div class="sentence-card">${q.sentence.replace('（　）','<span class="blank">（　）</span>')}</div>`;
    html += `<p class="q-prompt">📖 ${q.prompt}</p>`;
  }
  html += '<div class="options">';
  shuffle(q.options).forEach(w => {
    html += `<button class="option" data-word="${escapeHtml(w)}">${escapeHtml(w)}</button>`;
  });
  html += '</div>';

  $('questionArea').innerHTML = html;
  $('questionArea').querySelectorAll('.option').forEach(btn => {
    btn.onclick = () => choose(btn, q);
  });
}

/* ---------- 选择答案 ---------- */
function choose(btn, q){
  if(state.locked) return;
  state.locked = true;

  const ok = btn.dataset.word === q.answer;
  const all = $('questionArea').querySelectorAll('.option');

  all.forEach(o => {
    o.classList.add('disabled');
    if(o.dataset.word === q.answer) o.classList.add('correct');
  });

  if(ok){
    state.correct++;
    toast('答对啦！🎉', true);
  }else{
    state.wrong++;
    btn.classList.add('wrong');
    toast(`再想想～ 正确答案是「${q.answer}」`, false);
  }

  setTimeout(nextQuestion, ok ? 780 : 1450);
}

function nextQuestion(){
  state.idx++;
  if(state.idx >= QUESTIONS.length) finish();
  else renderQuestion();
}

/* ---------- 结算 ---------- */
function finish(){
  clearInterval(state.timerId);
  $('progressFill').style.width = '100%';

  const timeSec = Math.max(1, Math.round((Date.now() - state.startTime)/1000));
  const score = Math.max(0, state.correct*100 - state.wrong*50 - Math.floor(timeSec/2));
  const ts = Date.now();
  state.lastTs = ts;

  const record = { name:state.name, score, correct:state.correct, wrong:state.wrong, time:timeSec, ts };
  saveRecord(record);

  $('statTime').textContent    = fmt(timeSec);
  $('statCorrect').textContent = state.correct;
  $('statWrong').textContent   = state.wrong;
  $('statScore').textContent   = score;

  let medal = '💪', title = '继续加油！';
  if(state.wrong === 0 && state.correct === QUESTIONS.length){ medal='🏅'; title='全对！太厉害了！'; }
  else if(state.correct >= 7){ medal='🎉'; title='非常棒！'; }
  else if(state.correct >= 5){ medal='👍'; title='完成闯关！'; }
  $('medal').textContent = medal;
  $('resultTitle').textContent = title;

  showScreen('screen-result');
}

/* ---------- 成绩存取 ---------- */
function saveRecord(record){
  let list = [];
  try{ list = JSON.parse(localStorage.getItem(KEY) || '[]'); }catch(e){ list = []; }
  list.push(record);
  try{ localStorage.setItem(KEY, JSON.stringify(list)); }catch(e){}

  if(SUBMIT_URL){
    fetch(SUBMIT_URL, {
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body: JSON.stringify(record)
    }).catch(()=>{});
  }
}

function renderBoard(){
  let list = [];
  try{ list = JSON.parse(localStorage.getItem(KEY) || '[]'); }catch(e){ list = []; }
  list.sort((a,b) => (b.score - a.score) || (a.time - b.time));

  const box = $('boardList');
  if(!list.length){
    box.innerHTML = '<p class="empty">还没有成绩，快来当第一名吧！</p>';
    return;
  }
  box.innerHTML = list.map((r, i) => {
    const rank = i===0 ? '🥇' : i===1 ? '🥈' : i===2 ? '🥉' : (i+1);
    const me = (r.ts === state.lastTs) ? ' me' : '';
    return `<div class="row${me}">
      <span class="rank">${rank}</span>
      <div class="rmain">
        <div class="rname">${escapeHtml(r.name)}</div>
        <div class="rdetail">答对 ${r.correct} · 答错 ${r.wrong} · 用时 ${fmt(r.time)}</div>
      </div>
      <span class="rscore">${r.score}</span>
    </div>`;
  }).join('');
}

/* ---------- 按钮绑定 ---------- */
$('toBoardBtn').onclick = () => { renderBoard(); showScreen('screen-board'); };
$('toBoardBtn2').onclick = () => { renderBoard(); showScreen('screen-board'); };
$('backBtn').onclick = () => showScreen(state.startTime && state.idx < QUESTIONS.length ? 'screen-quiz' : 'screen-start');
$('againBtn').onclick = () => {
  clearInterval(state.timerId);
  showScreen('screen-start');
  $('nameInput').value = state.name === '匿名同学' ? '' : state.name;
};
$('clearBtn').onclick = () => {
  if(confirm('确定清空本机保存的所有成绩吗？')){
    localStorage.removeItem(KEY);
    renderBoard();
    toast('已清空', true);
  }
};
</script>
</body>
</html>