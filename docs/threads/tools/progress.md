# Tiến Độ Học Tập

Theo dõi tiến trình học tiếng Anh của bạn. Dữ liệu được lưu trên trình duyệt của bạn.

<div id="progress-app">

  <div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(180px, 1fr)); gap:16px; margin:20px 0;">
    <div style="background:linear-gradient(135deg,#667eea 0%,#764ba2 100%); border-radius:12px; padding:20px; color:white; text-align:center;">
      <div style="font-size:14px; opacity:0.8;">Flashcard đã thuộc</div>
      <div id="stat-fc" style="font-size:36px; font-weight:700; margin:8px 0;">0</div>
      <div style="font-size:12px; opacity:0.7;">từ vựng</div>
    </div>
    <div style="background:linear-gradient(135deg,#f093fb 0%,#f5576c 100%); border-radius:12px; padding:20px; color:white; text-align:center;">
      <div style="font-size:14px; opacity:0.8;">Quiz đã làm</div>
      <div id="stat-quiz" style="font-size:36px; font-weight:700; margin:8px 0;">0</div>
      <div style="font-size:12px; opacity:0.7;">bài quiz</div>
    </div>
    <div style="background:linear-gradient(135deg,#38a169 0%,#2f855a 100%); border-radius:12px; padding:20px; color:white; text-align:center;">
      <div style="font-size:14px; opacity:0.8;">Ngữ pháp đã làm</div>
      <div id="stat-gram" style="font-size:36px; font-weight:700; margin:8px 0;">0</div>
      <div style="font-size:12px; opacity:0.7;">bài tập</div>
    </div>
    <div style="background:linear-gradient(135deg,#ed8936 0%,#dd6b20 100%); border-radius:12px; padding:20px; color:white; text-align:center;">
      <div style="font-size:14px; opacity:0.8;">Điểm trung bình</div>
      <div id="stat-avg" style="font-size:36px; font-weight:700; margin:8px 0;">-</div>
      <div style="font-size:12px; opacity:0.7;">phần trăm</div>
    </div>
  </div>

  <div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(300px, 1fr)); gap:20px; margin:20px 0;">
    
    <div style="background:#f7fafc; border-radius:12px; padding:20px;">
      <h3 style="margin-top:0; color:#2d3748;">Lịch sử Quiz</h3>
      <div id="quiz-history-list" style="max-height:300px; overflow-y:auto;"></div>
    </div>

    <div style="background:#f7fafc; border-radius:12px; padding:20px;">
      <h3 style="margin-top:0; color:#2d3748;">Lịch sử Ngữ pháp</h3>
      <div id="gram-history-list" style="max-height:300px; overflow-y:auto;"></div>
    </div>
  </div>

  <div style="background:#f7fafc; border-radius:12px; padding:20px; margin:20px 0;">
    <h3 style="margin-top:0; color:#2d3748;">Biểu đồ tiến độ</h3>
    <div id="progress-chart" style="display:flex; align-items:flex-end; gap:4px; height:200px; padding:10px 0; border-bottom:2px solid #e2e8f0;">
    </div>
    <div style="display:flex; justify-content:space-between; color:#a0aec0; font-size:12px; margin-top:4px;">
      <span>Cũ nhất</span><span>Mới nhất</span>
    </div>
  </div>

  <div style="background:#f7fafc; border-radius:12px; padding:20px; margin:20px 0;">
    <h3 style="margin-top:0; color:#2d3748;">Chuỗi ngày học (Streak)</h3>
    <div style="display:flex; align-items:center; gap:16px; margin:16px 0;">
      <div style="text-align:center;">
        <div id="streak-count" style="font-size:48px; font-weight:700; color:#ed8936;">0</div>
        <div style="color:#718096; font-size:14px;">ngày liên tiếp</div>
      </div>
      <div id="streak-calendar" style="display:flex; gap:4px; flex-wrap:wrap; flex:1;"></div>
    </div>
  </div>

  <div style="background:#fff5f5; border-radius:12px; padding:20px; margin:20px 0;">
    <h3 style="margin-top:0; color:#c53030;">Từ vựng cần ôn lại</h3>
    <p style="color:#718096; font-size:14px;">Những từ bạn đã trả lời sai trong quiz gần đây:</p>
    <div id="words-to-review" style="display:flex; flex-wrap:wrap; gap:8px;"></div>
  </div>

  <div style="text-align:center; margin:20px 0;">
    <button onclick="resetProgress()" style="padding:10px 24px; border:2px solid #fc8181; background:transparent; color:#e53e3e; border-radius:8px; cursor:pointer; font-size:14px;">Xóa tất cả dữ liệu</button>
  </div>
</div>

<script>
function loadProgress() {
  // Flashcard stats
  const fcKnown = JSON.parse(localStorage.getItem('fc-known') || '{}');
  const fcCount = Object.keys(fcKnown).length;
  document.getElementById('stat-fc').textContent = fcCount;

  // Quiz history
  const quizHistory = JSON.parse(localStorage.getItem('quiz-history') || '[]');
  document.getElementById('stat-quiz').textContent = quizHistory.length;
  
  const quizListEl = document.getElementById('quiz-history-list');
  if(quizHistory.length === 0) {
    quizListEl.innerHTML = '<div style="color:#a0aec0; text-align:center; padding:20px;">Chưa có dữ liệu quiz</div>';
  } else {
    quizListEl.innerHTML = quizHistory.slice().reverse().map(h => {
      const d = new Date(h.date);
      const pct = Math.round(h.score/h.total*100);
      const color = pct >= 70 ? '#38a169' : pct >= 50 ? '#ed8936' : '#e53e3e';
      return `<div style="display:flex; justify-content:space-between; align-items:center; padding:8px 12px; border-bottom:1px solid #e2e8f0;">
        <span style="color:#718096; font-size:13px;">${d.toLocaleDateString('vi-VN')} ${d.toLocaleTimeString('vi-VN',{hour:'2-digit',minute:'2-digit'})}</span>
        <span style="color:${color}; font-weight:600;">${h.score}/${h.total} (${pct}%)</span>
      </div>`;
    }).join('');
  }

  // Grammar history
  const gramHistory = JSON.parse(localStorage.getItem('gram-history') || '[]');
  document.getElementById('stat-gram').textContent = gramHistory.length;
  
  const topicNames = {tenses:'Thì',prepositions:'Giới từ',articles:'Mạo từ',conditionals:'Câu điều kiện',passive:'Bị động',relative:'Mệnh đề QH'};
  const gramListEl = document.getElementById('gram-history-list');
  if(gramHistory.length === 0) {
    gramListEl.innerHTML = '<div style="color:#a0aec0; text-align:center; padding:20px;">Chưa có dữ liệu ngữ pháp</div>';
  } else {
    gramListEl.innerHTML = gramHistory.slice().reverse().map(h => {
      const d = new Date(h.date);
      const pct = Math.round(h.score/h.total*100);
      const color = pct >= 70 ? '#38a169' : pct >= 50 ? '#ed8936' : '#e53e3e';
      return `<div style="display:flex; justify-content:space-between; align-items:center; padding:8px 12px; border-bottom:1px solid #e2e8f0;">
        <div><span style="color:#718096; font-size:13px;">${d.toLocaleDateString('vi-VN')}</span> <span style="background:#edf2f7; padding:2px 8px; border-radius:4px; font-size:12px;">${topicNames[h.topic]||h.topic}</span></div>
        <span style="color:${color}; font-weight:600;">${h.score}/${h.total} (${pct}%)</span>
      </div>`;
    }).join('');
  }

  // Average score
  const allScores = [...quizHistory, ...gramHistory];
  if(allScores.length > 0) {
    const avg = Math.round(allScores.reduce((s,h) => s + h.score/h.total*100, 0) / allScores.length);
    document.getElementById('stat-avg').textContent = avg + '%';
  }

  // Progress chart
  const chartEl = document.getElementById('progress-chart');
  const allEntries = [...quizHistory.map(h=>({...h,type:'quiz'})), ...gramHistory.map(h=>({...h,type:'gram'}))]
    .sort((a,b) => new Date(a.date) - new Date(b.date)).slice(-20);
  
  if(allEntries.length === 0) {
    chartEl.innerHTML = '<div style="color:#a0aec0; text-align:center; width:100%; display:flex; align-items:center; justify-content:center;">Hoàn thành quiz hoặc bài tập ngữ pháp để xem biểu đồ</div>';
  } else {
    chartEl.innerHTML = allEntries.map(h => {
      const pct = Math.round(h.score/h.total*100);
      const color = h.type === 'quiz' ? '#667eea' : '#38a169';
      return `<div style="flex:1; min-width:20px; max-width:40px; display:flex; flex-direction:column; align-items:center; gap:4px;">
        <span style="font-size:11px; color:#718096;">${pct}%</span>
        <div style="width:100%; background:${color}; border-radius:4px 4px 0 0; height:${Math.max(pct*1.8, 10)}px; transition:height 0.3s;" title="${h.type === 'quiz' ? 'Quiz' : 'Ngữ pháp'}: ${pct}%"></div>
      </div>`;
    }).join('');
  }

  // Streak
  const streakDates = new Set();
  allScores.forEach(h => streakDates.add(new Date(h.date).toDateString()));
  
  let streak = 0;
  const today = new Date();
  for(let i = 0; i < 365; i++) {
    const d = new Date(today);
    d.setDate(d.getDate() - i);
    if(streakDates.has(d.toDateString())) streak++;
    else if(i > 0) break;
  }
  document.getElementById('streak-count').textContent = streak;

  // Calendar (last 30 days)
  const calEl = document.getElementById('streak-calendar');
  let calHTML = '';
  for(let i = 29; i >= 0; i--) {
    const d = new Date(today);
    d.setDate(d.getDate() - i);
    const active = streakDates.has(d.toDateString());
    calHTML += `<div style="width:20px; height:20px; border-radius:4px; background:${active ? '#48bb78' : '#e2e8f0'};" title="${d.toLocaleDateString('vi-VN')}${active ? ' - Đã học' : ''}"></div>`;
  }
  calEl.innerHTML = calHTML;

  // Words to review (from wrong quiz answers)
  const wordsEl = document.getElementById('words-to-review');
  const wrongWords = JSON.parse(localStorage.getItem('quiz-wrong') || '[]');
  if(wrongWords.length === 0) {
    wordsEl.innerHTML = '<span style="color:#a0aec0;">Chưa có từ nào cần ôn!</span>';
  } else {
    wordsEl.innerHTML = wrongWords.slice(-15).map(w => 
      `<span style="background:#fed7d7; color:#c53030; padding:4px 12px; border-radius:16px; font-size:14px;">${w}</span>`
    ).join('');
  }
}

function resetProgress() {
  if(confirm('Bạn có chắc muốn xóa tất cả dữ liệu học tập? Hành động này không thể hoàn tác.')) {
    localStorage.removeItem('fc-known');
    localStorage.removeItem('quiz-history');
    localStorage.removeItem('gram-history');
    localStorage.removeItem('quiz-wrong');
    loadProgress();
  }
}

if(document.getElementById('progress-app')) loadProgress();
</script>
