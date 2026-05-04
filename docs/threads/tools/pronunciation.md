# Luyện Phát Âm

Luyện phát âm tiếng Anh với công nghệ nhận diện giọng nói. Nghe mẫu, ghi âm giọng của bạn và so sánh.

<div id="pronunciation-app">
  <div style="text-align:center; margin:20px 0;">
    <label for="pron-cat" style="font-weight:600;">Chọn chủ đề: </label>
    <select id="pron-cat" onchange="loadPronCategory()" style="padding:8px 16px; border-radius:8px; border:1px solid #ddd; font-size:15px;">
      <option value="minimal">Cặp từ tối thiểu (Minimal Pairs)</option>
      <option value="vowels">Nguyên âm khó</option>
      <option value="consonants">Phụ âm khó</option>
      <option value="stress">Trọng âm</option>
      <option value="sentences">Câu thường dùng</option>
    </select>
  </div>

  <div id="pron-card" style="max-width:600px; margin:0 auto; background:linear-gradient(135deg,#1a365d 0%,#2d3748 100%); border-radius:16px; padding:32px; color:white; text-align:center; box-shadow:0 10px 30px rgba(0,0,0,0.2);">
    <div id="pron-counter" style="font-size:13px; opacity:0.6; margin-bottom:8px;"></div>
    <div id="pron-word" style="font-size:36px; font-weight:700; margin-bottom:8px;"></div>
    <div id="pron-phonetic" style="font-size:18px; opacity:0.8; margin-bottom:4px;"></div>
    <div id="pron-meaning" style="font-size:15px; opacity:0.7; margin-bottom:16px;"></div>
    <div id="pron-tip" style="font-size:13px; background:rgba(255,255,255,0.1); padding:10px 16px; border-radius:8px; margin-bottom:20px; line-height:1.5;"></div>

    <div style="display:flex; justify-content:center; gap:12px; flex-wrap:wrap; margin-bottom:16px;">
      <button onclick="pronSpeak('slow')" style="padding:10px 20px; border:none; background:#4299e1; color:white; border-radius:8px; cursor:pointer; font-size:14px;">🔊 Chậm</button>
      <button onclick="pronSpeak('normal')" style="padding:10px 20px; border:none; background:#3182ce; color:white; border-radius:8px; cursor:pointer; font-size:14px;">🔊 Bình thường</button>
      <button id="pron-record-btn" onclick="toggleRecord()" style="padding:10px 20px; border:none; background:#e53e3e; color:white; border-radius:8px; cursor:pointer; font-size:14px;">🎤 Ghi âm</button>
    </div>

    <div id="pron-result" style="min-height:40px; margin-top:8px;"></div>
  </div>

  <div style="text-align:center; margin:20px 0; display:flex; justify-content:center; gap:12px;">
    <button onclick="prevPron()" style="padding:10px 24px; border:none; background:#e2e8f0; border-radius:8px; cursor:pointer; font-size:15px;">⬅ Trước</button>
    <button onclick="nextPron()" style="padding:10px 24px; border:none; background:#e2e8f0; border-radius:8px; cursor:pointer; font-size:15px;">Tiếp ➡</button>
  </div>

  <div style="max-width:600px; margin:20px auto; background:#f7fafc; border-radius:12px; padding:20px;">
    <h3 style="margin-top:0;">Mẹo luyện phát âm</h3>
    <ul style="line-height:2; color:#4a5568;">
      <li><strong>Nghe trước, nói sau:</strong> Nghe mẫu 2-3 lần trước khi thử phát âm</li>
      <li><strong>Ghi âm so sánh:</strong> Ghi lại giọng mình và so với mẫu</li>
      <li><strong>Tập chậm:</strong> Bắt đầu chậm, tăng tốc dần khi đã chính xác</li>
      <li><strong>Chú ý miệng:</strong> Quan sát vị trí lưỡi, môi khi phát âm</li>
      <li><strong>Lặp lại hàng ngày:</strong> Luyện 10-15 phút mỗi ngày hiệu quả hơn 1 giờ/tuần</li>
    </ul>
  </div>
</div>

<script>
const pronData = {
  minimal: [
    {word:"ship / sheep",phonetic:"/ʃɪp/ vs /ʃiːp/",meaning:"Tàu / Con cừu",tip:"'ship' dùng nguyên âm ngắn /ɪ/ (miệng hơi mở). 'sheep' dùng nguyên âm dài /iː/ (miệng kéo dài, môi rộng)."},
    {word:"bit / beat",phonetic:"/bɪt/ vs /biːt/",meaning:"Mảnh / Đánh bại",tip:"Phân biệt /ɪ/ ngắn và /iː/ dài. Kéo dài âm /iː/ trong 'beat'."},
    {word:"pull / pool",phonetic:"/pʊl/ vs /puːl/",meaning:"Kéo / Bể bơi",tip:"/ʊ/ trong 'pull' ngắn hơn /uː/ trong 'pool'. Tròn môi hơn cho /uː/."},
    {word:"bed / bad",phonetic:"/bed/ vs /bæd/",meaning:"Giường / Tệ",tip:"/e/ miệng mở vừa, /æ/ miệng mở rộng hơn (như khi nói 'a' trong tiếng Việt)."},
    {word:"think / sink",phonetic:"/θɪŋk/ vs /sɪŋk/",meaning:"Nghĩ / Chìm",tip:"/θ/ đặt lưỡi giữa hai hàm răng, thổi hơi qua. /s/ lưỡi ở sau răng trên."},
    {word:"rice / lice",phonetic:"/raɪs/ vs /laɪs/",meaning:"Gạo / Rận",tip:"/r/ cong lưỡi ra sau, không chạm vòm. /l/ đầu lưỡi chạm vòm miệng phía trước."},
    {word:"right / light",phonetic:"/raɪt/ vs /laɪt/",meaning:"Đúng / Ánh sáng",tip:"Tương tự cặp trên. Luyện tập R và L riêng biệt trước."},
    {word:"van / fan",phonetic:"/væn/ vs /fæn/",meaning:"Xe tải / Quạt",tip:"/v/ rung dây thanh, răng trên chạm môi dưới. /f/ không rung, chỉ thổi hơi."},
  ],
  vowels: [
    {word:"cat",phonetic:"/kæt/",meaning:"Con mèo",tip:"Âm /æ/ — mở miệng rộng, lưỡi phẳng thấp. Giống âm 'e' mở rộng trong tiếng Việt."},
    {word:"cup",phonetic:"/kʌp/",meaning:"Cái cốc",tip:"Âm /ʌ/ — miệng mở vừa, lưỡi ở giữa. Gần giống 'ơ' ngắn trong tiếng Việt."},
    {word:"bird",phonetic:"/bɜːrd/",meaning:"Con chim",tip:"Âm /ɜː/ — giữ lưỡi ở giữa, kéo dài, cuộn lưỡi nhẹ. Không có âm tương đương tiếng Việt."},
    {word:"book",phonetic:"/bʊk/",meaning:"Quyển sách",tip:"Âm /ʊ/ — tròn môi nhẹ, ngắn. Gần giống 'u' ngắn tiếng Việt."},
    {word:"food",phonetic:"/fuːd/",meaning:"Thức ăn",tip:"Âm /uː/ — tròn môi hoàn toàn, kéo dài. Giống 'u' dài trong tiếng Việt."},
    {word:"about",phonetic:"/əˈbaʊt/",meaning:"Về, khoảng",tip:"Âm /ə/ (schwa) — âm phổ biến nhất tiếng Anh, rất nhẹ và ngắn, miệng gần như không mở."},
  ],
  consonants: [
    {word:"three",phonetic:"/θriː/",meaning:"Ba (số 3)",tip:"Âm /θ/ — đặt đầu lưỡi giữa hai hàm răng, thổi hơi nhẹ. Người Việt thường phát thành /t/ hoặc /s/."},
    {word:"this",phonetic:"/ðɪs/",meaning:"Cái này",tip:"Âm /ð/ — giống /θ/ nhưng có rung dây thanh. Đặt lưỡi giữa răng và rung cổ."},
    {word:"zoo",phonetic:"/zuː/",meaning:"Vườn thú",tip:"Âm /z/ — giống /s/ nhưng rung dây thanh. Đặt tay lên cổ để cảm nhận rung."},
    {word:"vision",phonetic:"/ˈvɪʒ.ən/",meaning:"Tầm nhìn",tip:"Âm /ʒ/ — môi tròn, lưỡi gần vòm miệng, rung cổ. Giống âm 'gi' nhẹ trong tiếng Việt."},
    {word:"church",phonetic:"/tʃɜːrtʃ/",meaning:"Nhà thờ",tip:"Âm /tʃ/ — bắt đầu bằng /t/ rồi chuyển sang /ʃ/. Giống âm 'ch' trong tiếng Việt."},
    {word:"world",phonetic:"/wɜːrld/",meaning:"Thế giới",tip:"Kết hợp /w/ + /ɜːr/ + /ld/ — tròn môi cho /w/, cuộn lưỡi cho /r/, kết thúc bằng /ld/."},
  ],
  stress: [
    {word:"record (n) / record (v)",phonetic:"/ˈrek.ɚd/ vs /rɪˈkɔːrd/",meaning:"Bản ghi / Ghi lại",tip:"Danh từ nhấn âm 1: REcord. Động từ nhấn âm 2: reCORD. Quy tắc phổ biến trong tiếng Anh."},
    {word:"present (n) / present (v)",phonetic:"/ˈprez.ənt/ vs /prɪˈzent/",meaning:"Quà / Trình bày",tip:"Danh từ: PREsent. Động từ: preSENT. Chú ý âm tiết được nhấn mạnh hơn và kéo dài hơn."},
    {word:"photograph / photographer / photographic",phonetic:"/ˈfoʊ.t̬ə.ɡræf/",meaning:"Ảnh / Thợ ảnh / Thuộc ảnh",tip:"Trọng âm di chuyển: PHOtograph → phoTOGrapher → photoGRAPHic. Quy tắc hậu tố ảnh hưởng trọng âm."},
    {word:"economy / economic",phonetic:"/ɪˈkɑː.nə.mi/ vs /ˌek.əˈnɑː.mɪk/",meaning:"Kinh tế / Thuộc kinh tế",tip:"eCONomy → ecoNOMic. Hậu tố -ic đẩy trọng âm về trước nó."},
    {word:"comfortable",phonetic:"/ˈkʌmf.tɚ.bəl/",meaning:"Thoải mái",tip:"3 âm tiết thực tế: COMF-ter-ble (không phải com-FOR-ta-ble). Âm tiết giữa thường bị nuốt."},
  ],
  sentences: [
    {word:"How are you doing today?",phonetic:"/haʊ ɑːr juː ˈduː.ɪŋ təˈdeɪ/",meaning:"Bạn hôm nay thế nào?",tip:"Nối âm: 'How_are' → /haʊ_ɑːr/. Nhấn 'doing' và 'today'. 'Are you' có thể rút gọn thành /ɑːrjə/."},
    {word:"I'd like a cup of coffee, please.",phonetic:"/aɪd laɪk ə kʌp əv ˈkɑː.fi pliːz/",meaning:"Tôi muốn một cốc cà phê.",tip:"'Cup of' → /kʌpəv/ (nối âm). 'I'd like' rút gọn từ 'I would like'. Nhấn 'coffee'."},
    {word:"Could you repeat that, please?",phonetic:"/kʊd juː rɪˈpiːt ðæt pliːz/",meaning:"Bạn có thể nhắc lại không?",tip:"'Could you' → /kʊdʒə/ (nối âm tự nhiên). Nhấn 'repeat'. Câu lịch sự cần dùng hàng ngày."},
    {word:"What do you do for a living?",phonetic:"/wʌt duː juː duː fɔːr ə ˈlɪv.ɪŋ/",meaning:"Bạn làm nghề gì?",tip:"'What do you' → /wʌdəjə/ (khẩu ngữ). Nhấn 'do' thứ 2 và 'living'. Câu hỏi phổ biến khi gặp gỡ."},
    {word:"I'm looking forward to hearing from you.",phonetic:"/aɪm ˈlʊk.ɪŋ ˈfɔːr.wɚd tə ˈhɪr.ɪŋ frʌm juː/",meaning:"Tôi mong nhận được tin từ bạn.",tip:"Nhấn 'looking', 'forward', 'hearing'. 'To' đọc nhẹ /tə/. Câu kết email chuyên nghiệp."},
  ]
};

let pronCategory = 'minimal';
let pronIndex = 0;
let isRecording = false;
let recognition = null;

function loadPronCategory() {
  const sel = document.getElementById('pron-cat');
  if(sel) pronCategory = sel.value;
  pronIndex = 0;
  showPronCard();
}

function showPronCard() {
  const items = pronData[pronCategory];
  if(!items || !items[pronIndex]) return;
  const item = items[pronIndex];
  document.getElementById('pron-word').textContent = item.word;
  document.getElementById('pron-phonetic').textContent = item.phonetic;
  document.getElementById('pron-meaning').textContent = item.meaning;
  document.getElementById('pron-tip').textContent = item.tip;
  document.getElementById('pron-counter').textContent = `${pronIndex+1}/${items.length}`;
  document.getElementById('pron-result').innerHTML = '';
}

function pronSpeak(speed) {
  const items = pronData[pronCategory];
  const text = items[pronIndex].word.split(' / ')[0].split(' (')[0];
  const u = new SpeechSynthesisUtterance(text);
  u.lang = 'en-US';
  u.rate = speed === 'slow' ? 0.5 : 0.9;
  speechSynthesis.speak(u);
}

function toggleRecord() {
  if(!('webkitSpeechRecognition' in window || 'SpeechRecognition' in window)) {
    document.getElementById('pron-result').innerHTML = '<span style="color:#e53e3e;">Trình duyệt không hỗ trợ nhận diện giọng nói. Hãy dùng Chrome.</span>';
    return;
  }
  
  if(isRecording) {
    recognition.stop();
    return;
  }

  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  recognition = new SpeechRecognition();
  recognition.lang = 'en-US';
  recognition.continuous = false;
  recognition.interimResults = false;
  
  const btn = document.getElementById('pron-record-btn');
  btn.textContent = '⏹ Dừng ghi';
  btn.style.background = '#c53030';
  isRecording = true;

  recognition.onresult = function(event) {
    const transcript = event.results[0][0].transcript.toLowerCase();
    const confidence = Math.round(event.results[0][0].confidence * 100);
    const expected = pronData[pronCategory][pronIndex].word.split(' / ')[0].split(' (')[0].toLowerCase();
    
    let resultHTML = `<div style="margin-top:8px;">`;
    resultHTML += `<div style="font-size:14px; opacity:0.7;">Bạn nói:</div>`;
    resultHTML += `<div style="font-size:20px; font-weight:600;">"${transcript}"</div>`;
    
    if(transcript.includes(expected) || expected.includes(transcript)) {
      resultHTML += `<div style="margin-top:8px; color:#68d391; font-weight:600;">Phát âm tốt! (${confidence}% chính xác)</div>`;
    } else {
      resultHTML += `<div style="margin-top:8px; color:#fc8181;">Thử lại nhé! Cần nói: "${expected}"</div>`;
    }
    resultHTML += `</div>`;
    document.getElementById('pron-result').innerHTML = resultHTML;
  };

  recognition.onerror = function(event) {
    document.getElementById('pron-result').innerHTML = `<span style="color:#fc8181;">Lỗi: ${event.error}. Hãy thử lại.</span>`;
  };

  recognition.onend = function() {
    btn.textContent = '🎤 Ghi âm';
    btn.style.background = '#e53e3e';
    isRecording = false;
  };

  recognition.start();
  document.getElementById('pron-result').innerHTML = '<span style="opacity:0.7;">Đang nghe... Hãy phát âm từ trên.</span>';
}

function nextPron() {
  const items = pronData[pronCategory];
  pronIndex = (pronIndex + 1) % items.length;
  showPronCard();
}

function prevPron() {
  const items = pronData[pronCategory];
  pronIndex = (pronIndex - 1 + items.length) % items.length;
  showPronCard();
}

if(document.getElementById('pronunciation-app')) loadPronCategory();
</script>
