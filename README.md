<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>Star Bank</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
  margin:0;
  font-family:Tahoma, Arial;
  color:white;
  text-align:center;
  background:radial-gradient(circle at top, #0b1230, #000);
  min-height:100vh;
  display:flex;
  flex-direction:column;
}
.box{
  background:rgba(11,16,38,0.9);
  width:92%;
  max-width:460px;
  margin:15px auto;
  padding:20px;
  border-radius:18px;
}
.hidden{display:none}
.grid{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:15px;
}
.planetBtn{
  background:#11173a;
  padding:18px 6px;
  border-radius:15px;
  cursor:pointer;
  font-weight:bold;
}
input,select,button{
  width:100%;
  padding:12px;
  margin-top:10px;
  border-radius:10px;
  border:none;
  font-size:15px;
}
button{background:#4fd1ff;cursor:pointer}
.back{background:#444}
.money{font-size:28px;color:gold}
footer{
  margin-top:auto;
  padding:15px;
}
footer a{
  color:#4fd1ff;
  text-decoration:none;
  font-weight:bold;
}
</style>
</head>

<body>

<h1 id="title"></h1>

<div class="box" id="planetBox">
  <h3 id="choosePlanet"></h3>
  <div class="grid" id="planetGrid"></div>
</div>

<div class="box hidden" id="nameBox">
  <h3 id="planetTitle"></h3>
  <input id="userName">
  <button onclick="loginUser()" id="enterBtn"></button>
  <button class="back" onclick="goHome()" id="backBtn1"></button>
</div>

<div class="box hidden" id="balanceBox">
  <h3 id="welcomeText"></h3>
  <div class="money" id="showMoney"></div>
  <button class="back" onclick="goHome()" id="backBtn2"></button>
</div>

<div class="box">
  <button onclick="showAdmin()" id="adminBtn"></button>
</div>

<div class="box">
  <button onclick="toggleLanguage()" id="langBtn"></button>
</div>

<div class="box hidden" id="adminLogin">
  <input id="adminPass" type="password">
  <button onclick="loginAdmin()" id="adminLoginBtn"></button>
  <button class="back" onclick="goHome()" id="backBtn3"></button>
</div>

<div class="box hidden" id="adminPanel">
  <input id="adminName" placeholder="Name">
  <input id="adminMoney" type="number" placeholder="Money">
  <select id="adminPlanet"></select>
  <button onclick="saveAccount()" id="saveBtn"></button>
  <button class="back" onclick="goHome()" id="backBtn4"></button>
</div>

<footer>
  <a href="https://www.instagram.com/_jw16" target="_blank">karam almahayni</a>
</footer>

<script>
const SERVER_URL = "https://YOUR-SERVER.onrender.com"; // 🔴 غيّر هذا

let isArabic=true;
let selectedPlanet="";
const ADMIN_PASSWORD="1234";

const planets=[
 {ar:"☀️ الشمس",en:"☀️ Sun"},
 {ar:"🔅 عطارد",en:"🔅 Mercury"},
 {ar:"🌸 الزهرة",en:"🌸 Venus"},
 {ar:"🌍 الأرض",en:"🌍 Earth"},
 {ar:"🌑 المريخ",en:"🌑 Mars"},
 {ar:"🪨 المشتري",en:"🪨 Jupiter"},
 {ar:"🪐 زحل",en:"🪐 Saturn"},
 {ar:"🔷 أورانوس",en:"🔷 Uranus"},
 {ar:"💎 نبتون",en:"💎 Neptune"},
 {ar:"❄️ بلوتو",en:"❄️ Pluto"}
];

function applyLanguage(){
 document.documentElement.dir=isArabic?"rtl":"ltr";
 title.innerText=isArabic?"🏦 بنك ستار المجري":"🏦 Galactic Star Bank";
 choosePlanet.innerText=isArabic?"اختر كوكبك":"Choose Your Planet";
 enterBtn.innerText=isArabic?"دخول":"Enter";
 adminBtn.innerText=isArabic?"الإدارة":"Admin";
 adminLoginBtn.innerText=isArabic?"تسجيل الدخول":"Login";
 saveBtn.innerText=isArabic?"حفظ":"Save";
 backBtn1.innerText=backBtn2.innerText=backBtn3.innerText=backBtn4.innerText=isArabic?"عودة":"Back";
 langBtn.innerText=isArabic?"🇺🇸 English":"🇸🇾 العربية";

 planetGrid.innerHTML="";
 adminPlanet.innerHTML="";
 planets.forEach(p=>{
   let btn=document.createElement("div");
   btn.className="planetBtn";
   btn.innerText=isArabic?p.ar:p.en;
   btn.onclick=()=>choosePlanet(isArabic?p.ar:p.en);
   planetGrid.appendChild(btn);

   let opt=document.createElement("option");
   opt.text=isArabic?p.ar:p.en;
   adminPlanet.add(opt);
 });
}

function toggleLanguage(){isArabic=!isArabic;applyLanguage();}
function hideAll(){planetBox.classList.add("hidden");nameBox.classList.add("hidden");balanceBox.classList.add("hidden");adminLogin.classList.add("hidden");adminPanel.classList.add("hidden");}
function goHome(){hideAll();planetBox.classList.remove("hidden");}
function choosePlanet(p){selectedPlanet=p;hideAll();planetTitle.innerText=p;nameBox.classList.remove("hidden");}

async function loginUser(){
 let name=userName.value.trim();
 if(!name)return;

 const res = await fetch(`${SERVER_URL}/balance?name=${name}&planet=${selectedPlanet}`);
 const data = await res.json();

 showMoney.innerText = data.money + " ⭐";
 hideAll();balanceBox.classList.remove("hidden");
}

function showAdmin(){hideAll();adminLogin.classList.remove("hidden");}
function loginAdmin(){
 if(adminPass.value!==ADMIN_PASSWORD)return alert("Wrong password");
 hideAll();adminPanel.classList.remove("hidden");
}

async function saveAccount(){
 await fetch(`${SERVER_URL}/save`,{
   method:"POST",
   headers:{ "Content-Type":"application/json" },
   body:JSON.stringify({
     name: adminName.value,
     planet: adminPlanet.value,
     money: Number(adminMoney.value)
   })
 });
 alert("Saved");
}

applyLanguage();
</script>
</body>
</html>
