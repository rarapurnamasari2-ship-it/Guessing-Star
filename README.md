<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Guess the Star — Spot the Icon</title>
<style>
  :root{
    --navy: #0A0E27;
    --navy-deep: #050714;
    --gold: #D4AF37;
    --gold-soft: #F0D68A;
    --magenta: #FF3D7F;
    --cream: #F3EEDF;
    --muted: #8B90B3;
    --panel: #12163A;
    --panel-line: #262C5C;
    --good: #4ADE80;
    --bad: #FF5C7A;
  }

  *{ box-sizing: border-box; }

  body{
    margin:0;
    min-height:100vh;
    background:
      radial-gradient(ellipse 60% 40% at 50% 0%, rgba(212,175,55,0.10), transparent 60%),
      var(--navy);
    background-attachment: fixed;
    font-family: 'Poppins', 'Segoe UI', sans-serif;
    color: var(--cream);
    position: relative;
    overflow-x:hidden;
  }

  /* twinkle field */
  .stars{
    position:fixed; inset:0; z-index:0; pointer-events:none;
  }
  .star{
    position:absolute; width:2px; height:2px; background:var(--gold-soft);
    border-radius:50%; opacity:0.5;
    animation: twinkle 3s ease-in-out infinite;
  }
  @keyframes twinkle{
    0%,100%{ opacity:.15; transform:scale(1); }
    50%{ opacity:.9; transform:scale(1.6); }
  }

  .wrap{
    position:relative; z-index:1;
    max-width: 680px;
    margin: 0 auto;
    padding: 28px 20px 60px;
  }

  /* MARQUEE HEADER */
  .marquee{
    text-align:center;
    margin-bottom: 22px;
  }
  .marquee .eyebrow{
    font-size: 11px;
    letter-spacing: 4px;
    color: var(--gold);
    text-transform: uppercase;
    display:inline-block;
    padding: 4px 14px;
    border: 1px solid rgba(212,175,55,0.4);
    border-radius: 999px;
    margin-bottom: 12px;
  }
  h1{
    font-family: 'Georgia', 'Playfair Display', serif;
    font-size: clamp(30px, 7vw, 46px);
    margin: 0 0 6px;
    letter-spacing: 1px;
    background: linear-gradient(180deg, var(--gold-soft), var(--gold) 60%, #A9822A);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    text-shadow: 0 4px 30px rgba(212,175,55,0.25);
  }
  .sub{
    color: var(--muted);
    font-size: 14px;
    margin: 0;
  }

  /* SCOREBOARD */
  .scoreboard{
    display:flex;
    justify-content: space-between;
    align-items:center;
    background: var(--panel);
    border: 1px solid var(--panel-line);
    border-radius: 16px;
    padding: 14px 18px;
    margin-bottom: 18px;
  }
  .stat{ text-align:center; flex:1; }
  .stat .num{ font-size: 20px; font-weight:700; color: var(--gold-soft); font-family: 'Georgia', serif; }
  .stat .lbl{ font-size: 10px; letter-spacing: 2px; color: var(--muted); text-transform: uppercase; margin-top:2px; }
  .divider-v{ width:1px; background: var(--panel-line); align-self: stretch; margin: 0 6px; }
  .lives{ font-size: 16px; letter-spacing: 3px; }

  /* TIMER BAR */
  .timer-track{
    height: 6px;
    background: rgba(255,255,255,0.06);
    border-radius: 999px;
    overflow: hidden;
    margin-bottom: 22px;
  }
  .timer-fill{
    height:100%;
    background: linear-gradient(90deg, var(--magenta), var(--gold));
    width:100%;
    transition: width 1s linear;
    border-radius: 999px;
  }

  /* STAGE */
  .stage{
    position: relative;
    background: radial-gradient(ellipse 80% 60% at 50% 20%, rgba(212,175,55,0.16), transparent 65%), var(--panel);
    border: 1px solid var(--panel-line);
    border-radius: 24px;
    padding: 36px 24px 28px;
    text-align:center;
    margin-bottom: 20px;
    overflow:hidden;
  }
  .spot-beam{
    position:absolute;
    top:-40px; left:50%;
    width: 340px; height: 240px;
    transform: translateX(-50%);
    background: conic-gradient(from 200deg at 50% 0%, transparent 0deg, rgba(255,255,255,0.10) 90deg, transparent 180deg);
    filter: blur(2px);
    pointer-events:none;
  }
  .category-badge{
    display:inline-block;
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 5px 14px;
    border-radius: 999px;
    margin-bottom: 18px;
    font-weight: 600;
  }
  .cat-Idol{ background: rgba(255,61,127,0.15); color: var(--magenta); border:1px solid rgba(255,61,127,0.4); }
  .cat-Actor{ background: rgba(212,175,55,0.15); color: var(--gold-soft); border:1px solid rgba(212,175,55,0.4); }
  .cat-Athlete{ background: rgba(74,222,128,0.15); color: var(--good); border:1px solid rgba(74,222,128,0.4); }

  .avatar{
    width: 96px; height: 96px;
    margin: 0 auto 18px;
    border-radius: 50%;
    background: linear-gradient(160deg, #1A1F4D, #0A0E27);
    border: 2px solid var(--gold);
    display:flex; align-items:center; justify-content:center;
    font-size: 42px;
    position:relative;
    box-shadow: 0 0 0 6px rgba(212,175,55,0.08), 0 10px 30px rgba(0,0,0,0.4);
  }
  .avatar.reveal-glow{
    animation: pulseGlow 0.6s ease;
  }
  @keyframes pulseGlow{
    0%{ box-shadow: 0 0 0 0 rgba(212,175,55,0.6); }
    100%{ box-shadow: 0 0 0 14px rgba(212,175,55,0); }
  }

  .clue-box{
    min-height: 66px;
    display:flex; align-items:center; justify-content:center;
    padding: 0 8px;
  }
  .clue-text{
    font-size: 17px;
    line-height:1.5;
    color: var(--cream);
    font-style: italic;
  }
  .clue-count{
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 1px;
    margin-top: 10px;
    text-transform: uppercase;
  }

  .reveal-btn{
    margin-top: 16px;
    background: transparent;
    border: 1px dashed rgba(212,175,55,0.5);
    color: var(--gold-soft);
    padding: 8px 18px;
    border-radius: 999px;
    font-size: 12px;
    letter-spacing: 1px;
    cursor:pointer;
    transition: all .2s ease;
    font-family: inherit;
  }
  .reveal-btn:hover:not(:disabled){ background: rgba(212,175,55,0.1); transform: translateY(-1px); }
  .reveal-btn:disabled{ opacity: 0.3; cursor: default; }

  /* OPTIONS */
  .options{
    display:grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 8px;
  }
  .opt{
    background: var(--panel);
    border: 1px solid var(--panel-line);
    color: var(--cream);
    padding: 16px 12px;
    border-radius: 14px;
    font-size: 14.5px;
    font-weight: 600;
    cursor: pointer;
    font-family: inherit;
    transition: all .15s ease;
    text-align:center;
  }
  .opt:hover:not(:disabled){
    border-color: var(--gold);
    transform: translateY(-2px);
    background: #171B45;
  }
  .opt:disabled{ cursor: default; }
  .opt.correct{
    background: rgba(74,222,128,0.15);
    border-color: var(--good);
    color: var(--good);
  }
  .opt.wrong{
    background: rgba(255,92,122,0.15);
    border-color: var(--bad);
    color: var(--bad);
  }
  .opt.dim{ opacity: 0.35; }

  .feedback{
    text-align:center;
    min-height: 26px;
    font-size: 13px;
    font-weight:600;
    letter-spacing: 0.5px;
    margin: 10px 0 4px;
  }
  .feedback.good-txt{ color: var(--good); }
  .feedback.bad-txt{ color: var(--bad); }

  .next-btn{
    display:block;
    width:100%;
    margin-top: 10px;
    background: linear-gradient(135deg, var(--gold), #A9822A);
    color: var(--navy-deep);
    border:none;
    padding: 14px;
    border-radius: 14px;
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 1px;
    text-transform: uppercase;
    cursor:pointer;
    font-family: inherit;
    opacity: 0;
    pointer-events:none;
    transform: translateY(6px);
    transition: all .25s ease;
  }
  .next-btn.show{ opacity:1; pointer-events:auto; transform:none; }
  .next-btn:hover{ filter: brightness(1.08); }

  /* START / END SCREEN */
  .center-screen{
    text-align:center;
    background: var(--panel);
    border: 1px solid var(--panel-line);
    border-radius: 24px;
    padding: 40px 26px;
  }
  .center-screen h2{
    font-family: 'Georgia', serif;
    font-size: 26px;
    color: var(--gold-soft);
    margin: 6px 0 10px;
  }
  .center-screen p{ color: var(--muted); font-size: 14px; margin-bottom: 26px; line-height:1.6;}
  .big-emoji{ font-size: 52px; margin-bottom: 6px; }
  .rank-title{
    font-size: 15px;
    color: var(--magenta);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 4px;
  }
  .primary-btn{
    background: linear-gradient(135deg, var(--magenta), #C22A5C);
    color: white;
    border:none;
    padding: 15px 36px;
    border-radius: 999px;
    font-size: 14px;
    font-weight:700;
    letter-spacing: 1px;
    text-transform: uppercase;
    cursor:pointer;
    font-family: inherit;
    box-shadow: 0 8px 24px rgba(255,61,127,0.35);
  }
  .primary-btn:hover{ filter: brightness(1.08); transform: translateY(-1px); }

  .hidden{ display:none !important; }

  .progress-dots{
    display:flex; justify-content:center; gap:6px; margin-top: 16px;
  }
  .dot{
    width:7px; height:7px; border-radius:50%;
    background: var(--panel-line);
  }
  .dot.done{ background: var(--gold); }
  .dot.current{ background: var(--magenta); }

  @media (prefers-reduced-motion: reduce){
    *{ animation: none !important; transition: none !important; }
  }
</style>
</head>
<body>

<div class="stars" id="stars"></div>

<div class="wrap">

  <div class="marquee">
    <span class="eyebrow">World Icons Quiz</span>
    <h1>Guess the Star</h1>
    <p class="sub">Idols · Actors & Actresses · Athletes — spot the icon from the clues</p>
  </div>

  <!-- START SCREEN -->
  <div id="startScreen" class="center-screen">
    <div class="big-emoji">🌟</div>
    <h2>Ready for the Spotlight?</h2>
    <p>
      You'll get 10 rounds. Each round, one clue appears at a time about a world-famous<br>
      idol, actor/actress, or athlete. Guess early for more points — but be careful,<br>
      you only have 3 lives!
    </p>
    <button class="primary-btn" onclick="startGame()">Start the Show</button>
  </div>

  <!-- GAME SCREEN -->
  <div id="gameScreen" class="hidden">

    <div class="scoreboard">
      <div class="stat">
        <div class="num" id="scoreVal">0</div>
        <div class="lbl">Score</div>
      </div>
      <div class="divider-v"></div>
      <div class="stat">
        <div class="num" id="roundVal">1 / 10</div>
        <div class="lbl">Round</div>
      </div>
      <div class="divider-v"></div>
      <div class="stat">
        <div class="lives" id="livesVal">❤️❤️❤️</div>
        <div class="lbl">Lives</div>
      </div>
    </div>

    <div class="timer-track"><div class="timer-fill" id="timerFill"></div></div>

    <div class="stage">
      <div class="spot-beam"></div>
      <span class="category-badge" id="categoryBadge">Idol</span>
      <div class="avatar" id="avatar">🎤</div>

      <div class="clue-box">
        <div class="clue-text" id="clueText">Loading clue...</div>
      </div>
      <div class="clue-count" id="clueCount">Clue 1 of 3</div>

      <button class="reveal-btn" id="revealBtn" onclick="revealNextClue()">Reveal Another Clue (-10 pts)</button>
    </div>

    <div class="options" id="optionsGrid"></div>

    <div class="feedback" id="feedback"></div>
    <button class="next-btn" id="nextBtn" onclick="nextRound()">Next Round →</button>

    <div class="progress-dots" id="progressDots"></div>
  </div>

  <!-- END SCREEN -->
  <div id="endScreen" class="center-screen hidden">
    <div class="big-emoji" id="endEmoji">🏆</div>
    <div class="rank-title" id="rankTitle">Superstar</div>
    <h2 id="endHeadline">Show's Over!</h2>
    <p id="endSummary">You scored big tonight.</p>
    <button class="primary-btn" onclick="restartGame()">Play Again</button>
  </div>

</div>

<script>
/* ---------- DATA ---------- */
/* Clues only — no images/lyrics reproduced. Purely factual, publicly known career facts. */
const POOL = [
  {
    name: "Taylor Swift",
    category: "Idol",
    avatar: "🎤",
    clues: [
      "This American singer-songwriter re-recorded several of her early albums after a dispute over ownership of her masters.",
      "She has won Album of the Year at the Grammys a record-breaking four times.",
      "Her record-breaking world tour in 2023-2024 was named after the concept of 'eras' in her career."
    ],
    options: ["Taylor Swift", "Ariana Grande", "Katy Perry", "Billie Eilish"]
  },
  {
    name: "BTS",
    category: "Idol",
    avatar: "🎶",
    clues: [
      "This seven-member group debuted in Seoul in 2013 under a small label.",
      "Their fandom is officially called 'ARMY' and is known for massive global streaming power.",
      "The group addressed the United Nations General Assembly and later paused for members' mandatory military service."
    ],
    options: ["BTS", "EXO", "Blackpink", "Seventeen"]
  },
  {
    name: "Beyoncé",
    category: "Idol",
    avatar: "👑",
    clues: [
      "She first rose to fame as the lead singer of a girl group in the late 1990s.",
      "She holds the record for the most Grammy wins of any artist in history.",
      "Her surprise visual album in 2016 explored themes of identity, marriage, and heritage."
    ],
    options: ["Beyoncé", "Rihanna", "Alicia Keys", "Jennifer Lopez"]
  },
  {
    name: "Leonardo DiCaprio",
    category: "Actor",
    avatar: "🎬",
    clues: [
      "This Hollywood actor was famously nominated for an Oscar multiple times before finally winning.",
      "He played a shipwrecked artist in a 1997 blockbuster about a sinking ocean liner.",
      "He finally won Best Actor for a 2015 survival film set in the American wilderness."
    ],
    options: ["Leonardo DiCaprio", "Brad Pitt", "Matt Damon", "Tom Hanks"]
  },
  {
    name: "Meryl Streep",
    category: "Actor",
    avatar: "🎭",
    clues: [
      "This actress holds the record for the most Academy Award acting nominations of all time.",
      "She played a demanding fashion magazine editor in a well-known 2006 comedy-drama.",
      "She has portrayed several real-life political and public figures throughout her career."
    ],
    options: ["Meryl Streep", "Judi Dench", "Cate Blanchett", "Nicole Kidman"]
  },
  {
    name: "Zendaya",
    category: "Actor",
    avatar: "✨",
    clues: [
      "This actress started her career as a Disney Channel star before moving into major films.",
      "She became one of the youngest people ever to win the Emmy for Lead Actress in a Drama.",
      "She stars as a tough desert-planet heroine in a major sci-fi film franchise released in the 2020s."
    ],
    options: ["Zendaya", "Sydney Sweeney", "Anya Taylor-Joy", "Florence Pugh"]
  },
  {
    name: "Dwayne Johnson",
    category: "Actor",
    avatar: "💪",
    clues: [
      "Before acting, this star was a professional wrestler known by a one-word nickname.",
      "He has appeared in a long-running car-chase action franchise.",
      "His nickname refers to a precious gemstone and a common first name."
    ],
    options: ["Dwayne Johnson", "John Cena", "Vin Diesel", "Jason Statham"]
  },
  {
    name: "Cristiano Ronaldo",
    category: "Athlete",
    avatar: "⚽",
    clues: [
      "This footballer from Madeira, Portugal is one of the highest goal-scorers in the sport's history.",
      "He has played for major clubs in England, Spain, Italy, and Saudi Arabia.",
      "He is famous for his 'Siuu' goal celebration and long-standing rivalry with an Argentine star."
    ],
    options: ["Cristiano Ronaldo", "Lionel Messi", "Neymar", "Kylian Mbappé"]
  },
  {
    name: "Lionel Messi",
    category: "Athlete",
    avatar: "🏆",
    clues: [
      "This Argentine footballer spent most of his early career at a single Spanish club.",
      "He has won the Ballon d'Or award more times than any other player in history.",
      "He finally lifted the FIFA World Cup trophy with his national team in Qatar in 2022."
    ],
    options: ["Lionel Messi", "Cristiano Ronaldo", "Luka Modrić", "Neymar"]
  },
  {
    name: "Serena Williams",
    category: "Athlete",
    avatar: "🎾",
    clues: [
      "This American athlete dominated women's tennis for over two decades.",
      "She won 23 Grand Slam singles titles, the most in the Open Era.",
      "She often competed alongside her older sister in doubles matches."
    ],
    options: ["Serena Williams", "Venus Williams", "Naomi Osaka", "Maria Sharapova"]
  },
  {
    name: "LeBron James",
    category: "Athlete",
    avatar: "🏀",
    clues: [
      "This basketball player was drafted straight out of high school as the number one overall pick.",
      "He has played for teams based in Ohio, Florida, and California over his long career.",
      "He became the NBA's all-time leading scorer, passing a record once held by Kareem Abdul-Jabbar."
    ],
    options: ["LeBron James", "Kevin Durant", "Stephen Curry", "Kobe Bryant"]
  },
  {
    name: "Usain Bolt",
    category: "Athlete",
    avatar: "🏃",
    clues: [
      "This sprinter from Jamaica is nicknamed 'Lightning Bolt.'",
      "He still holds the world records in both the 100m and 200m sprints.",
      "He famously celebrated wins with a signature lightning-bolt pose."
    ],
    options: ["Usain Bolt", "Yohan Blake", "Noah Lyles", "Justin Gatlin"]
  }
];

/* ---------- STATE ---------- */
let deck = [];
let currentIndex = 0;
let score = 0;
let lives = 3;
let clueLevel = 1;
let timer = null;
let timeLeft = 20;
const TOTAL_ROUNDS = 10;
let roundLocked = false;

/* ---------- STARFIELD ---------- */
function buildStars(){
  const el = document.getElementById('stars');
  let html = '';
  for(let i=0;i<50;i++){
    const top = Math.random()*100;
    const left = Math.random()*100;
    const delay = Math.random()*3;
    const size = Math.random()<0.2 ? 3 : 2;
    html += `<div class="star" style="top:${top}%;left:${left}%;animation-delay:${delay}s;width:${size}px;height:${size}px;"></div>`;
  }
  el.innerHTML = html;
}
buildStars();

/* ---------- HELPERS ---------- */
function shuffle(arr){
  const a = [...arr];
  for(let i=a.length-1;i>0;i--){
    const j = Math.floor(Math.random()*(i+1));
    [a[i],a[j]] = [a[j],a[i]];
  }
  return a;
}

function startGame(){
  deck = shuffle(POOL).slice(0, TOTAL_ROUNDS);
  currentIndex = 0;
  score = 0;
  lives = 3;
  document.getElementById('startScreen').classList.add('hidden');
  document.getElementById('endScreen').classList.add('hidden');
  document.getElementById('gameScreen').classList.remove('hidden');
  buildDots();
  loadRound();
}

function restartGame(){
  document.getElementById('endScreen').classList.add('hidden');
  document.getElementById('startScreen').classList.remove('hidden');
}

function buildDots(){
  const dots = document.getElementById('progressDots');
  dots.innerHTML = '';
  for(let i=0;i<TOTAL_ROUNDS;i++){
    const d = document.createElement('div');
    d.className = 'dot';
    d.id = 'dot-'+i;
    dots.appendChild(d);
  }
}

function updateDots(){
  for(let i=0;i<TOTAL_ROUNDS;i++){
    const d = document.getElementById('dot-'+i);
    d.classList.remove('done','current');
    if(i < currentIndex) d.classList.add('done');
    else if(i === currentIndex) d.classList.add('current');
  }
}

function loadRound(){
  if(currentIndex >= TOTAL_ROUNDS || lives <= 0){
    endGame();
    return;
  }
  roundLocked = false;
  clueLevel = 1;
  timeLeft = 20;

  const item = deck[currentIndex];

  document.getElementById('scoreVal').textContent = score;
  document.getElementById('roundVal').textContent = `${currentIndex+1} / ${TOTAL_ROUNDS}`;
  document.getElementById('livesVal').textContent = '❤️'.repeat(lives) + '🖤'.repeat(3-lives);
  document.getElementById('categoryBadge').textContent = item.category;
  document.getElementById('categoryBadge').className = 'category-badge cat-' + item.category;
  document.getElementById('avatar').textContent = item.avatar;
  document.getElementById('avatar').classList.remove('reveal-glow');
  document.getElementById('clueText').textContent = item.clues[0];
  document.getElementById('clueCount').textContent = `Clue 1 of ${item.clues.length}`;
  document.getElementById('revealBtn').disabled = false;
  document.getElementById('revealBtn').textContent = 'Reveal Another Clue (-10 pts)';
  document.getElementById('feedback').textContent = '';
  document.getElementById('feedback').className = 'feedback';
  document.getElementById('nextBtn').classList.remove('show');

  const shuffledOptions = shuffle(item.options);
  const grid = document.getElementById('optionsGrid');
  grid.innerHTML = '';
  shuffledOptions.forEach(opt => {
    const btn = document.createElement('button');
    btn.className = 'opt';
    btn.textContent = opt;
    btn.onclick = () => selectAnswer(opt, btn);
    grid.appendChild(btn);
  });

  updateDots();
  startTimer();
}

function startTimer(){
  clearInterval(timer);
  const fill = document.getElementById('timerFill');
  fill.style.transition = 'none';
  fill.style.width = '100%';
  requestAnimationFrame(() => {
    fill.style.transition = `width ${timeLeft}s linear`;
    fill.style.width = '0%';
  });
  timer = setInterval(() => {
    timeLeft -= 1;
    if(timeLeft <= 0){
      clearInterval(timer);
      if(!roundLocked) handleTimeout();
    }
  }, 1000);
}

function handleTimeout(){
  roundLocked = true;
  const item = deck[currentIndex];
  lives -= 1;
  document.getElementById('livesVal').textContent = '❤️'.repeat(Math.max(lives,0)) + '🖤'.repeat(3-Math.max(lives,0));
  const feedback = document.getElementById('feedback');
  feedback.textContent = `⏱ Time's up! It was ${item.name}.`;
  feedback.className = 'feedback bad-txt';
  lockOptions(item.name);
  document.getElementById('revealBtn').disabled = true;
  document.getElementById('nextBtn').classList.add('show');
}

function revealNextClue(){
  const item = deck[currentIndex];
  if(clueLevel >= item.clues.length) return;
  clueLevel += 1;
  score = Math.max(0, score - 10);
  document.getElementById('scoreVal').textContent = score;
  document.getElementById('clueText').textContent = item.clues[clueLevel-1];
  document.getElementById('clueCount').textContent = `Clue ${clueLevel} of ${item.clues.length}`;
  document.getElementById('avatar').classList.remove('reveal-glow');
  void document.getElementById('avatar').offsetWidth;
  document.getElementById('avatar').classList.add('reveal-glow');
  if(clueLevel >= item.clues.length){
    document.getElementById('revealBtn').disabled = true;
    document.getElementById('revealBtn').textContent = 'No more clues';
  }
}

function selectAnswer(chosen, btnEl){
  if(roundLocked) return;
  roundLocked = true;
  clearInterval(timer);
  document.getElementById('revealBtn').disabled = true;

  const item = deck[currentIndex];
  const correct = chosen === item.name;
  const feedback = document.getElementById('feedback');

  if(correct){
    const bonus = Math.max(0, (item.clues.length - clueLevel)) * 15;
    const base = 30;
    const gained = base + bonus;
    score += gained;
    feedback.textContent = `✅ Correct! +${gained} points`;
    feedback.className = 'feedback good-txt';
    btnEl.classList.add('correct');
  } else {
    lives -= 1;
    feedback.textContent = `❌ Not quite — it was ${item.name}.`;
    feedback.className = 'feedback bad-txt';
    btnEl.classList.add('wrong');
  }

  lockOptions(item.name);
  document.getElementById('scoreVal').textContent = score;
  document.getElementById('livesVal').textContent = '❤️'.repeat(Math.max(lives,0)) + '🖤'.repeat(3-Math.max(lives,0));
  document.getElementById('nextBtn').classList.add('show');
}

function lockOptions(correctName){
  const opts = document.querySelectorAll('.opt');
  opts.forEach(o => {
    o.disabled = true;
    if(o.textContent === correctName && !o.classList.contains('wrong')){
      o.classList.add('correct');
    } else if(!o.classList.contains('correct') && !o.classList.contains('wrong')){
      o.classList.add('dim');
    }
  });
}

function nextRound(){
  currentIndex += 1;
  if(lives <= 0 || currentIndex >= TOTAL_ROUNDS){
    endGame();
  } else {
    loadRound();
  }
}

function endGame(){
  clearInterval(timer);
  document.getElementById('gameScreen').classList.add('hidden');
  document.getElementById('endScreen').classList.remove('hidden');

  const maxPossible = TOTAL_ROUNDS * 30 + TOTAL_ROUNDS * 30; // rough ceiling reference
  let rank, emoji, headline;

  if(lives <= 0){
    rank = "Backstage Pass";
    emoji = "🎫";
    headline = "The Show Must Go On";
  } else if(score >= 400){
    rank = "A-List Superstar";
    emoji = "🏆";
    headline = "Incredible Performance!";
  } else if(score >= 250){
    rank = "Rising Star";
    emoji = "🌟";
    headline = "Great Job!";
  } else if(score >= 120){
    rank = "Fan Favorite";
    emoji = "🎬";
    headline = "Nicely Done!";
  } else {
    rank = "Rookie Spotter";
    emoji = "🔦";
    headline = "Keep Practicing!";
  }

  document.getElementById('endEmoji').textContent = emoji;
  document.getElementById('rankTitle').textContent = rank;
  document.getElementById('endHeadline').textContent = headline;
  document.getElementById('endSummary').textContent =
    `Final score: ${score} points, ${Math.min(currentIndex, TOTAL_ROUNDS)} rounds played, ${Math.max(lives,0)} ${lives===1?'life':'lives'} remaining.`;
}
</script>

</body>
</html>
