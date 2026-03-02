<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>شاشة مسجد - أوقات الصلاة والأذكار</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');

  body {
    margin: 0; padding: 0;
    font-family: 'Cairo', sans-serif;
    background: #004225 url('https://images.unsplash.com/photo-1580418827493-f2b22c0a76cb?q=80&w=3840') no-repeat center center fixed;
    background-size: cover;
    color: #fff;
  }
  .container {
    max-width: 1200px;
    margin: auto;
    background: #004225cc;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 0 40px #0009;
  }

  /* رأس الصفحة */
  header {
    display: flex;
    justify-content: space-between;
    padding: 5px 10px;
    border-bottom: 2px solid #aad34e;
    font-weight: 700;
  }
  .header-left {
    text-align: right;
  }
  .header-center {
    font-size: 20px;
    font-weight: 700;
  }
  .header-right {
    font-weight: 700;
  }

  /* المحتوى الرئيسي */
  .main-content {
    display: flex;
    gap: 40px;
    margin-top: 15px;
    align-items: center;
    justify-content: center;
  }

  /* الساعة التناظرية */
  .clock-wrapper {
    position: relative;
    width: 300px;
    height: 300px;
    background: #fff;
    border-radius: 50%;
    box-shadow: 0 0 0 10px #a89052 inset;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  /* إطار زخرفي */
  .clock-wrapper::before {
    content: '';
    position: absolute;
    width: 320px; height: 320px;
    border-radius: 50%;
    box-shadow:
      0 0 0 2px #a89052,
      0 0 20px 5px #a89052 inset;
    pointer-events: none;
  }

  /* شعار وسط الساعة */
  .clock-center-logo {
    position: absolute;
    width: 80px;
    height: 80px;
    background: url('https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Mosque_icon.svg/1024px-Mosque_icon.svg.png') no-repeat center center / contain;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    opacity: 0.6;
  }

  /* عقارب الساعة */
  .hand {
    position: absolute;
    background: #a60a0a;
    border-radius: 10px;
    top: 50%;
    left: 50%;
    transform-origin: 0% 50%;
    transform: rotate(90deg);
    transition: all 0.3s ease-in-out;
  }
  .hand.hour {
    width: 80px;
    height: 9px;
    background: #850000;
    z-index: 3;
  }
  .hand.minute {
    width: 110px;
    height: 6px;
    background: #c80000;
    z-index: 2;
  }
  .hand.second {
    width: 130px;
    height: 3px;
    background: #ff0000;
    z-index: 1;
  }
  .center-dot {
    position: absolute;
    width: 16px;
    height: 16px;
    background: #a60a0a;
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 8px #a60a0a;
    z-index: 4;
  }

  /* العمود الأيمن */
  .right-column {
    flex-grow: 1;
    max-width: 400px;
  }

  /* الساعة الرقمية */
  .digital-clock {
    font-family: monospace;
    font-weight: 700;
    font-size: 42px;
    color: #f00;
    text-align: right;
    margin-bottom: 8px;
    user-select: none;
  }
  .ampm {
    font-size: 24px;
    vertical-align: super;
    margin-right: 6px;
  }

  /* التاريخ */
  .date-hijri {
    font-size: 20px;
    font-weight: 700;
    color: #aad34e;
    margin-bottom: 15px;
    text-align: right;
    user-select: none;
  }

  /* الطقس */
  .weather {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    user-select: none;
  }
  .temp-day, .temp-night {
    text-align: center;
    font-weight: 700;
  }
  .temp-day {
    font-size: 36px;
    color: #fff;
  }
  .temp-night {
    font-size: 24px;
    color: #ddd;
  }
  .weather-icon {
    width: 40px;
    height: 40px;
    filter: drop-shadow(0 0 1px #000);
  }

  /* أوقات الصلاة */
  .prayer-times {
    text-align: right;
    font-weight: 700;
    font-size: 24px;
    line-height: 1.8;
    color: #ffc84d;
    user-select: none;
  }
  .prayer-times .prayer {
    display: flex;
    justify-content: space-between;
    padding: 3px 0;
  }
  .prayer-times .next {
    color: #fff;
    font-weight: 900;
  }

  /* عداد الإقامة الدائري */
  .iqama-counter-wrapper {
    position: relative;
    width: 120px;
    height: 120px;
    margin: 20px auto;
  }
  svg {
    transform: rotate(-90deg);
  }
  circle.bg {
    fill: none;
    stroke: #444;
    stroke-width: 10;
  }
  circle.progress {
    fill: none;
    stroke: #aad34e;
    stroke-width: 10;
    stroke-linecap: round;
    stroke-dasharray: 377;
    stroke-dashoffset: 377;
    transition: stroke-dashoffset 1s linear;
  }
  .iqama-counter-text {
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    font-size: 28px;
    font-weight: 700;
    color: #aad34e;
    user-select: none;
  }

  /* شريط نص أسفل */
  footer {
    background: #333a0b;
    padding: 12px 20px;
    margin-top: 20px;
    font-weight: 700;
    font-size: 20px;
    text-align: center;
    color: #d8e043;
    user-select: none;
  }

  /* شاشة الأذكار */
  .azkar-box {
    background: #1a3311;
    border-radius: 8px;
    padding: 12px;
    font-size: 20px;
    font-weight: 700;
    color: #b9d02b;
    margin-top: 10px;
    min-height: 90px;
    text-align: center;
    user-select: none;
  }
</style>
</head>
<body>

<div class="container">

  <header>
    <div class="header-right" id="hijriDate">الجمعة 12 محرم 1444 هـ</div>
    <div class="header-center" id="gregorianDate">Sun 21 Aug 2022</div>
    <div class="header-left">جامع الفرقان</div>
  </header>

  <div class="main-content">

    <!-- الساعة التناظرية -->
    <div class="clock-wrapper" aria-label="ساعة تناظرية">
      <div class="hand hour" id="hourHand"></div>
      <div class="hand minute" id="minuteHand"></div>
      <div class="hand second" id="secondHand"></div>
      <div class="center-dot"></div>
      <div class="clock-center-logo" title="شعار المسجد"></div>
    </div>

    <!-- العمود الأيمن -->
    <div class="right-column">

      <div class="digital-clock" id="digitalClock">
        04:42:13 <span class="ampm" id="ampm">AM</span>
      </div>

      <div class="weather" aria-label="درجة الحرارة">
        <div class="temp-day">
          <div id="tempDay">24°C</div>
        </div>
        <div>
          <img src="https://cdn-icons-png.flaticon.com/512/1163/1163624.png" alt="غيوم خفيفة" class="weather-icon" />
          <div class="temp-night" id="tempNight">21°C</div>
        </div>
      </div>

      <div class="prayer-times" id="prayerTimes">
        <!-- أوقات الصلاة تظهر هنا -->
      </div>

      <div class="iqama-counter-wrapper" aria-label="عداد الإقامة">
        <svg width="120" height="120">
          <circle class="bg" cx="60" cy="60" r="60" />
          <circle class="progress" cx="60" cy="60" r="60" />
        </svg>
        <div class="iqama-counter-text" id="iqamaCountdown">19</div>
      </div>

      <div class="azkar-box" id="azkarBox">
        الأذكار ستظهر هنا قبل كل صلاة
      </div>

    </div>

  </div>

  <footer>
    إن الصلاة كانت على المؤمنين كتابا موقوتا
  </footer>

</div>

<script>
  // إعدادات
  const city = "Jeddah";
  const method = 4; // طريقة حساب مكة المكرمة
  const iqamaOffsetMinutes = 19; // فترة الإقامة بالدقائق (مثال)
  const azkarBeforeIqama = 5; // دقائق عرض الأذكار قبل الإقامة

  // أذكار خاصة لكل صلاة
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
  const generalAzkar = [
    "سبحان الله وبحمده سبحان الله العظيم",
    "أستغفر الله العظيم وأتوب إليه",
    "اللهم صل وسلم على نبينا محمد",
    "لا إله إلا الله وحده لا شريك له له الملك وله الحمد وهو على كل شيء قدير"
  ];

  // عناصر HTML
  const hourHand = document.getElementById('hourHand');
  const minuteHand = document.getElementById('minuteHand');
  const secondHand = document.getElementById('secondHand');
  const digitalClock = document.getElementById('digitalClock');
  const ampmText = document.getElementById('ampm');
  const hijriDateElem = document.getElementById('hijriDate');
  const gregorianDateElem = document.getElementById('gregorianDate');
  const prayerTimesElem = document.getElementById('prayerTimes');
  const iqamaCountdownElem = document.getElementById('iqamaCountdown');
  const progressCircle = document.querySelector('circle.progress');
  const azkarBox = document.getElementById('azkarBox');
  const tempDayElem = document.getElementById('tempDay');
  const tempNightElem = document.getElementById('tempNight');

  // إعداد الوقت الحالي وتحديث الساعة التناظرية والرقمية
  function updateClocks() {
    const now = new Date();

    // تناظرية
    const hours = now.getHours();
    const minutes = now.getMinutes();
    const seconds = now.getSeconds();

    hourHand.style.transform = `rotate(${(hours % 12) * 30 + minutes * 0.5}deg)`;
    minuteHand.style.transform = `rotate(${minutes * 6}deg)`;
    secondHand.style.transform = `rotate(${seconds * 6}deg)`;

    // رقمية
    let displayHours = hours % 12;
    displayHours = displayHours === 0 ? 12 : displayHours;
    let ampm = hours >= 12 ? 'PM' : 'AM';

    let str = [
      displayHours.toString().padStart(2, '0'),
      minutes.toString().padStart(2, '0'),
      seconds.toString().padStart(2, '0')
    ].join(':');

    digitalClock.innerHTML = `${str} <span class="ampm">${ampm}</span>`;
  }

  // تحديث التاريخ الميلادي والهجري
  async function updateDates() {
    const now = new Date();
    // الميلادي
    const options = { weekday: 'short', year: 'numeric', month: 'short', day: 'numeric' };
    gregorianDateElem.textContent = now.toLocaleDateString('en-US', options);

    // هجري من API العadhan
    try {
      const res = await fetch(`https://api.aladhan.com/v1/gToH?date=${now.getDate()}-${now.getMonth() + 1}-${now.getFullYear()}`);
      const data = await res.json();
      if (data.code === 200) {
        const h = data.data.hijri;
        hijriDateElem.textContent = `${h.weekday.ar} ${h.day} ${h.month.ar} ${h.year} هـ`;
      }
    } catch {
      hijriDateElem.textContent = "خطأ في تحميل التاريخ الهجري";
    }
  }

  // تحميل أوقات الصلاة
  let prayers = [];
  let nextPrayer = null;
  let nextPrayerTime = null;
  async function loadPrayerTimes() {
    try {
      const res = await fetch(`https://api.aladhan.com/v1/timingsByCity?city=${city}&country=SA&method=${method}`);
      const data = await res.json();
      prayers = [
        { name: "الفجر", time: data.data.timings.Fajr },
        { name: "الشروق", time: data.data.timings.Sunrise },
        { name: "الظهر", time: data.data.timings.Dhuhr },
        { name: "العصر", time: data.data.timings.Asr },
        { name: "المغرب", time: data.data.timings.Maghrib },
        { name: "العشاء", time: data.data.timings.Isha }
      ];

      // عرض أوقات الصلاة
      const now = new Date();
      prayerTimesElem.innerHTML = '';
      nextPrayer = null;
      nextPrayerTime = null;

      prayers.forEach(p => {
        const [h, m] = p.time.split(':').map(Number);
        const prayerDate = new Date(now.getFullYear(), now.getMonth(), now.getDate(), h, m);
        const isNext = !nextPrayer && prayerDate > now;

        const div = document.createElement('div');
        div.className = 'prayer' + (isNext ? ' next' : '');
        div.innerHTML = `<span>${p.name}</span><span>${p.time}</span>`;
        prayerTimesElem.appendChild(div);

        if (isNext) {
          nextPrayer = p.name;
          nextPrayerTime = prayerDate;
        }
      });

      // بدء عداد الإقامة والاذكار
      if (nextPrayer && nextPrayerTime) {
        startIqamaCountdown(nextPrayerTime, nextPrayer);
      }
    } catch {
      prayerTimesElem.innerHTML = "خطأ في تحميل أوقات الصلاة";
