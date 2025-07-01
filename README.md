<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>지구를 구하는 에코 챌린지</title>
  <!-- Google Fonts: Noto Sans KR -->
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --green: #2E7D32;
      --blue: #0288D1;
      --white: #fff;
      --red: #D32F2F;
      --gray: #616161;
      --card-bg: #E8F5E9;
      --font-main: 'Noto Sans KR', Arial, sans-serif;
    }
    html, body {
      margin: 0; padding: 0; box-sizing: border-box;
      font-family: var(--font-main);
      background: var(--white);
      min-height: 100vh;
    }
    /* 공통 레이아웃 */
    .center {
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      min-height: 100vh;
    }
    /* 1. 시작 화면 */
    .start-bg {
      position: fixed; inset: 0; z-index: 0;
      background: url('https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=1200&q=80') center/cover no-repeat;
      opacity: 0;
      animation: fadeInBg 1s forwards;
    }
    @keyframes fadeInBg { to { opacity: 1; } }
    .start-title {
      font-size: 3rem; color: var(--white);
      text-shadow: 0 0 8px var(--green), 0 2px 0 #0006;
      margin-bottom: 2rem;
      letter-spacing: 0.05em;
      border-radius: 12px;
      padding: 0.5em 1em;
      background: rgba(46,125,50,0.15);
      transform: translateY(50px);
      opacity: 0;
      animation: slideUpTitle 0.8s 0.5s forwards;
    }
    @keyframes slideUpTitle {
      to { transform: translateY(0); opacity: 1; }
    }
    .start-btn {
      font-size: 1.5rem; font-weight: bold;
      background: var(--green); color: var(--white);
      border: none; border-radius: 32px; padding: 1em 2.5em;
      cursor: pointer; position: relative; overflow: hidden;
      box-shadow: 0 4px 16px #0002;
      transition: transform 0.2s;
      outline: none;
    }
    .start-btn:hover {
      animation: ripple 0.6s;
    }
    .start-btn:active {
      transform: scale(0.97);
    }
    /* 물결 효과 */
    @keyframes ripple {
      0% { box-shadow: 0 0 0 0 var(--white); }
      100% { box-shadow: 0 0 0 24px transparent; }
    }
    /* 반짝이 효과 */
    .sparkle {
      position: absolute; pointer-events: none;
      width: 8px; height: 8px; border-radius: 50%;
      background: var(--white); opacity: 0.8;
      animation: sparkle 0.7s linear forwards;
    }
    @keyframes sparkle {
      0% { transform: scale(0.5); opacity: 1; }
      100% { transform: scale(2.5); opacity: 0; }
    }
    /* 2. 미션 선택 화면 */
    .mission-bg {
      position: fixed; inset: 0; z-index: 0;
      background: linear-gradient(120deg, #e0f7fa 0%, #e8f5e9 100%);
      overflow: hidden;
    }
    .rotating-icons {
      position: absolute; width: 100vw; height: 100vh; pointer-events: none;
      z-index: 1;
    }
    .rotating-icons img {
      position: absolute; opacity: 0.15;
      animation: rotateIcon 10s linear infinite;
    }
    @keyframes rotateIcon {
      100% { transform: rotate(360deg); }
    }
    .mission-cards {
      display: flex; gap: 2vw; justify-content: center; align-items: center;
      margin-top: 4vh;
      z-index: 2;
    }
    .mission-card {
      background: var(--card-bg); border-radius: 24px;
      box-shadow: 0 2px 12px #0001;
      width: 260px; min-height: 180px; padding: 2rem 1.5rem;
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      font-size: 1.3rem; font-weight: 700;
      color: var(--green); border: 3px solid transparent;
      cursor: pointer; opacity: 0; transform: translateY(30px);
      transition: border 0.3s, transform 0.3s, box-shadow 0.3s, opacity 0.5s;
      margin-bottom: 2vh;
    }
    .mission-card:hover {
      border: 3px solid var(--blue);
      transform: translateY(-10px) scale(1.03);
      box-shadow: 0 8px 24px #0288d133;
    }
    .mission-card.card1 { animation: fadeInCard 0.7s 0.2s forwards; }
    .mission-card.card2 { animation: fadeInCard 0.7s 0.7s forwards; }
    .mission-card.card3 { animation: fadeInCard 0.7s 1.2s forwards; }
    @keyframes fadeInCard {
      to { opacity: 1; transform: translateY(0); }
    }
    /* 3. 퀴즈 미션 */
    .quiz-wrap {
      background: linear-gradient(120deg, #e8f5e9 0%, #e0f7fa 100%);
      border-radius: 24px; box-shadow: 0 2px 16px #0002;
      max-width: 420px; margin: 5vh auto; padding: 2.5rem 2rem 2rem 2rem;
      position: relative;
    }
    .quiz-q {
      font-size: 1.2rem; font-weight: 700; color: var(--green);
      margin-bottom: 2rem;
      opacity: 0; transform: translateY(-50px);
      animation: slideInQ 0.5s 0.1s forwards;
    }
    @keyframes slideInQ { to { opacity: 1; transform: translateY(0); } }
    .quiz-answers {
      display: flex; flex-direction: column; gap: 1rem;
    }
    .quiz-answers label {
      display: block; background: var(--white); color: #222;
      border-radius: 12px; padding: 1em; font-size: 1.1rem;
      cursor: pointer; border: 2px solid #eee;
      transition: background 0.3s, border 0.3s, color 0.3s, box-shadow 0.3s;
      box-shadow: 0 2px 8px #0001;
      position: relative;
    }
    .quiz-answers input[type="radio"] {
      display: none;
    }
    .quiz-answers input[type="radio"]:checked + span {
      font-weight: bold;
    }
    /* 정답/오답 효과 */
    .quiz-answers label.correct {
      animation: correctFlash 0.5s;
      background: var(--green); color: var(--white); border: 2px solid var(--green);
    }
    @keyframes correctFlash {
      0% { background: var(--green); color: var(--white); }
      50% { background: #a5d6a7; color: var(--green); }
      100% { background: var(--green); color: var(--white); }
    }
    .quiz-answers label.wrong {
      animation: shake 0.3s;
      background: var(--red); color: var(--white); border: 2px solid var(--red);
    }
    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-5px); }
      40%, 80% { transform: translateX(5px); }
    }
    /* 트리/쓰레기 시각 효과 */
    .tree-grow {
      width: 60px; height: 60px; margin: 1.5rem auto 0 auto;
      background: url('https://cdn-icons-png.flaticon.com/512/427/427735.png') center/contain no-repeat;
      transform: scale(0);
      animation: treeGrow 0.5s forwards;
    }
    @keyframes treeGrow { to { transform: scale(1); } }
    .trash-pile {
      width: 60px; height: 60px; margin: 1.5rem auto 0 auto;
      background: url('https://cdn-icons-png.flaticon.com/512/565/565547.png') center/contain no-repeat;
      opacity: 0;
      animation: trashAppear 0.5s forwards;
    }
    @keyframes trashAppear { to { opacity: 1; } }
    /* 점수 표시 */
    .score {
      position: absolute; top: 1rem; right: 1.5rem;
      font-size: 1.1rem; color: var(--blue); font-weight: bold;
      background: #fff8; border-radius: 8px; padding: 0.3em 1em;
      box-shadow: 0 2px 8px #0001;
    }
    /* 4. 실천 미션 */
    .action-wrap {
      max-width: 700px; margin: 5vh auto; padding: 2rem 1rem;
      background: #e8f5e9; border-radius: 24px; box-shadow: 0 2px 16px #0002;
      display: flex; flex-direction: column; align-items: center;
    }
    .action-items {
      display: flex; flex-wrap: wrap; gap: 1rem; justify-content: center;
      margin-bottom: 2rem;
    }
    .action-item {
      width: 100px; height: 50px; background: var(--white);
      border-radius: 12px; box-shadow: 0 2px 8px #0001;
      display: flex; align-items: center; justify-content: center;
      font-size: 1rem; font-weight: 500; color: #333;
      cursor: grab; position: relative;
      opacity: 0; transform: translateX(-100px);
      animation: slideInItem 0.5s forwards;
    }
    .action-item:nth-child(2) { animation-delay: 0.2s; }
    .action-item:nth-child(3) { animation-delay: 0.4s; }
    .action-item:nth-child(4) { animation-delay: 0.6s; }
    .action-item:nth-child(5) { animation-delay: 0.8s; }
    .action-item:nth-child(6) { animation-delay: 1s; }
    @keyframes slideInItem { to { opacity: 1; transform: translateX(0); } }
    .action-item.sparkle {
      animation: sparkleItem 0.7s;
    }
    @keyframes sparkleItem {
      0% { box-shadow: 0 0 0 0 var(--blue); }
      100% { box-shadow: 0 0 24px 12px transparent; opacity: 0; }
    }
    .action-item.bounce {
      animation: bounceBack 0.5s;
    }
    @keyframes bounceBack {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-10px); }
      40%, 80% { transform: translateX(10px); }
    }
    .drop-zones {
      display: flex; gap: 2rem; justify-content: center; width: 100%;
    }
    .drop-zone {
      flex: 1 1 0; min-width: 120px; min-height: 120px;
      border-radius: 20px; display: flex; flex-direction: column; align-items: center; justify-content: center;
      font-size: 1.2rem; font-weight: bold; color: var(--white);
      margin: 0 1vw; position: relative;
      box-shadow: 0 2px 12px #0001;
      animation: pulseZone 2s infinite alternate;
    }
    .drop-zone.school { background: var(--green); }
    .drop-zone.home { background: var(--blue); }
    @keyframes pulseZone {
      0% { transform: scale(1); }
      100% { transform: scale(1.05); }
    }
    /* 5. 청소 미션 */
    .cleanup-wrap {
      max-width: 800px; margin: 5vh auto; padding: 2rem 1rem;
      background: #e0f7fa; border-radius: 24px; box-shadow: 0 2px 16px #0002;
      display: flex; flex-direction: column; align-items: center;
    }
    .trash-area {
      display: flex; flex-wrap: wrap; gap: 1.5rem; justify-content: center; margin-bottom: 2rem;
      min-height: 120px;
    }
    .trash-item {
      width: 48px; height: 48px; background-size: contain; background-repeat: no-repeat;
      opacity: 0; animation: fadeInTrash 0.5s forwards;
      cursor: pointer; transition: transform 0.2s;
    }
    .trash-item:nth-child(1) { animation-delay: 0.1s; }
    .trash-item:nth-child(2) { animation-delay: 0.3s; }
    .trash-item:nth-child(3) { animation-delay: 0.5s; }
    .trash-item:nth-child(4) { animation-delay: 0.7s; }
    .trash-item:nth-child(5) { animation-delay: 0.9s; }
    .trash-item:nth-child(6) { animation-delay: 1.1s; }
    .trash-item:nth-child(7) { animation-delay: 1.3s; }
    .trash-item:nth-child(8) { animation-delay: 1.5s; }
    .trash-item:nth-child(9) { animation-delay: 1.7s; }
    .trash-item:nth-child(10) { animation-delay: 1.9s; }
    @keyframes fadeInTrash { to { opacity: 1; } }
    .trash-item.scale {
      transform: scale(1.2);
    }
    .bins {
      display: flex; gap: 2vw; justify-content: center; align-items: flex-end;
      margin-top: 1rem;
    }
    .bin {
      width: 70px; height: 90px; border-radius: 16px 16px 24px 24px;
      display: flex; flex-direction: column; align-items: center; justify-content: flex-end;
      font-size: 1rem; font-weight: bold; color: var(--white);
      margin: 0 1vw; position: relative;
      box-shadow: 0 2px 12px #0001;
      transition: transform 0.3s;
      background-size: 60% 60%; background-repeat: no-repeat; background-position: center 18px;
    }
    .bin.plastic { background: var(--blue); }
    .bin.paper { background: var(--green); }
    .bin.general { background: var(--gray); }
    .bin:hover { transform: scale(1.1); }
    .bin.flash {
      animation: binFlash 0.5s;
    }
    @keyframes binFlash {
      0% { box-shadow: 0 0 0 0 var(--green); }
      100% { box-shadow: 0 0 24px 12px transparent; }
    }
    .bin.shake {
      animation: shakeBin 0.4s;
    }
    @keyframes shakeBin {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-8px); }
      40%, 80% { transform: translateX(8px); }
    }
    /* 6. 결과 화면 */
    .result-wrap {
      background: linear-gradient(120deg, #e8f5e9 0%, #e0f7fa 100%);
      border-radius: 24px; box-shadow: 0 2px 16px #0002;
      max-width: 420px; margin: 5vh auto; padding: 2.5rem 2rem 2rem 2rem;
      position: relative; text-align: center;
      opacity: 0; animation: fadeInResult 1s 0.2s forwards;
    }
    @keyframes fadeInResult { to { opacity: 1; } }
    .result-score {
      font-size: 2.2rem; color: var(--blue); font-weight: bold;
      margin-bottom: 1.2rem;
      counter-reset: score 0;
      animation: countUp 1s linear;
    }
    @keyframes countUp {
      from { content: '0'; }
      to { content: '90'; }
    }
    .result-earth {
      width: 120px; height: 120px; margin: 0 auto 1.2rem auto;
      background: url('https://cdn-icons-png.flaticon.com/512/4149/4149676.png') center/contain no-repeat;
      filter: saturate(1);
      animation: rotateEarth 10s linear infinite;
    }
    @keyframes rotateEarth {
      100% { transform: rotate(360deg); }
    }
    .result-msg {
      font-size: 1.3rem; color: var(--green); margin-bottom: 2rem;
      opacity: 0; transform: translateY(20px);
      animation: floatUpMsg 0.7s 0.7s forwards;
    }
    @keyframes floatUpMsg { to { opacity: 1; transform: translateY(0); } }
    .result-btn {
      font-size: 1.1rem; font-weight: bold;
      background: var(--green); color: var(--white);
      border: none; border-radius: 32px; padding: 0.8em 2em;
      cursor: pointer; position: relative; overflow: hidden;
      box-shadow: 0 4px 16px #0002;
      transition: transform 0.2s;
    }
    .result-btn:hover { animation: ripple 0.6s; }
    .result-btn:active { transform: scale(0.97); }
    /* 반응형 디자인 */
    @media (max-width: 600px) {
      .start-title { font-size: 2rem; }
      .mission-cards { flex-direction: column; gap: 1.5rem; }
      .mission-card { width: 90vw; min-height: 120px; font-size: 1.1rem; }
      .quiz-wrap, .result-wrap { max-width: 98vw; padding: 1.2rem 0.5rem; }
      .action-wrap, .cleanup-wrap { max-width: 98vw; padding: 1.2rem 0.5rem; }
      .drop-zones { flex-direction: column; gap: 1.2rem; }
      .drop-zone { min-width: 90vw; min-height: 80px; font-size: 1rem; }
      .bins { flex-direction: column; gap: 1.2rem; }
      .bin { width: 90vw; height: 60px; font-size: 1rem; }
    }
  </style>
</head>
<body>
  <!-- 1. 시작 화면 -->
  <div class="start-bg"></div>
  <main class="center" id="start-screen">
    <h1 class="start-title">지구를 구하는 에코 챌린지</h1>
    <button class="start-btn">게임 시작</button>
  </main>
  <!-- 2. 미션 선택 화면 -->
  <div class="mission-bg" style="display:none;"></div>
  <main class="center" id="mission-screen" style="display:none;">
    <div class="rotating-icons">
      <img src="https://cdn-icons-png.flaticon.com/512/565/565547.png" style="top:10%;left:10%;width:60px;animation-delay:0s;">
      <img src="https://cdn-icons-png.flaticon.com/512/427/427735.png" style="top:60%;left:80%;width:80px;animation-delay:2s;">
      <img src="https://cdn-icons-png.flaticon.com/512/4149/4149676.png" style="top:30%;left:50%;width:70px;animation-delay:4s;">
      <img src="https://cdn-icons-png.flaticon.com/512/565/565547.png" style="top:70%;left:20%;width:50px;animation-delay:6s;">
    </div>
    <div class="mission-cards">
      <div class="mission-card card1" data-mission="quiz">퀴즈 미션</div>
      <div class="mission-card card2" data-mission="action">실천 미션</div>
      <div class="mission-card card3" data-mission="cleanup">청소 미션</div>
    </div>
  </main>
  <!-- 3. 퀴즈 미션 -->
  <main class="quiz-wrap" id="quiz-screen" style="display:none;">
    <div class="score">점수: <span id="score-quiz">0</span></div>
    <div class="quiz-q" id="quiz-question"></div>
    <form class="quiz-answers" id="quiz-answers"></form>
    <div class="tree-grow" id="quiz-tree" style="display:none;"></div>
    <div class="trash-pile" id="quiz-trash" style="display:none;"></div>
  </main>
  <!-- 4. 실천 미션 -->
  <main class="action-wrap" id="action-screen" style="display:none;">
    <div class="score">점수: <span id="score-action">0</span></div>
    <div class="action-items" id="action-items">
      <!-- JS로 아이템 생성 -->
    </div>
    <div class="drop-zones">
      <div class="drop-zone school" data-zone="학교">학교</div>
      <div class="drop-zone home" data-zone="집">집</div>
    </div>
  </main>
  <!-- 5. 청소 미션 -->
  <main class="cleanup-wrap" id="cleanup-screen" style="display:none;">
    <div class="score">점수: <span id="score-cleanup">0</span></div>
    <div class="trash-area" id="trash-area">
      <!-- JS로 쓰레기 생성 -->
    </div>
    <div class="bins">
      <div class="bin plastic" data-bin="플라스틱" style="background-image:url('https://cdn-icons-png.flaticon.com/512/565/565547.png');">플라스틱</div>
      <div class="bin paper" data-bin="종이" style="background-image:url('https://cdn-icons-png.flaticon.com/512/427/427735.png');">종이</div>
      <div class="bin general" data-bin="일반" style="background-image:url('https://cdn-icons-png.flaticon.com/512/4149/4149676.png');">일반</div>
    </div>
  </main>
  <!-- 6. 결과 화면 -->
  <main class="result-wrap" id="result-screen" style="display:none;">
    <div class="result-score">너의 에코 포인트: <span id="final-score">0</span>/90</div>
    <div class="result-earth" id="result-earth"></div>
    <div class="result-msg">너의 선택이 지구를 구했어!</div>
    <button class="result-btn">다시 플레이</button>
  </main>
  <script>
    // ----------------------
    // 게임 데이터
    // ----------------------
    const quizData = [
      {
        q: '종이컵 하나를 재활용하면 나무를 얼마나 보호할 수 있을까?',
        a: 0,
        options: ['약 0.01그루', '약 1그루', '약 10그루'],
        src: '한국환경공단, 자원순환 교육자료, 2023'
      },
      {
        q: '미세먼지를 줄이기 위해 자전거를 타면 1km당 얼마나 많은 이산화탄소를 줄일 수 있을까?',
        a: 1,
        options: ['약 50g', '약 250g', '약 1kg'],
        src: '환경부, 탄소중립 생활실천 가이드, 2024'
      },
      {
        q: '음식물 쓰레기를 1kg 줄이면 온실가스를 얼마나 줄일 수 있을까?',
        a: 2,
        options: ['약 0.5kg', '약 1kg', '약 2.5kg'],
        src: '농림축산식품부, 음식물 쓰레기 감축 캠페인, 2023'
      }
    ];
    const actionItems = [
      { text: '연습장 양면 사용', zone: '학교' },
      { text: '샤워 1분 줄이기', zone: '집' },
      { text: '페트병 라벨 제거', zone: '학교' },
      { text: '에코백 사용', zone: '집' },
      { text: '에어컨 1시간 덜 틀기', zone: '집' },
      { text: '음식물 쓰레기 줄이기', zone: '학교' }
    ];
    const trashItems = [
      { type: '플라스틱', img: 'https://cdn-icons-png.flaticon.com/512/565/565547.png' },
      { type: '플라스틱', img: 'https://cdn-icons-png.flaticon.com/512/565/565547.png' },
      { type: '플라스틱', img: 'https://cdn-icons-png.flaticon.com/512/565/565547.png' },
      { type: '플라스틱', img: 'https://cdn-icons-png.flaticon.com/512/565/565547.png' },
      { type: '플라스틱', img: 'https://cdn-icons-png.flaticon.com/512/565/565547.png' },
      { type: '종이', img: 'https://cdn-icons-png.flaticon.com/512/427/427735.png' },
      { type: '종이', img: 'https://cdn-icons-png.flaticon.com/512/427/427735.png' },
      { type: '종이', img: 'https://cdn-icons-png.flaticon.com/512/427/427735.png' },
      { type: '종이', img: 'https://cdn-icons-png.flaticon.com/512/427/427735.png' },
      { type: '일반', img: 'https://cdn-icons-png.flaticon.com/512/4149/4149676.png' }
    ];
    // ----------------------
    // 상태 변수
    // ----------------------
    let score = 0;
    let quizIdx = 0;
    let actionCorrect = 0;
    let cleanupCorrect = 0;
    // ----------------------
    // 화면 전환 함수
    // ----------------------
    function showScreen(id) {
      document.querySelectorAll('main, .mission-bg').forEach(e => e.style.display = 'none');
      if(id === 'mission-screen') document.querySelector('.mission-bg').style.display = '';
      document.getElementById(id).style.display = '';
    }
    // ----------------------
    // 시작 화면 → 미션 선택
    // ----------------------
    document.querySelector('.start-btn').onclick = function(e) {
      // 반짝이 효과
      for(let i=0; i<8; i++) {
        const s = document.createElement('span');
        s.className = 'sparkle';
        s.style.left = (50 + 30*Math.cos(i*Math.PI/4))+'px';
        s.style.top = (20 + 30*Math.sin(i*Math.PI/4))+'px';
        this.appendChild(s);
        setTimeout(()=>s.remove(), 700);
      }
      setTimeout(()=>showScreen('mission-screen'), 600);
    };
    // ----------------------
    // 미션 선택 → 각 미션
    // ----------------------
    document.querySelectorAll('.mission-card').forEach(card => {
      card.onclick = function() {
        this.style.transform = 'scale(1.2)';
        this.style.opacity = '0';
        setTimeout(()=>{
          if(this.dataset.mission==='quiz') startQuiz();
          if(this.dataset.mission==='action') startAction();
          if(this.dataset.mission==='cleanup') startCleanup();
        }, 400);
      };
    });
    // ----------------------
    // 퀴즈 미션
    // ----------------------
    function startQuiz() {
      showScreen('quiz-screen');
      score = 0; quizIdx = 0;
      document.getElementById('score-quiz').textContent = score;
      showQuizQ();
    }
    function showQuizQ() {
      const q = quizData[quizIdx];
      document.getElementById('quiz-question').textContent = `Q${quizIdx+1}. ${q.q}`;
      const answers = document.getElementById('quiz-answers');
      answers.innerHTML = '';
      q.options.forEach((opt, i) => {
        const label = document.createElement('label');
        const input = document.createElement('input');
        input.type = 'radio'; input.name = 'quiz';
        label.appendChild(input);
        const span = document.createElement('span');
        span.textContent = opt;
        label.appendChild(span);
        label.onclick = function() {
          if(input.checked) return;
          input.checked = true;
          if(i === q.a) {
            label.classList.add('correct');
            score += 10;
            document.getElementById('score-quiz').textContent = score;
            document.getElementById('quiz-tree').style.display = '';
            document.getElementById('quiz-tree').style.animation = 'treeGrow 0.5s';
            setTimeout(()=>{
              document.getElementById('quiz-tree').style.display = 'none';
              nextQuiz();
            }, 900);
          } else {
            label.classList.add('wrong');
            document.getElementById('quiz-trash').style.display = '';
            document.getElementById('quiz-trash').style.animation = 'trashAppear 0.5s';
            setTimeout(()=>{
              document.getElementById('quiz-trash').style.display = 'none';
              nextQuiz();
            }, 900);
          }
        };
        answers.appendChild(label);
      });
    }
    function nextQuiz() {
      quizIdx++;
      if(quizIdx < quizData.length) {
        showQuizQ();
      } else {
        setTimeout(()=>showScreen('mission-screen'), 500);
      }
    }
    // ----------------------
    // 실천 미션
    // ----------------------
    function startAction() {
      showScreen('action-screen');
      score = 0; actionCorrect = 0;
      document.getElementById('score-action').textContent = score;
      const items = document.getElementById('action-items');
      items.innerHTML = '';
      actionItems.forEach((item, idx) => {
        const div = document.createElement('div');
        div.className = 'action-item';
        div.textContent = item.text;
        div.draggable = true;
        div.ondragstart = e => {
          e.dataTransfer.setData('text/plain', idx);
        };
        items.appendChild(div);
      });
      document.querySelectorAll('.drop-zone').forEach(zone => {
        zone.ondragover = e => e.preventDefault();
        zone.ondrop = e => {
          e.preventDefault();
          const idx = e.dataTransfer.getData('text/plain');
          const item = actionItems[idx];
          if(zone.dataset.zone === item.zone) {
            items.children[idx].classList.add('sparkle');
            setTimeout(()=>{
              items.children[idx].style.display = 'none';
              score += 5; actionCorrect++;
              document.getElementById('score-action').textContent = score;
              if(actionCorrect === actionItems.length) setTimeout(()=>showScreen('mission-screen'), 700);
            }, 700);
          } else {
            items.children[idx].classList.add('bounce');
            setTimeout(()=>items.children[idx].classList.remove('bounce'), 500);
          }
        };
      });
    }
    // ----------------------
    // 청소 미션
    // ----------------------
    function startCleanup() {
      showScreen('cleanup-screen');
      score = 0; cleanupCorrect = 0;
      document.getElementById('score-cleanup').textContent = score;
      const area = document.getElementById('trash-area');
      area.innerHTML = '';
      trashItems.forEach((item, idx) => {
        const div = document.createElement('div');
        div.className = 'trash-item';
        div.style.backgroundImage = `url('${item.img}')`;
        div.draggable = true;
        div.ondragstart = e => {
          e.dataTransfer.setData('text/plain', idx);
        };
        area.appendChild(div);
      });
      document.querySelectorAll('.bin').forEach(bin => {
        bin.ondragover = e => e.preventDefault();
        bin.ondrop = e => {
          e.preventDefault();
          const idx = e.dataTransfer.getData('text/plain');
          const item = trashItems[idx];
          if(bin.dataset.bin === item.type) {
            bin.classList.add('flash');
            setTimeout(()=>bin.classList.remove('flash'), 500);
            area.children[idx].style.display = 'none';
            score += 3; cleanupCorrect++;
            document.getElementById('score-cleanup').textContent = score;
            if(cleanupCorrect === trashItems.length) setTimeout(()=>showResult(), 700);
          } else {
            bin.classList.add('shake');
            setTimeout(()=>bin.classList.remove('shake'), 500);
          }
        };
      });
    }
    // ----------------------
    // 결과 화면
    // ----------------------
    function showResult() {
      showScreen('result-screen');
      // 점수 합산
      const total = (Number(document.getElementById('score-quiz').textContent)||0)
                  + (Number(document.getElementById('score-action').textContent)||0)
                  + (Number(document.getElementById('score-cleanup').textContent)||0);
      document.getElementById('final-score').textContent = total;
      // 지구 이미지 색상
      const earth = document.getElementById('result-earth');
      earth.style.filter = `saturate(${Math.max(0.3, total/90)})`;
    }
    // ----------------------
    // 다시 플레이
    // ----------------------
    document.querySelector('.result-btn').onclick = function() {
      showScreen('start-screen');
    };
    // ----------------------
    // 최초 진입 시 시작 화면만 표시
    // ----------------------
    showScreen('start-screen');
  </script>
</body>
</html>

