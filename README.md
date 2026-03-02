<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>مسجد الأمير فواز - جدة</title>

<style>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap');

body{
    margin:0;
    font-family:'Cairo',sans-serif;
    color:#D4AF37;
    overflow:hidden;
    background:url('https://images.unsplash.com/photo-1580418827493-f2b22c0a76cb?q=80&w=3840') no-repeat center center fixed;
    background-size:cover;
}

.overlay{
    position:fixed;
    top:0;left:0;
    width:100%;height:100%;
    background:rgba(0,0,0,0.75);
}

.content{
    position:relative;
    z-index:2;
}

.header{
    text-align:center;
    font-size:70px;
    padding-top:20px;
    font-weight:bold;
}

.clock-container{
    display:flex;
    justify-content:center;
    align-items:center;
    gap:50px;
    margin-bottom:20px;
}

.clock{
    font-size:60px;
}

.analog-clock{
    width:120px;
    height:120px;
    border:6px solid #D4AF37;
    border-radius:50%;
    position:relative;
    background:rgba(0,0,0,0.6);
}

.hand{
    position:absolute;
    width:50%;
    height:3px;
    background:#D4AF37;
    top:50%;
    transform-origin:100%;
    transform:rotate(90deg);
    transition: all 0.5s ease-in-out;
}

.hand.hour{
    height:5px;
}

.hand.minute{
    height:3px;
}

.hand.second{
    background:red;
    height:2px;
}

.date{
    text-align:center;
    font-size:28px;
    margin-bottom:20px;
}

.container{
    width:80%;
    margin:auto;
    font-size:45px;
}

.row{
    display:flex;
    justify-content:space-between;
    padding:15px 30px;
    border-bottom:1px solid rgba(212,175,55,0.4);
    transition:0.3s;
}

.next{
    background:#D4AF37;
    color:black;
    border-radius:10px;
    font-weight:bold;
}

.countdown{
    text-align:center;
    font-size:50px;
    margin-top:25px;
    font-weight:bold;
}

/* شاشة الأذكار */
.azkarScreen{
    position:fixed;
    top:0;left:0;
    width:100%;height:100%;
    background:rgba(0,0,0,0.95);
    color:#D4AF37;
    display:none;
    justify-content:center;
    align-items:center;
    text-align:center;
    font-size:55px;
    padding:60px;
    z-index:5;
    animation:fadeIn 1s ease;
}

@keyframes fadeIn{
    from{opacity:0;}
    to{opacity:1;}
}

.footer{
    position:absolute;
    bottom:15px;
    width:100%;
    text-align:center;
    font-size:20px;
    opacity:0.5;
}
</style>
</head>

<body>

<div class="overlay"></div>

<div class="content">
<div class="header">مسجد الأمير فواز</div>

<div class="clock-container">
    <div class="analog-clock">
        <div class="hand hour" id="hourHand"></div>
        <div class="hand minute" id="minuteHand"></div>
        <div class="hand second" id="secondHand"></div>
    </div>
    <div class="clock" id="clock"></div>
</div>

<div class="date" id="date"></div>
<div class="container" id="prayers"></div>
<div class="countdown" id="iqama"></div>
<div class="footer">نسخة VIP Ultra 4K مع ساعة برج</div>
</div>

<div class="azkarScreen" id="azkar"></div>

<script>
// ==== إعدادات ====
const iqamaOffset = 10;
const azkarBefore = 5;
const city = "Jeddah";
const method = 4;

// ==== قائمة الأذكار ====
const azkarList = [
"سبحان الله وبحمده سبحان الله العظيم",
"أستغفر الله العظيم وأتوب إليه",
"اللهم صل وسلم على نبينا محمد",
"لا إله إلا الله وحده لا شريك له له الملك وله الحمد وهو على كل شيء قدير",
"اللهم أعني على ذكرك وشكرك وحسن عبادتك",
"سبحان الله والحمد لله ولا إله إلا الله والله أكبر"
];

let azkarInterval=null;
let currentIqamaTime=null;

// ==== تحويل الوقت لـ 12 ساعة ====
function format12(time24){
    let [hour, minute] = time24.split(":");
    hour = parseInt(hour);
    let period = hour >= 12 ? "م" : "ص";
    hour = hour % 12;
    if(hour === 0) hour = 12;
    return `${hour}:${minute} ${period}`;
}

// ==== الساعة الرقمية ====
function updateClock(){
    const now = new Date();
    document.getElementById("clock").innerHTML = now.toLocaleTimeString('ar-EG');

    // ساعة العقارب
    const hour = now.getHours();
    const minute = now.getMinutes();
    const second = now.getSeconds();

    document.getElementById("hourHand").style.transform = `rotate(${(hour%12)*30 + minute*0.5}deg)`;
    document.getElementById("minuteHand").style.transform = `rotate(${minute*6}deg)`;
    document.getElementById("secondHand").style.transform = `rotate(${second*6}deg)`;
}
setInterval(updateClock,1000);
updateClock();

// ==== تحميل أوقات الصلاة ====
async function loadPrayers(){
    const res=await fetch(`https://api.aladhan.com/v1/timingsByCity?city=${city}&country=SA&method=${method}`);
    const data=await res.json();
    const t=data.data.timings;

    const prayers=[
        {name:"الفجر",time:t.Fajr},
        {name:"الظهر",time:t.Dhuhr},
        {name:"العصر",time:t.Asr},
        {name:"المغرب",time:t.Maghrib},
        {name:"العشاء",time:t.Isha}
    ];

    const container=document.getElementById("prayers");
    container.innerHTML="";
    const now=new Date();
    let nextPrayer=null;

    prayers.forEach(p=>{
        const [h,m]=p.time.split(":");
        let pt=new Date();
        pt.setHours(h);
        pt.setMinutes(m);
        pt.setSeconds(0);

        if(!nextPrayer && pt>now){
            nextPrayer=p.name;
            currentIqamaTime=new Date(pt.getTime()+iqamaOffset*60000);
        }

        const row=document.createElement("div");
        row.className="row";
        row.innerHTML=`<div>${p.name}</div><div>${format12(p.time)}</div>`;
        container.appendChild(row);
    });

    if(nextPrayer){
        highlightNext(nextPrayer);
        startCountdown(nextPrayer);
    }
}

// ==== تمييز الصلاة القادمة ====
function highlightNext(name){
    document.querySelectorAll(".row").forEach(r=>{
        if(r.innerText.includes(name)){
            r.classList.add("next");
        }
    });
}

// ==== عداد الإقامة + أظهار الأذكار ====
function startCountdown(prayerName){
    setInterval(()=>{
        const now=new Date();
        const diff=currentIqamaTime-now;

        if(diff<=0){
            hideAzkar();
            return;
        }

        const minutes=Math.floor(diff/60000);
        const seconds=Math.floor(diff/1000)%60;

        document.getElementById("iqama").innerHTML=
        `إقامة ${prayerName} بعد: ${minutes}:${seconds<10?"0":""}${seconds}`;

        if(minutes <= azkarBefore){
            showAzkar();
        }else{
            hideAzkar();
        }

    },1000);
}

// ==== أظهار شاشة الأذكار ====
function showAzkar(){
    const azkar=document.getElementById("azkar");
    if(azkar.style.display==="flex") return;

    azkar.style.display="flex";
    azkar.innerHTML=azkarList[0];

    azkarInterval=setInterval(()=>{
        azkar.innerHTML=azkarList[Math.floor(Math.random()*azkarList.length)];
    },15000);
}

// ==== اخفاء شاشة الأذكار ====
function hideAzkar(){
    const azkar=document.getElementById("azkar");
    azkar.style.display="none";
    clearInterval(azkarInterval);
}

// ==== التاريخ الهجري ====
async function loadHijri(){
    const res=await fetch(`https://api.aladhan.com/v1/gToH?date=${new Date().toLocaleDateString('en-GB')}`);
    const data=await res.json();
    const h=data.data.hijri;
    document.getElementById("date").innerHTML=
    `${h.weekday.ar} ${h.day} ${h.month.ar} ${h.year} هـ`;
}

// ==== تنفيذ التحميل ====
loadPrayers();
loadHijri();
</script>

</body>
</html>
