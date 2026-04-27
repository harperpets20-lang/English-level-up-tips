# Bài Tập Ngữ Pháp

Luyện ngữ pháp tiếng Anh qua các bài tập tương tác. Chọn đáp án đúng hoặc điền từ phù hợp.

<div id="grammar-app">
  <div style="text-align:center; margin:20px 0;">
    <label for="gram-topic" style="font-weight:600;">Chủ đề: </label>
    <select id="gram-topic" onchange="loadGrammar()" style="padding:8px 16px; border-radius:8px; border:1px solid #ddd; font-size:15px;">
      <option value="tenses">Thì (Tenses)</option>
      <option value="prepositions">Giới từ (Prepositions)</option>
      <option value="articles">Mạo từ (Articles)</option>
      <option value="conditionals">Câu điều kiện (Conditionals)</option>
      <option value="passive">Bị động (Passive Voice)</option>
      <option value="relative">Mệnh đề quan hệ (Relative Clauses)</option>
    </select>
    <button onclick="startGrammar()" style="padding:10px 24px; border:none; background:#38a169; color:white; border-radius:8px; cursor:pointer; font-size:15px; margin-left:8px;">Bắt đầu</button>
  </div>

  <div id="gram-intro" style="max-width:650px; margin:0 auto; background:#f7fafc; border-radius:12px; padding:24px;">
    <h3 style="margin-top:0; color:#2d3748;">Hướng dẫn</h3>
    <p style="color:#4a5568; line-height:1.8;">Mỗi bài tập có 8-10 câu hỏi. Chọn đáp án đúng và xem giải thích ngữ pháp chi tiết. Hoàn thành bài tập để xem điểm tổng kết và các lỗi cần ôn lại.</p>
  </div>

  <div id="gram-area" style="max-width:650px; margin:0 auto; display:none;">
    <div style="background:#f7fafc; border-radius:12px; padding:24px; box-shadow:0 2px 10px rgba(0,0,0,0.08);">
      <div id="gram-counter" style="text-align:right; color:#a0aec0; font-size:14px; margin-bottom:8px;"></div>
      <div id="gram-rule" style="background:#ebf8ff; border-left:4px solid #4299e1; padding:12px 16px; border-radius:0 8px 8px 0; margin-bottom:16px; font-size:14px; color:#2b6cb0; line-height:1.6;"></div>
      <div id="gram-question" style="font-size:18px; font-weight:500; margin-bottom:20px; color:#2d3748; line-height:1.6;"></div>
      <div id="gram-options" style="display:flex; flex-direction:column; gap:10px;"></div>
      <div id="gram-feedback" style="margin-top:16px; min-height:60px;"></div>
      <div style="text-align:center; margin-top:16px;">
        <button id="gram-next" onclick="nextGram()" style="padding:10px 24px; border:none; background:#38a169; color:white; border-radius:8px; cursor:pointer; font-size:15px; display:none;">Câu tiếp →</button>
      </div>
    </div>
  </div>

  <div id="gram-result" style="max-width:650px; margin:0 auto; display:none;">
    <div style="background:linear-gradient(135deg,#38a169 0%,#2f855a 100%); border-radius:16px; padding:40px; text-align:center; color:white;">
      <div style="font-size:28px; font-weight:700; margin-bottom:8px;">Kết quả</div>
      <div id="gram-score" style="font-size:20px; margin-bottom:8px;"></div>
      <div id="gram-grade" style="font-size:16px; opacity:0.9; margin-bottom:20px;"></div>
      <button onclick="startGrammar()" style="padding:12px 32px; border:none; background:white; color:#38a169; border-radius:8px; cursor:pointer; font-size:16px; font-weight:600;">Làm lại</button>
    </div>
    <div id="gram-review" style="margin-top:20px;"></div>
  </div>
</div>

<script>
const grammarData = {
  tenses: {
    rule: "Thì trong tiếng Anh: Simple (đơn giản), Continuous (tiếp diễn), Perfect (hoàn thành). Chú ý dấu hiệu thời gian để chọn đúng thì.",
    questions: [
      {q:"She ___ to school every day.", opts:["goes","is going","has gone","went"], ans:0, explain:"Dùng Simple Present cho thói quen hàng ngày. Dấu hiệu: 'every day'."},
      {q:"Look! The children ___ in the park.", opts:["play","are playing","played","have played"], ans:1, explain:"Dùng Present Continuous cho hành động đang xảy ra. Dấu hiệu: 'Look!'"},
      {q:"I ___ this book since last week.", opts:["read","am reading","have been reading","was reading"], ans:2, explain:"Dùng Present Perfect Continuous cho hành động bắt đầu quá khứ, kéo dài đến hiện tại. 'since last week'."},
      {q:"They ___ to Japan last summer.", opts:["go","are going","have gone","went"], ans:3, explain:"Dùng Simple Past cho hành động đã hoàn thành trong quá khứ. 'last summer'."},
      {q:"By next year, she ___ her degree.", opts:["finishes","will finish","will have finished","is finishing"], ans:2, explain:"Dùng Future Perfect cho hành động sẽ hoàn thành trước một thời điểm tương lai. 'By next year'."},
      {q:"While I ___ dinner, the phone rang.", opts:["cook","was cooking","cooked","have cooked"], ans:1, explain:"Dùng Past Continuous cho hành động đang xảy ra khi bị gián đoạn. 'While... rang'."},
      {q:"He ___ here for 10 years.", opts:["works","is working","has worked","worked"], ans:2, explain:"Dùng Present Perfect cho hành động kéo dài đến hiện tại. 'for 10 years'."},
      {q:"I ___ my homework before you arrived.", opts:["finish","finished","had finished","have finished"], ans:2, explain:"Dùng Past Perfect cho hành động xảy ra trước hành động khác trong quá khứ."},
    ]
  },
  prepositions: {
    rule: "Giới từ chỉ thời gian: at (giờ), on (ngày), in (tháng/năm). Giới từ chỉ nơi: at (điểm), on (bề mặt), in (bên trong).",
    questions: [
      {q:"The meeting is ___ Monday.", opts:["in","on","at","by"], ans:1, explain:"Dùng 'on' cho ngày trong tuần: on Monday, on Friday."},
      {q:"She was born ___ 1995.", opts:["in","on","at","during"], ans:0, explain:"Dùng 'in' cho năm, tháng, mùa: in 1995, in May, in summer."},
      {q:"The class starts ___ 9 o'clock.", opts:["in","on","at","by"], ans:2, explain:"Dùng 'at' cho giờ cụ thể: at 9 o'clock, at noon."},
      {q:"The book is ___ the table.", opts:["in","on","at","under"], ans:1, explain:"Dùng 'on' cho bề mặt tiếp xúc: on the table, on the wall."},
      {q:"She lives ___ 42 Oak Street.", opts:["in","on","at","by"], ans:2, explain:"Dùng 'at' cho địa chỉ cụ thể: at 42 Oak Street."},
      {q:"We arrived ___ the airport early.", opts:["in","on","at","to"], ans:2, explain:"Dùng 'at' cho điểm đến cụ thể: at the airport, at the station."},
      {q:"The cat is hiding ___ the bed.", opts:["on","in","under","at"], ans:2, explain:"'Under' = bên dưới. The cat is under the bed."},
      {q:"I'm interested ___ learning English.", opts:["at","on","in","for"], ans:2, explain:"Cụm cố định: interested IN + V-ing."},
    ]
  },
  articles: {
    rule: "a/an: không xác định, lần đầu nhắc đến. the: xác định, đã biết. Không dùng mạo từ cho danh từ chung/trừu tượng.",
    questions: [
      {q:"I saw ___ elephant at the zoo.", opts:["a","an","the","(no article)"], ans:1, explain:"Dùng 'an' trước nguyên âm. 'elephant' bắt đầu bằng /e/."},
      {q:"___ sun rises in the east.", opts:["A","An","The","(no article)"], ans:2, explain:"Dùng 'the' cho vật duy nhất: the sun, the moon, the earth."},
      {q:"She is ___ best student in class.", opts:["a","an","the","(no article)"], ans:2, explain:"Dùng 'the' với so sánh nhất (superlative): the best, the tallest."},
      {q:"___ water is essential for life.", opts:["A","An","The","(no article)"], ans:3, explain:"Không dùng mạo từ cho danh từ không đếm được khi nói chung."},
      {q:"I need ___ umbrella.", opts:["a","an","the","(no article)"], ans:1, explain:"Dùng 'an' trước âm nguyên âm: an umbrella (/ʌm/)."},
      {q:"He plays ___ guitar very well.", opts:["a","an","the","(no article)"], ans:2, explain:"Dùng 'the' với nhạc cụ: play the guitar, the piano."},
      {q:"___ honesty is the best policy.", opts:["A","An","The","(no article)"], ans:3, explain:"Không dùng mạo từ cho danh từ trừu tượng khi nói chung: honesty, love, happiness."},
      {q:"Can you pass me ___ salt?", opts:["a","an","the","(no article)"], ans:2, explain:"Dùng 'the' khi cả hai đều biết vật đang nói đến (lọ muối trên bàn)."},
    ]
  },
  conditionals: {
    rule: "If loại 0: hiện tại + hiện tại (sự thật). Loại 1: hiện tại + will (có thể xảy ra). Loại 2: quá khứ + would (giả định). Loại 3: quá khứ hoàn thành + would have (không thể thay đổi).",
    questions: [
      {q:"If you heat ice, it ___.", opts:["melts","will melt","would melt","melted"], ans:0, explain:"Câu điều kiện loại 0 (sự thật hiển nhiên): If + present, present."},
      {q:"If it rains, I ___ an umbrella.", opts:["take","will take","would take","took"], ans:1, explain:"Câu điều kiện loại 1 (có thể xảy ra): If + present, will + V."},
      {q:"If I ___ rich, I would travel the world.", opts:["am","was/were","had been","will be"], ans:1, explain:"Câu điều kiện loại 2 (giả định hiện tại): If + past, would + V."},
      {q:"If she had studied, she ___ the exam.", opts:["passes","will pass","would pass","would have passed"], ans:3, explain:"Câu điều kiện loại 3 (không thể thay đổi): If + past perfect, would have + V3."},
      {q:"I would help you if I ___ more time.", opts:["have","had","have had","will have"], ans:1, explain:"Loại 2: If + past simple. 'Had' là quá khứ giả định."},
      {q:"If I ___ you, I would apologize.", opts:["am","was/were","had been","will be"], ans:1, explain:"'If I were you' — câu khuyên nhủ giả định (loại 2). 'Were' dùng cho tất cả ngôi."},
      {q:"Unless you hurry, you ___ late.", opts:["are","will be","would be","were"], ans:1, explain:"Unless = If not. Đây là loại 1: Unless + present, will + V."},
      {q:"If they had left earlier, they ___ the train.", opts:["catch","will catch","would catch","would have caught"], ans:3, explain:"Loại 3 (hối tiếc quá khứ): If + had V3, would have V3."},
    ]
  },
  passive: {
    rule: "Bị động: S + be + V3 (past participle). Active: 'Tom built the house.' → Passive: 'The house was built by Tom.'",
    questions: [
      {q:"The cake ___ by my mother.", opts:["baked","was baked","is baking","bakes"], ans:1, explain:"Passive Past Simple: was/were + V3. 'Baked' là V3 của 'bake'."},
      {q:"English ___ in many countries.", opts:["speaks","is speaking","is spoken","has speaking"], ans:2, explain:"Passive Present Simple: is/are + V3. 'Spoken' là V3 của 'speak'."},
      {q:"The letter ___ yesterday.", opts:["sent","was sent","is sent","sends"], ans:1, explain:"Passive Past Simple: was + V3. 'Sent' là V3 của 'send'. 'Yesterday' = quá khứ."},
      {q:"A new hospital ___ next year.", opts:["will build","will be built","is building","builds"], ans:1, explain:"Passive Future: will be + V3."},
      {q:"The project ___ already ___.", opts:["has/completed","has/been completed","is/completing","was/completing"], ans:1, explain:"Passive Present Perfect: has/have been + V3."},
      {q:"The window ___ by the children.", opts:["broke","was broken","is breaking","breaks"], ans:1, explain:"Passive Past Simple: was + V3. 'Broken' là V3 của 'break'."},
      {q:"This song ___ by millions of people.", opts:["loves","is loved","is loving","has loving"], ans:1, explain:"Passive Present Simple: is + V3. 'Loved' là V3 của 'love'."},
      {q:"The report ___ right now.", opts:["writes","is written","is being written","was writing"], ans:2, explain:"Passive Present Continuous: is/are being + V3. 'Right now' = đang xảy ra."},
    ]
  },
  relative: {
    rule: "Who: người. Which: vật. That: người/vật. Where: nơi chốn. When: thời gian. Whose: sở hữu.",
    questions: [
      {q:"The man ___ lives next door is a doctor.", opts:["which","who","where","whose"], ans:1, explain:"'Who' thay thế cho người làm chủ ngữ: The man WHO lives..."},
      {q:"The book ___ I bought is interesting.", opts:["who","which","where","whose"], ans:1, explain:"'Which' thay thế cho vật làm tân ngữ: The book WHICH I bought..."},
      {q:"That's the restaurant ___ we had dinner.", opts:["which","who","where","when"], ans:2, explain:"'Where' thay thế cho nơi chốn: the restaurant WHERE we had dinner."},
      {q:"She's the woman ___ car was stolen.", opts:["who","which","where","whose"], ans:3, explain:"'Whose' chỉ sở hữu: the woman WHOSE car = xe của bà ấy."},
      {q:"2020 was the year ___ everything changed.", opts:["which","who","where","when"], ans:3, explain:"'When' thay thế cho thời gian: the year WHEN everything changed."},
      {q:"The students ___ passed the exam are happy.", opts:["who","which","where","whose"], ans:0, explain:"'Who' cho người: The students WHO passed..."},
      {q:"Is this the reason ___ you left?", opts:["which","who","why","when"], ans:2, explain:"'Why' dùng sau 'reason': the reason WHY you left."},
      {q:"The house ___ was built in 1900 is beautiful.", opts:["who","which","where","whose"], ans:1, explain:"'Which' cho vật: The house WHICH was built..."},
    ]
  }
};

let gramTopic = 'tenses';
let gramQuestions = [];
let gramIndex = 0;
let gramScoreVal = 0;
let gramAnswered = false;
let gramWrong = [];

function loadGrammar() {
  gramTopic = document.getElementById('gram-topic').value;
}

function startGrammar() {
  loadGrammar();
  const data = grammarData[gramTopic];
  gramQuestions = [...data.questions].sort(() => Math.random() - 0.5);
  gramIndex = 0;
  gramScoreVal = 0;
  gramWrong = [];
  gramAnswered = false;
  document.getElementById('gram-intro').style.display = 'none';
  document.getElementById('gram-area').style.display = 'block';
  document.getElementById('gram-result').style.display = 'none';
  showGramQ();
}

function showGramQ() {
  if(gramIndex >= gramQuestions.length) { showGramResult(); return; }
  const data = grammarData[gramTopic];
  const q = gramQuestions[gramIndex];
  document.getElementById('gram-counter').textContent = `Câu ${gramIndex+1}/${gramQuestions.length}`;
  document.getElementById('gram-rule').textContent = data.rule;
  document.getElementById('gram-question').textContent = q.q;
  document.getElementById('gram-feedback').innerHTML = '';
  document.getElementById('gram-next').style.display = 'none';
  gramAnswered = false;

  const optsEl = document.getElementById('gram-options');
  optsEl.innerHTML = '';
  q.opts.forEach((opt, i) => {
    const btn = document.createElement('button');
    btn.textContent = opt;
    btn.style.cssText = 'padding:12px 16px; border:2px solid #e2e8f0; background:white; border-radius:10px; cursor:pointer; font-size:15px; text-align:left; transition:all 0.2s;';
    btn.onmouseover = () => { if(!gramAnswered) btn.style.borderColor = '#38a169'; };
    btn.onmouseout = () => { if(!gramAnswered) btn.style.borderColor = '#e2e8f0'; };
    btn.onclick = () => checkGram(i, btn);
    optsEl.appendChild(btn);
  });
}

function checkGram(selected, btn) {
  if(gramAnswered) return;
  gramAnswered = true;
  const q = gramQuestions[gramIndex];
  const optsEl = document.getElementById('gram-options');
  
  Array.from(optsEl.children).forEach((b, i) => {
    if(i === q.ans) { b.style.borderColor = '#38a169'; b.style.background = '#f0fff4'; }
  });

  let fbHTML = '';
  if(selected === q.ans) {
    gramScoreVal++;
    btn.style.borderColor = '#38a169'; btn.style.background = '#f0fff4';
    fbHTML = `<div style="color:#38a169; font-weight:600; margin-bottom:6px;">Chính xác!</div>`;
  } else {
    btn.style.borderColor = '#fc8181'; btn.style.background = '#fff5f5';
    fbHTML = `<div style="color:#e53e3e; font-weight:600; margin-bottom:6px;">Sai rồi!</div>`;
    gramWrong.push(q);
  }
  fbHTML += `<div style="color:#4a5568; font-size:14px; line-height:1.6;">${q.explain}</div>`;
  document.getElementById('gram-feedback').innerHTML = fbHTML;
  document.getElementById('gram-next').style.display = 'inline-block';
}

function nextGram() {
  gramIndex++;
  showGramQ();
}

function showGramResult() {
  document.getElementById('gram-area').style.display = 'none';
  document.getElementById('gram-result').style.display = 'block';
  const pct = Math.round(gramScoreVal / gramQuestions.length * 100);
  document.getElementById('gram-score').textContent = `Điểm: ${gramScoreVal}/${gramQuestions.length} (${pct}%)`;
  
  let grade = pct >= 90 ? 'Xuất sắc!' : pct >= 70 ? 'Tốt lắm!' : pct >= 50 ? 'Cần ôn thêm!' : 'Hãy ôn lại lý thuyết!';
  document.getElementById('gram-grade').textContent = grade;

  const history = JSON.parse(localStorage.getItem('gram-history') || '[]');
  history.push({date: new Date().toISOString(), topic: gramTopic, score: gramScoreVal, total: gramQuestions.length});
  localStorage.setItem('gram-history', JSON.stringify(history.slice(-30)));

  const reviewEl = document.getElementById('gram-review');
  if(gramWrong.length > 0) {
    reviewEl.innerHTML = '<h3>Câu cần ôn lại:</h3>' +
      gramWrong.map(q => `<div style="padding:12px 16px; background:#fff5f5; border-radius:8px; margin:8px 0; border-left:4px solid #fc8181;">
        <div style="font-weight:600; margin-bottom:4px;">${q.q}</div>
        <div style="color:#38a169; font-size:14px;">Đáp án: ${q.opts[q.ans]}</div>
        <div style="color:#718096; font-size:13px; margin-top:4px;">${q.explain}</div>
      </div>`).join('');
  } else { reviewEl.innerHTML = ''; }
}
</script>
