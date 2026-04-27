# Quiz Từ Vựng

Kiểm tra kiến thức từ vựng với bài quiz trắc nghiệm. Chọn đáp án đúng cho mỗi câu hỏi.

<div id="quiz-app">
  <div style="text-align:center; margin:20px 0;">
    <label for="quiz-mode" style="font-weight:600;">Chế độ: </label>
    <select id="quiz-mode" style="padding:8px 16px; border-radius:8px; border:1px solid #ddd; font-size:15px;">
      <option value="en-vi">Anh → Việt</option>
      <option value="vi-en">Việt → Anh</option>
    </select>
    <select id="quiz-cat" style="padding:8px 16px; border-radius:8px; border:1px solid #ddd; font-size:15px; margin-left:8px;">
      <option value="all">Tất cả chủ đề</option>
      <option value="common">Thông dụng</option>
      <option value="tech">Công nghệ</option>
      <option value="ielts">IELTS/TOEFL</option>
      <option value="business">Kinh doanh</option>
      <option value="daily">Giao tiếp</option>
    </select>
    <button onclick="startQuiz()" style="padding:10px 24px; border:none; background:#667eea; color:white; border-radius:8px; cursor:pointer; font-size:15px; margin-left:8px;">Bắt đầu Quiz</button>
  </div>

  <div id="quiz-area" style="max-width:600px; margin:0 auto; display:none;">
    <div style="background:#f7fafc; border-radius:12px; padding:24px; box-shadow:0 2px 10px rgba(0,0,0,0.1);">
      <div id="quiz-counter" style="text-align:right; color:#a0aec0; font-size:14px; margin-bottom:8px;"></div>
      <div id="quiz-question" style="font-size:22px; font-weight:600; text-align:center; margin-bottom:24px; color:#2d3748;"></div>
      <div id="quiz-options" style="display:flex; flex-direction:column; gap:10px;"></div>
      <div id="quiz-feedback" style="text-align:center; margin-top:16px; font-size:16px; min-height:24px;"></div>
      <div style="text-align:center; margin-top:16px;">
        <button id="quiz-next" onclick="nextQuestion()" style="padding:10px 24px; border:none; background:#667eea; color:white; border-radius:8px; cursor:pointer; font-size:15px; display:none;">Câu tiếp →</button>
      </div>
    </div>
  </div>

  <div id="quiz-result" style="max-width:600px; margin:0 auto; display:none;">
    <div style="background:linear-gradient(135deg,#667eea 0%,#764ba2 100%); border-radius:16px; padding:40px; text-align:center; color:white;">
      <div style="font-size:48px; margin-bottom:16px;">🎉</div>
      <div style="font-size:28px; font-weight:700; margin-bottom:8px;">Hoàn thành!</div>
      <div id="quiz-score" style="font-size:20px; margin-bottom:8px;"></div>
      <div id="quiz-grade" style="font-size:18px; opacity:0.9; margin-bottom:20px;"></div>
      <button onclick="startQuiz()" style="padding:12px 32px; border:none; background:white; color:#667eea; border-radius:8px; cursor:pointer; font-size:16px; font-weight:600;">Làm lại</button>
    </div>
    <div id="quiz-review" style="margin-top:20px;"></div>
  </div>
</div>

<script>
const quizVocab = {
  common: [
    {en:"accomplish",vi:"Hoàn thành, đạt được"},{en:"beneficial",vi:"Có lợi, có ích"},{en:"comprehend",vi:"Hiểu, lĩnh hội"},
    {en:"diligent",vi:"Siêng năng, cần cù"},{en:"elaborate",vi:"Chi tiết, công phu"},{en:"fluctuate",vi:"Dao động, biến đổi"},
    {en:"genuine",vi:"Chân thật, thật sự"},{en:"hesitate",vi:"Do dự, lưỡng lự"},{en:"inevitable",vi:"Không thể tránh khỏi"},
    {en:"justify",vi:"Biện minh, chứng minh"},{en:"meticulous",vi:"Tỉ mỉ, cẩn thận"},{en:"negotiate",vi:"Đàm phán, thương lượng"},
    {en:"persistent",vi:"Kiên trì, bền bỉ"},{en:"reluctant",vi:"Miễn cưỡng"},{en:"substantial",vi:"Đáng kể, quan trọng"},
  ],
  tech: [
    {en:"algorithm",vi:"Thuật toán"},{en:"bandwidth",vi:"Băng thông"},{en:"compile",vi:"Biên dịch"},
    {en:"debug",vi:"Gỡ lỗi"},{en:"encrypt",vi:"Mã hóa"},{en:"framework",vi:"Khung, nền tảng"},
    {en:"repository",vi:"Kho lưu trữ"},{en:"deploy",vi:"Triển khai"},{en:"iterate",vi:"Lặp lại, duyệt"},
    {en:"scalable",vi:"Có khả năng mở rộng"},{en:"refactor",vi:"Tái cấu trúc code"},{en:"middleware",vi:"Phần mềm trung gian"},
  ],
  ielts: [
    {en:"alleviate",vi:"Giảm bớt, làm dịu"},{en:"contemporary",vi:"Đương đại, hiện đại"},{en:"deteriorate",vi:"Xấu đi, suy thoái"},
    {en:"exacerbate",vi:"Làm trầm trọng thêm"},{en:"predominant",vi:"Chiếm ưu thế"},{en:"sustainable",vi:"Bền vững"},
    {en:"unprecedented",vi:"Chưa từng có tiền lệ"},{en:"versatile",vi:"Đa năng, linh hoạt"},{en:"paradigm",vi:"Mô hình, khuôn mẫu"},
  ],
  business: [
    {en:"acquisition",vi:"Sự mua lại"},{en:"benchmark",vi:"Tiêu chuẩn"},{en:"leverage",vi:"Tận dụng, đòn bẩy"},
    {en:"stakeholder",vi:"Bên liên quan"},{en:"revenue",vi:"Doanh thu"},{en:"outsource",vi:"Thuê ngoài"},
  ],
  daily: [
    {en:"appreciate",vi:"Đánh giá cao, trân trọng"},{en:"awkward",vi:"Lúng túng, ngượng ngùng"},{en:"grab",vi:"Nắm lấy, chộp"},
    {en:"figure out",vi:"Tìm ra, hiểu ra"},{en:"overwhelmed",vi:"Choáng ngợp"},{en:"procrastinate",vi:"Trì hoãn, chần chừ"},
    {en:"exhausted",vi:"Kiệt sức"},{en:"commute",vi:"Đi lại (làm việc)"},
  ]
};

let quizQuestions = [];
let quizIndex = 0;
let quizScore = 0;
let quizAnswered = false;
let quizWrong = [];

function shuffle(arr) {
  const a = [...arr];
  for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}
  return a;
}

function startQuiz() {
  const cat = document.getElementById('quiz-cat').value;
  let pool = [];
  if(cat === 'all') {
    Object.values(quizVocab).forEach(v => pool.push(...v));
  } else {
    pool = quizVocab[cat] || [];
  }
  quizQuestions = shuffle(pool).slice(0, 10);
  quizIndex = 0;
  quizScore = 0;
  quizWrong = [];
  document.getElementById('quiz-area').style.display = 'block';
  document.getElementById('quiz-result').style.display = 'none';
  showQuestion();
}

function showQuestion() {
  if(quizIndex >= quizQuestions.length) { showResult(); return; }
  const mode = document.getElementById('quiz-mode').value;
  const q = quizQuestions[quizIndex];
  const qEl = document.getElementById('quiz-question');
  const counter = document.getElementById('quiz-counter');
  const optsEl = document.getElementById('quiz-options');
  const fb = document.getElementById('quiz-feedback');
  const nextBtn = document.getElementById('quiz-next');
  
  counter.textContent = `Câu ${quizIndex+1}/${quizQuestions.length}`;
  qEl.textContent = mode === 'en-vi' ? q.en : q.vi;
  fb.textContent = '';
  nextBtn.style.display = 'none';
  quizAnswered = false;

  let allPool = [];
  Object.values(quizVocab).forEach(v => allPool.push(...v));
  
  const correctAns = mode === 'en-vi' ? q.vi : q.en;
  const field = mode === 'en-vi' ? 'vi' : 'en';
  let options = [correctAns];
  const others = shuffle(allPool.filter(w => w[field] !== correctAns)).slice(0, 3).map(w => w[field]);
  options.push(...others);
  options = shuffle(options);

  optsEl.innerHTML = '';
  options.forEach(opt => {
    const btn = document.createElement('button');
    btn.textContent = opt;
    btn.style.cssText = 'padding:12px 16px; border:2px solid #e2e8f0; background:white; border-radius:10px; cursor:pointer; font-size:15px; text-align:left; transition:all 0.2s;';
    btn.onmouseover = () => { if(!quizAnswered) btn.style.borderColor = '#667eea'; };
    btn.onmouseout = () => { if(!quizAnswered) btn.style.borderColor = '#e2e8f0'; };
    btn.onclick = () => checkAnswer(opt, correctAns, btn);
    optsEl.appendChild(btn);
  });
}

function checkAnswer(selected, correct, btn) {
  if(quizAnswered) return;
  quizAnswered = true;
  const fb = document.getElementById('quiz-feedback');
  const nextBtn = document.getElementById('quiz-next');
  const optsEl = document.getElementById('quiz-options');
  
  Array.from(optsEl.children).forEach(b => {
    if(b.textContent === correct) { b.style.borderColor = '#48bb78'; b.style.background = '#f0fff4'; }
  });

  if(selected === correct) {
    quizScore++;
    btn.style.borderColor = '#48bb78';
    btn.style.background = '#f0fff4';
    fb.innerHTML = '<span style="color:#48bb78; font-weight:600;">Chính xác! ✓</span>';
  } else {
    btn.style.borderColor = '#fc8181';
    btn.style.background = '#fff5f5';
    fb.innerHTML = '<span style="color:#e53e3e; font-weight:600;">Sai rồi ✗</span>';
    quizWrong.push(quizQuestions[quizIndex]);
  }
  nextBtn.style.display = 'inline-block';
}

function nextQuestion() {
  quizIndex++;
  showQuestion();
}

function showResult() {
  document.getElementById('quiz-area').style.display = 'none';
  document.getElementById('quiz-result').style.display = 'block';
  const pct = Math.round(quizScore / quizQuestions.length * 100);
  document.getElementById('quiz-score').textContent = `Điểm: ${quizScore}/${quizQuestions.length} (${pct}%)`;
  
  let grade = '';
  if(pct >= 90) grade = 'Xuất sắc! Bạn nắm rất vững từ vựng!';
  else if(pct >= 70) grade = 'Tốt! Tiếp tục luyện tập thêm nhé!';
  else if(pct >= 50) grade = 'Khá! Cần ôn lại một số từ.';
  else grade = 'Cần cố gắng hơn! Hãy ôn lại flashcard.';
  document.getElementById('quiz-grade').textContent = grade;

  // Save score
  const history = JSON.parse(localStorage.getItem('quiz-history') || '[]');
  history.push({date: new Date().toISOString(), score: quizScore, total: quizQuestions.length});
  localStorage.setItem('quiz-history', JSON.stringify(history.slice(-20)));

  // Show wrong answers
  const reviewEl = document.getElementById('quiz-review');
  if(quizWrong.length > 0) {
    reviewEl.innerHTML = '<h3 style="margin-top:16px;">Từ cần ôn lại:</h3>' +
      quizWrong.map(w => `<div style="padding:10px 16px; background:#fff5f5; border-radius:8px; margin:6px 0; border-left:4px solid #fc8181;"><strong>${w.en}</strong> — ${w.vi}</div>`).join('');
  } else {
    reviewEl.innerHTML = '';
  }
}
</script>
