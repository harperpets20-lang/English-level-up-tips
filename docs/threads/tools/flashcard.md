# Flashcard Từ Vựng

Luyện tập từ vựng với thẻ lật tương tác. Click vào thẻ để xem nghĩa, sử dụng các nút để điều hướng.

<div id="flashcard-app">
  <div style="text-align:center; margin: 20px 0;">
    <label for="fc-category" style="font-weight:600;">Chọn chủ đề: </label>
    <select id="fc-category" onchange="loadCategory()" style="padding:8px 16px; border-radius:8px; border:1px solid #ddd; font-size:15px;">
      <option value="common">Từ vựng thông dụng</option>
      <option value="tech">Công nghệ</option>
      <option value="ielts">IELTS / TOEFL</option>
      <option value="business">Kinh doanh</option>
      <option value="daily">Giao tiếp hàng ngày</option>
    </select>
  </div>

  <div id="flashcard-container" style="display:flex; justify-content:center; align-items:center; min-height:300px;">
    <div id="flashcard" onclick="flipCard()" style="width:400px; max-width:90vw; height:250px; perspective:1000px; cursor:pointer;">
      <div id="card-inner" style="position:relative; width:100%; height:100%; transition:transform 0.6s; transform-style:preserve-3d;">
        <div id="card-front" style="position:absolute; width:100%; height:100%; backface-visibility:hidden; background:linear-gradient(135deg,#667eea 0%,#764ba2 100%); border-radius:16px; display:flex; flex-direction:column; justify-content:center; align-items:center; color:white; padding:20px; box-sizing:border-box; box-shadow:0 10px 30px rgba(0,0,0,0.2);">
          <div id="card-word" style="font-size:32px; font-weight:700; margin-bottom:10px;">Loading...</div>
          <div id="card-phonetic" style="font-size:16px; opacity:0.8;"></div>
          <div style="margin-top:15px; font-size:13px; opacity:0.6;">Click để xem nghĩa</div>
        </div>
        <div id="card-back" style="position:absolute; width:100%; height:100%; backface-visibility:hidden; background:linear-gradient(135deg,#f093fb 0%,#f5576c 100%); border-radius:16px; display:flex; flex-direction:column; justify-content:center; align-items:center; color:white; padding:20px; box-sizing:border-box; transform:rotateY(180deg); box-shadow:0 10px 30px rgba(0,0,0,0.2);">
          <div id="card-meaning" style="font-size:22px; font-weight:600; margin-bottom:8px;"></div>
          <div id="card-example" style="font-size:14px; opacity:0.9; text-align:center; line-height:1.5;"></div>
        </div>
      </div>
    </div>
  </div>

  <div style="text-align:center; margin:20px 0; display:flex; justify-content:center; gap:12px; flex-wrap:wrap;">
    <button onclick="prevCard()" style="padding:10px 24px; border:none; background:#e2e8f0; border-radius:8px; cursor:pointer; font-size:15px;">⬅ Trước</button>
    <button onclick="speakWord()" style="padding:10px 24px; border:none; background:#4299e1; color:white; border-radius:8px; cursor:pointer; font-size:15px;">🔊 Phát âm</button>
    <button onclick="markKnown()" style="padding:10px 24px; border:none; background:#48bb78; color:white; border-radius:8px; cursor:pointer; font-size:15px;">Đã biết</button>
    <button onclick="nextCard()" style="padding:10px 24px; border:none; background:#e2e8f0; border-radius:8px; cursor:pointer; font-size:15px;">Tiếp ➡</button>
  </div>

  <div id="fc-progress" style="text-align:center; color:#718096; font-size:14px;"></div>
  <div style="max-width:400px; margin:10px auto; background:#e2e8f0; border-radius:8px; height:8px;">
    <div id="fc-progress-bar" style="background:linear-gradient(90deg,#667eea,#764ba2); height:100%; border-radius:8px; transition:width 0.3s;"></div>
  </div>
</div>

<script>
const vocabData = {
  common: [
    {word:"accomplish",phonetic:"/əˈkɑːm.plɪʃ/",meaning:"Hoàn thành, đạt được",example:"She accomplished her goal of running a marathon."},
    {word:"beneficial",phonetic:"/ˌben.ɪˈfɪʃ.əl/",meaning:"Có lợi, có ích",example:"Exercise is beneficial to your health."},
    {word:"comprehend",phonetic:"/ˌkɑːm.prɪˈhend/",meaning:"Hiểu, lĩnh hội",example:"It's difficult to comprehend the scale of the universe."},
    {word:"diligent",phonetic:"/ˈdɪl.ɪ.dʒənt/",meaning:"Siêng năng, cần cù",example:"She is a diligent student who always completes her homework."},
    {word:"elaborate",phonetic:"/ɪˈlæb.ər.ət/",meaning:"Chi tiết, công phu",example:"Could you elaborate on your proposal?"},
    {word:"fluctuate",phonetic:"/ˈflʌk.tʃu.eɪt/",meaning:"Dao động, biến đổi",example:"Prices fluctuate depending on supply and demand."},
    {word:"genuine",phonetic:"/ˈdʒen.ju.ɪn/",meaning:"Chân thật, thật sự",example:"He showed genuine concern for her well-being."},
    {word:"hesitate",phonetic:"/ˈhez.ɪ.teɪt/",meaning:"Do dự, lưỡng lự",example:"Don't hesitate to ask if you need help."},
    {word:"inevitable",phonetic:"/ɪˈnev.ɪ.tə.bəl/",meaning:"Không thể tránh khỏi",example:"Change is inevitable in today's fast-paced world."},
    {word:"justify",phonetic:"/ˈdʒʌs.tɪ.faɪ/",meaning:"Biện minh, chứng minh",example:"How can you justify spending so much money?"},
    {word:"meticulous",phonetic:"/məˈtɪk.jə.ləs/",meaning:"Tỉ mỉ, cẩn thận",example:"She is meticulous about keeping records."},
    {word:"negotiate",phonetic:"/nɪˈɡoʊ.ʃi.eɪt/",meaning:"Đàm phán, thương lượng",example:"They negotiated a fair deal for both parties."},
    {word:"persistent",phonetic:"/pɚˈsɪs.tənt/",meaning:"Kiên trì, bền bỉ",example:"Her persistent efforts finally paid off."},
    {word:"reluctant",phonetic:"/rɪˈlʌk.tənt/",meaning:"Miễn cưỡng, không sẵn lòng",example:"He was reluctant to admit his mistake."},
    {word:"substantial",phonetic:"/səbˈstæn.ʃəl/",meaning:"Đáng kể, quan trọng",example:"They made substantial progress on the project."},
  ],
  tech: [
    {word:"algorithm",phonetic:"/ˈæl.ɡə.rɪ.ðəm/",meaning:"Thuật toán",example:"The search algorithm returns results in milliseconds."},
    {word:"bandwidth",phonetic:"/ˈbænd.wɪdθ/",meaning:"Băng thông",example:"We need more bandwidth to handle the traffic."},
    {word:"compile",phonetic:"/kəmˈpaɪl/",meaning:"Biên dịch",example:"The code failed to compile due to syntax errors."},
    {word:"debug",phonetic:"/diːˈbʌɡ/",meaning:"Gỡ lỗi",example:"I spent hours debugging the application."},
    {word:"encrypt",phonetic:"/ɪnˈkrɪpt/",meaning:"Mã hóa",example:"All passwords are encrypted before storage."},
    {word:"framework",phonetic:"/ˈfreɪm.wɝːk/",meaning:"Khung, nền tảng",example:"React is a popular JavaScript framework."},
    {word:"repository",phonetic:"/rɪˈpɑː.zɪ.tɔːr.i/",meaning:"Kho lưu trữ",example:"Push your code to the remote repository."},
    {word:"deploy",phonetic:"/dɪˈplɔɪ/",meaning:"Triển khai",example:"We deploy updates every two weeks."},
    {word:"iterate",phonetic:"/ˈɪt.ə.reɪt/",meaning:"Lặp lại, duyệt",example:"We iterate over the array to find the maximum value."},
    {word:"scalable",phonetic:"/ˈskeɪ.lə.bəl/",meaning:"Có khả năng mở rộng",example:"The architecture needs to be scalable."},
    {word:"refactor",phonetic:"/riːˈfæk.tər/",meaning:"Tái cấu trúc code",example:"Let's refactor this module for better readability."},
    {word:"middleware",phonetic:"/ˈmɪd.əl.wer/",meaning:"Phần mềm trung gian",example:"The middleware handles authentication."},
  ],
  ielts: [
    {word:"alleviate",phonetic:"/əˈliː.vi.eɪt/",meaning:"Giảm bớt, làm dịu",example:"Measures to alleviate poverty in urban areas."},
    {word:"contemporary",phonetic:"/kənˈtem.pə.rer.i/",meaning:"Đương đại, hiện đại",example:"Contemporary art often challenges traditional norms."},
    {word:"deteriorate",phonetic:"/dɪˈtɪr.i.ə.reɪt/",meaning:"Xấu đi, suy thoái",example:"Air quality continues to deteriorate in major cities."},
    {word:"exacerbate",phonetic:"/ɪɡˈzæs.ɚ.beɪt/",meaning:"Làm trầm trọng thêm",example:"Climate change exacerbates natural disasters."},
    {word:"fluctuation",phonetic:"/ˌflʌk.tʃuˈeɪ.ʃən/",meaning:"Sự dao động",example:"Currency fluctuations affect international trade."},
    {word:"predominant",phonetic:"/prɪˈdɑː.mə.nənt/",meaning:"Chiếm ưu thế, chủ yếu",example:"English is the predominant language in academia."},
    {word:"sustainable",phonetic:"/səˈsteɪ.nə.bəl/",meaning:"Bền vững",example:"We need sustainable solutions for energy production."},
    {word:"unprecedented",phonetic:"/ʌnˈpres.ɪ.den.tɪd/",meaning:"Chưa từng có tiền lệ",example:"The pandemic caused unprecedented disruption."},
    {word:"versatile",phonetic:"/ˈvɝː.sə.t̬əl/",meaning:"Đa năng, linh hoạt",example:"Python is a versatile programming language."},
    {word:"paradigm",phonetic:"/ˈper.ə.daɪm/",meaning:"Mô hình, khuôn mẫu",example:"A paradigm shift in education is needed."},
  ],
  business: [
    {word:"acquisition",phonetic:"/ˌæk.wɪˈzɪʃ.ən/",meaning:"Sự mua lại, thu mua",example:"The acquisition of the startup cost $2 billion."},
    {word:"benchmark",phonetic:"/ˈbentʃ.mɑːrk/",meaning:"Tiêu chuẩn, chuẩn đối sánh",example:"We use industry benchmarks to measure performance."},
    {word:"leverage",phonetic:"/ˈlev.ər.ɪdʒ/",meaning:"Tận dụng, đòn bẩy",example:"We should leverage our existing customer base."},
    {word:"ROI",phonetic:"/ɑːr.oʊˈaɪ/",meaning:"Lợi tức đầu tư",example:"What's the expected ROI on this campaign?"},
    {word:"stakeholder",phonetic:"/ˈsteɪk.hoʊl.dɚ/",meaning:"Bên liên quan",example:"All stakeholders must approve the budget."},
    {word:"revenue",phonetic:"/ˈrev.ən.uː/",meaning:"Doanh thu",example:"Annual revenue exceeded $10 million."},
    {word:"scalability",phonetic:"/ˌskeɪ.ləˈbɪl.ə.t̬i/",meaning:"Khả năng mở rộng",example:"Scalability is key for a growing business."},
    {word:"outsource",phonetic:"/ˈaʊt.sɔːrs/",meaning:"Thuê ngoài",example:"Many companies outsource customer support."},
  ],
  daily: [
    {word:"appreciate",phonetic:"/əˈpriː.ʃi.eɪt/",meaning:"Đánh giá cao, trân trọng",example:"I really appreciate your help with this."},
    {word:"awkward",phonetic:"/ˈɑː.kwɚd/",meaning:"Lúng túng, ngượng ngùng",example:"There was an awkward silence in the room."},
    {word:"grab",phonetic:"/ɡræb/",meaning:"Nắm lấy, chộp",example:"Let's grab a coffee after work."},
    {word:"hang out",phonetic:"/hæŋ aʊt/",meaning:"Đi chơi, la cà",example:"Do you want to hang out this weekend?"},
    {word:"figure out",phonetic:"/ˈfɪɡ.jɚ aʊt/",meaning:"Tìm ra, hiểu ra",example:"I need to figure out how to fix this."},
    {word:"look forward to",phonetic:"/lʊk ˈfɔːr.wɚd tuː/",meaning:"Mong chờ, trông đợi",example:"I look forward to hearing from you."},
    {word:"overwhelmed",phonetic:"/ˌoʊ.vɚˈwelmd/",meaning:"Choáng ngợp, quá tải",example:"She felt overwhelmed by the workload."},
    {word:"procrastinate",phonetic:"/proʊˈkræs.tə.neɪt/",meaning:"Trì hoãn, chần chừ",example:"Stop procrastinating and start studying!"},
    {word:"exhausted",phonetic:"/ɪɡˈzɑː.stɪd/",meaning:"Kiệt sức, mệt lả",example:"I'm exhausted after working all day."},
    {word:"commute",phonetic:"/kəˈmjuːt/",meaning:"Đi lại (làm việc)",example:"My daily commute takes about 45 minutes."},
  ]
};

let currentCategory = 'common';
let currentIndex = 0;
let isFlipped = false;
let knownWords = JSON.parse(localStorage.getItem('fc-known') || '{}');

function loadCategory() {
  const sel = document.getElementById('fc-category');
  if(sel) currentCategory = sel.value;
  currentIndex = 0;
  isFlipped = false;
  showCard();
}

function showCard() {
  const cards = vocabData[currentCategory];
  if(!cards || !cards[currentIndex]) return;
  const c = cards[currentIndex];
  const front = document.getElementById('card-word');
  const phonetic = document.getElementById('card-phonetic');
  const meaning = document.getElementById('card-meaning');
  const example = document.getElementById('card-example');
  const inner = document.getElementById('card-inner');
  if(front) front.textContent = c.word;
  if(phonetic) phonetic.textContent = c.phonetic;
  if(meaning) meaning.textContent = c.meaning;
  if(example) example.textContent = c.example;
  if(inner) inner.style.transform = '';
  isFlipped = false;
  updateProgress();
}

function flipCard() {
  const inner = document.getElementById('card-inner');
  if(!inner) return;
  isFlipped = !isFlipped;
  inner.style.transform = isFlipped ? 'rotateY(180deg)' : '';
}

function nextCard() {
  const cards = vocabData[currentCategory];
  if(currentIndex < cards.length - 1) currentIndex++;
  else currentIndex = 0;
  showCard();
}

function prevCard() {
  const cards = vocabData[currentCategory];
  if(currentIndex > 0) currentIndex--;
  else currentIndex = cards.length - 1;
  showCard();
}

function speakWord() {
  const cards = vocabData[currentCategory];
  const word = cards[currentIndex].word;
  const utterance = new SpeechSynthesisUtterance(word);
  utterance.lang = 'en-US';
  utterance.rate = 0.8;
  speechSynthesis.speak(utterance);
}

function markKnown() {
  const cards = vocabData[currentCategory];
  const key = currentCategory + ':' + cards[currentIndex].word;
  knownWords[key] = true;
  localStorage.setItem('fc-known', JSON.stringify(knownWords));
  updateProgress();
  nextCard();
}

function updateProgress() {
  const cards = vocabData[currentCategory];
  const known = cards.filter(c => knownWords[currentCategory + ':' + c.word]).length;
  const el = document.getElementById('fc-progress');
  const bar = document.getElementById('fc-progress-bar');
  if(el) el.textContent = `Thẻ ${currentIndex+1}/${cards.length} | Đã thuộc: ${known}/${cards.length}`;
  if(bar) bar.style.width = (known/cards.length*100) + '%';
}

// init
if(document.getElementById('flashcard-app')) {
  loadCategory();
}
</script>
