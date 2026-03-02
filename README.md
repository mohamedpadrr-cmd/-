<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>مسجد الأمير فواز - شاشة الأوقات والأذكار</title>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');

  * {
    box-sizing: border-box;
  }

  body {
    margin: 0;
    background: #5c5c5c url('https://images.unsplash.com/photo-1580418827493-f2b22c0a76cb?q=80&w=3840') no-repeat center center fixed;
    background-size: cover;
    font-family: 'Cairo', sans-serif;
    color: #fff;
    overflow: hidden;
  }

  .container {
    width: 100%;
    max-width: 1200px;
    margin: auto;
    padding: 15px;
    background: #1a1a1add;
    border-radius: 10px;
    box-shadow: 0 0 25px #000a inset;
    display: grid;
    grid-template-columns: 2fr 1fr;
    grid-template-rows: auto 1fr auto;
    gap: 15px;
    direction: rtl;
  }

  /* العنوان والتاريخ */
  .header {
    grid-column: 1 / 3;
    background: linear-gradient(90deg, #423400, #9c7e00);
    border-radius: 10px 10px 0 0;
    padding: 10px 20px;
    color: #ffdb00;
    font-size: 24px;
    font-weight: 700;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header .date-info {
    font-size: 18px;
    color: #fff5b7;
  }

  /* القسم الرئيسي الأيسر */
  .main-left {
    background: #2a2a2a;
    border-radius: 10px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;
  }

  /* آيات قرآنية */
  .quran-verse {
    background: #3c3c3c;
    border-radius: 10px;
    padding: 15px;
    font-size: 22px;
    line-height: 1.5;
    color: #ddd;
    margin-bottom: 15px;
    border: 2px solid #a27f00;
  }

  .quran-verse span.highlight {
    color: #1ccc0e;
    font-weight: bold;
  }

  /* شريط جانبي */
  .side-bar {
    position: absolute;
    top: 0;
    right: -80px;
    width: 80px;
    height: 100%;
    background: #624a00;
    color: #fff;
    font-weight: 700;
    font-size: 18px;
    writing-mode: vertical-rl;
    text-align: center;
    padding-top: 20px;
    border-radius: 10px 0 0 10px;
  }

  /* القسم الأيمن */
  .main-right {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }

  /* ساعة تناظرية */
  .analog-clock {
    width: 160px;
    height: 160px;
    background: #222;
    border: 5px solid #d4af37;
    border-radius: 50%;
    position: relative;
    margin: auto;
    box-shadow: 0 0 10px #d4af37;
  }

  .hand {
    position: absolute;
    width: 50%;
    height: 4px;
    background: #d4af37;
    top: 50%;
    left: 50%;
    transform-origin: 0% 50%;
    transform: rotate(90deg);
    border-radius: 4px;
    transition: all 0.5s ease-in-out;
  }

  .hand.hour {
    height: 6px;
    width: 35%;
  }

  .hand.minute {
    height: 4px;
    width: 45%;
  }

  .hand.second {
    background: #ff4444;
    height: 2px;
    width: 48%;
  }

  /* نقطة مركز الساعة */
  .center-dot {
    position: absolute;
    width: 14px;
    height: 14px;
    background: #d4af37;
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 8px #d4af37;
  }

  /* الساعة الرقمية */
  .digital-clock {
    font-size: 36px;
    font-weight: 700;
    color: #ff3232;
    text-align: center;
    font-family: monospace;
    margin-top: 10px;
  }

  /* AM/PM صغير */
  .ampm {
    font-size: 20px;
    vertical-align: super;
    margin-right: 6px;
    color: #ff3232;
  }

  /* التاريخ الهجري */
  .hijri-date {
    text-align: center;
    font-size: 22px;
    font-weight: 700;
    color: #fff5b7;
    margin-bottom: 15px;
  }

  /* أوقات الصلاة */
  .prayers {
    background: #222;
    border-radius: 10px;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    color: #d4af37;
    box-shadow: 0 0 8px #a6841b inset;
  }

  .prayers .row {
    display: flex;
    justify-content: space-between;
    padding: 8px 10px;
    border-bottom: 1px solid #d4af371a;
    transition: background-color 0.3s ease;
  }

  .prayers .row:last-child {
    border-bottom: none;
  }

  .prayers .row.next {
    background: #d4af37;
    color: #000;
    border-radius: 8px;
  }

  /* عداد الإقامة */
  .iqama-countdown {
    font-size: 28px;
    font-weight: 700;
    text-align: center;
    margin-top: 12px;
    color: #ff4646;
    text-shadow: 0 0 4px #ff4646cc;
  }

  /* درجة الحرارة */
  .temperature {
    font-size: 20px;
    color: #f7f7f7;
    font-weight: 700;
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-top: 5px;
  }

  .temperature div {
    display: flex;
    align-items: center;
    gap: 6px;
  }

  /* رمز درجة الحرارة (ممكن تعديل لاحقاً) */
  .temp-icon {
    font-weight: normal;
    font-size: 20px;
  }

  /* شاشة الأذكار */
  .azkar-box {
    background: #2f2f2f;
    border-radius: 10px;
    padding: 15px;
    color: #b9d02b;
    font-size: 22px;
    font-weight: 700;
    text-align: center;
    min-height: 120px;
    box-shadow: 0 0 10px #b9d02b inset;
    user-select: none;
  }
</style>
</head>

<body>
  <div class="container">
    <div class="header">
      <div>السبت 21 ديسمبر 2024</div>
      <div class="date-info" id="gregorianDate"></div>
    </div>

    <div class="main-left">
      <div class="side-bar">حسابات الأوقات حسب مكة</div>
      <div class="quran-verse" id="quranVerse">
        <p>وَكَانَ لَمْ يَغْنِعُوا فِيهَا ۝ <span class="highlight">يَعِيشُونَ</span> أي كأنهم لو قيّموا فيها ولم يعيشوا فيها فقط أي في ديارهم وليس معهمنا يعتنوا وتكثر أموالهم.</p>
      </div>
    </div>

    <div class="main-right">
      <div class="analog-clock" aria-label="ساعة تناظرية">
        <div class="hand hour" id="hourHand"></div>
        <div class="hand minute" id="minuteHand"></div>
        <div class="hand second" id="secondHand"></div>
        <div class="center-dot"></div>
      </div>
      <div class="digital-clock" id="digitalClock">05:41:52 <span class="ampm" id="ampm">PM</span></div>
      <div class="hijri-date" id="hijriDate">١٤ جمادى الأولى ١٤٤٦ هـ</div>

      <div class="temperature" aria-label="درجة الحرارة">
        <div><span class="temp-icon">🌡️</span> <span id="tempCurrent">22°C</span></div>
        <div><span class="temp-icon">🌙</span> <span id="tempNight">20°C</span></div>
      </div>

      <div class="prayers" id="prayersList"></div>
      <div class="iqama-countdown" id="iqamaCountdown"></div>

      <!-- مكان الأذكار -->
      <div class="azkar-box" id="azkarBox">جاري تحميل الأذكار...</div>
    </div>
  </div>

<script>
  // ======== إعدادات ========
  const city = "Jeddah";
  const method = 4;
  const iqamaOffset = 10; // دقائق بعد وقت الصلاة للإقامة
  const azkarBefore = 5;  // دقائق قبل الإقامة لعرض الأذكار

  // أذكار لكل صلاة (يمكن تعديل أو إضافة حسب الحاجة)
  const azkarByPrayer = {
    الفجر: [
      "أَصْبَحْنا وأصبح الملك لله",
      "اللهم بك أصبحنا وبك أمسينا",
      "اللهم أنت ربي لا إله إلا أنت"
    ],
    الظهر: [
      "اللهم إني أعوذ بك من الهم والحزن",
      "اللهم اجعل لي في كل خطوة بركة",
      "اللهم اجعل عملي خالصًا لوجهك الكريم"
    ],
    العصر: [
      "سبحان الله وبحمده",
      "اللهم اجعلني من عبادك الصالحين",
      "اللهم اغفر لنا ولوالدينا"
    ],
    المغرب: [
      "اللهم صل وسلم على نبينا محمد",
      "اللهم ارحم والديّ وارزقهم الجنة",
      "اللهم اجعل يومي هذا مباركًا"
    ],
    العشاء: [
      "أستغفر الله العظيم",
      "اللهم اجعل نومي هنيئًا ومباركًا",
      "اللهم اجعل قلبي مطمئنًا بك"
    ]
  };

  // أذكار عامة في الأوقات الأخرى
  const generalAzkar = [
    "سبحان الله وبحمده سبحان الله العظيم",
    "أستغفر الله العظيم وأتوب إليه",
    "اللهم صل وسلم على نبينا محمد",
    "لا إله إلا الله وحده لا شريك له له الملك وله الحمد وهو على كل شيء قدير"
  ];

  let azkarInterval = null;
  let currentIqamaTime = null;
  let currentPrayer = null;

  // ==== الساعة التناظرية ====
  function updateAnalogClock() {
    const now = new Date();

    const hour = now.getHours();
    const minute = now.getMinutes();
    const second = now.getSeconds();

    document.getElementById('hourHand').style.transform =
      `rotate(${(hour % 12) * 30 + minute * 0.5}deg)`;
    document.getElementById('minuteHand').style.transform =
      `rotate(${minute * 6}deg)`;
    document.getElementById('secondHand').style.transform =
      `rotate(${second * 6}deg)`;
  }

  // ==== الساعة الرقمية 12 ساعة مع AM/PM ====
  function updateDigitalClock() {
    const now = new Date();
    let hours = now.getHours();
    const minutes = now.getMinutes();
    const seconds = now.getSeconds();

    const ampm = hours >= 12 ? 'PM' : 'AM';
    document.getElementById('ampm').innerText = ampm;

    hours = hours % 12;
    if (hours === 0) hours = 12;

    const strTime = [
      hours.toString().padStart(2, '0'),
      minutes.toString().padStart(2, '0'),
      seconds.toString().padStart(2, '0')
    ].join(':');

    document.getElementById('digitalClock').innerText = strTime + ' ';
    document.getElementById('digitalClock').appendChild(document.getElementById('ampm'));
  }

  // ==== تحديث التاريخ الميلادي (للعرض فقط) ====
  function updateGregorianDate() {
    const now = new Date();
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    const formatted = now.toLocaleDateString('ar-EG', options);
    document.getElementById('gregorianDate').innerText = formatted;
  }

  // ==== تحويل الوقت لـ 12 ساعة مع حرف ص/م ====
  function format12(time24) {
    let [hour, minute] = time24.split(':');
    hour = parseInt(hour);
    let period = hour >= 12 ? 'م' : 'ص';
    hour = hour % 12;
    if (hour === 0) hour = 12;
    return `${hour}:${minute} ${period}`;
  }

  // ==== تحميل أوقات الصلاة ====
  async function loadPrayers() {
    const res = await fetch(`https://api.aladhan.com/v1/timingsByCity?city=${city}&country=SA&method=${method}`);
    const data = await res.json();
    const t = data.data.timings;

    const prayers = [
      { name: 'الفجر', time: t.Fajr },
      { name: 'الشروق', time: t.Sunrise },
      { name: 'الظهر', time: t.Dhuhr },
      { name: 'العصر', time: t.Asr },
      { name: 'المغرب', time: t.Maghrib },
      { name: 'العشاء', time: t.Isha }
    ];

    const container = document.getElementById('prayersList');
    container.innerHTML = '';
    const now = new Date();
    let nextPrayer = null;

    prayers.forEach(p => {
      const [h, m] = p.time.split(':');
      let pt = new Date();
      pt.setHours(h);
      pt.setMinutes(m);
      pt.setSeconds(0);

      if (!nextPrayer && pt > now) {
        nextPrayer = p.name;
        currentIqamaTime = new Date(pt.getTime() + iqamaOffset * 60000);
      }

      const row = document.createElement('div');
      row.className = 'row';
      row.innerHTML = `<div>${p.name}</div><div>${format12(p.time)}</div>`;
      container.appendChild(row);
    });

    if (nextPrayer) {
      highlightNext(nextPrayer);
      startCountdown(nextPrayer);
    }
  }

  // ==== تمييز الصلاة القادمة ====
  function highlightNext(name) {
    currentPrayer = name;
    document.querySelectorAll('.row').forEach(r => {
      if (r.innerText.includes(name)) {
        r.classList.add('next');
      } else {
        r.classList.remove('next');
      }
    });
  }

  // ==== عداد الإقامة + عرض الأذكار حسب الصلاة الحالية ====
  function startCountdown(prayerName) {
    setInterval(() => {
      const now = new Date();
      const diff = currentIqamaTime - now;

      if (diff <= 0) {
        hideAzkar();
        document.getElementById('iqamaCountdown').innerText = '';
        return;
      }

      const minutes = Math.floor(diff / 60000);
      const seconds = Math.floor(diff / 1000) % 60;

      document.getElementById('iqamaCountdown').innerText =
        `إقامة ${prayerName} بعد: ${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;

      if (minutes <= azkarBefore) {
        showAzkar(prayerName);
      } else {
        hideAzkar();
      }
    }, 1000);
  }

  // ==== عرض الأذكار حسب الصلاة الحالية ====
  function showAzkar(prayerName) {
    const azkarBox = document.getElementById('azkarBox');
    if (azkarInterval) return;

    let currentAzkarList = azkarByPrayer[prayerName] || generalAzkar;
    let idx = 0;

    azkarBox.innerText = currentAzkarList[idx];

    azkarInterval = setInterval(() => {
      idx++;
      if (idx >= currentAzkarList.length) idx = 0;
      azkarBox.innerText = currentAzkarList[idx];
    }, 15000);
  }

  // ==== إخفاء الأذكار ====
  function hideAzkar() {
    clearInterval(azkarInterval);
    azkarInterval = null;
    document.getElementById('azkarBox').innerText = 'الأذكار ستظهر قبل كل صلاة';
  }

  // ==== تحميل التاريخ الهجري ====
  async function loadHijri() {
