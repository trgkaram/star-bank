<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>Star Bank</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>

<style>
body{
  margin:0;
  font-family:Tahoma, Arial;
  background:#000814;
  color:white;
  text-align:center;
}
.box{
  background:#0b1230;
  margin:15px auto;
  padding:20px;
  width:92%;
  max-width:460px;
  border-radius:15px;
}
.hidden{display:none}
.grid{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:12px;
}
.planet{
  background:#111a40;
  padding:15px;
  border-radius:12px;
  cursor:pointer;
}
input,select,button{
  width:100%;
  padding:12px;
  margin-top:10px;
  border-radius:10px;
  border:none;
  font-size:14px;
}
button{background:#4fd1ff}
.back{background:#444}
.money{font-size:28px;color:gold}
footer{margin:20px}
footer a{color:#4fd1ff;text-decoration:none}
</style>
</head>

<body>

<h2 id="title"></h2>

<div class="box" id="planetBox">
  <h3 id="choosePlanet"></h3>
  <div class="grid" id="planets"></div>
</div>

<div class="box hidden" id="nameBox">
  <h3 id="planetTitle"></h3>
  <input id="userName">
  <button onclick="enterUser()" id="enterBtn"></button>
  <button class="back" onclick="goHome()" id="backBtn"></button>
</div>

<div class="box hidden" id="walletBox">
  <h3 id="welcome"></h3>
  <div class="money" id="stars"></div>
  <button class="back" onclick="goHome()" id="backBtn2"></button>
</div>

<div class="box">
  <button onclick="showAdmin()" id="adminBtn"></button>
</div>

<div class="box">
  <button onclick="toggleLang()" id="langBtn"></button>
</div>

<div class="box hidden" id="adminBox">
  <input id="adminName">
  <input id="adminStars" type="number">
  <select id="adminPlanet"></select>
  <button onclick="saveData()" id="saveBtn"></button>
  <button class="back" onclick="goHome()" id="backBtn3"></button>
</div>

<footer>
<a href="https://www.instagram.com/_jw16?igsh=bnp1cHJpMTI2ZDU0" target="_blank">
karam almahayni
</a>
</footer>

<script>
// 🔴 ضع بيانات Firebase هنا
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
};
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// 🌍 اللغة
let ar=true;
let selectedPlanet="";
const ADMIN1="karam";
const ADMIN2="admin";

const planets=[
{ar:"☀️ الشمس",en:"Sun"},
{ar:"🔅 عطارد",en:"Mercury"},
{ar:"🌸 الزهرة",en:"Venus"},
{ar:"🌍 الأرض",en:"Earth"},
{ar:"🌑 المريخ",en:"Mars"},
{ar:"🪨 المشتري",en:"Jupiter"},
{ar:"🪐 زحل",en:"Saturn"},
{ar:"🔷 أورانوس",en:"Uranus"},
{ar:"💎 نبتون",en:"Neptune"}
];

function applyLang(){
 title.innerText=ar?"🏦 بنك ستار":"🏦 Star Bank";
 choosePlanet.innerText=ar?"اختر كوكبك":"Choose planet";
 userName.placeholder=ar?"اكتب اسمك":"Your name";
 enterBtn.innerText=ar?"دخول":"Enter";
 adminBtn.innerText=ar?"الإدارة":"Admin";
 saveBtn.innerText=ar?"حفظ":"Save";
 backBtn.innerText=backBtn2.innerText=backBtn3.innerText=ar?"عودة":"Back";
 langBtn.innerText=ar?"🇺🇸 English":"🇸🇾 العربية";

 planetsDiv.innerHTML="";
 adminPlanet.innerHTML="";
 planets.forEach(p=>{
   let d=document.createElement("div");
   d.className="planet";
   d.innerText=ar?p.ar:p.en;
   d.onclick=()=>selectPlanet(ar?p.ar:p.en);
   planetsDiv.appendChild(d);

   let o=document.createElement("option");
   o.text=ar?p.ar:p.en;
   adminPlanet.add(o);
 });
}

const planetsDiv=document.getElementById("planets");

function toggleLang(){ar=!ar;applyLang();}
function hideAll(){
 planetBox.classList.add("hidden");
 nameBox.classList.add("hidden");
 walletBox.classList.add("hidden");
 adminBox.classList.add("hidden");
}
function goHome(){hideAll();planetBox.classList.remove("hidden");}
function selectPlanet(p){selectedPlanet=p;hideAll();planetTitle.innerText=p;nameBox.classList.remove("hidden");}

async function enterUser(){
 const name=userName.value.trim();
 if(!name)return;
 const id=name+"_"+selectedPlanet;
 const doc=await db.collection("accounts").doc(id).get();
 const starsCount=doc.exists?doc.data().stars:0;
 stars.innerText=starsCount+" ⭐";
 welcome.innerText=(ar?"مرحبًا ":"Welcome ")+name;
 hideAll();walletBox.classList.remove("hidden");
}

function showAdmin(){
 const name=prompt(ar?"اسم الأدمن":"Admin name");
 if(name!==ADMIN1 && name!==ADMIN2) return alert("Access denied");
 adminName.value=name;
 hideAll();adminBox.classList.remove("hidden");
}

function saveData(){
 const id=adminName.value+"_"+adminPlanet.value;
 db.collection("accounts").doc(id).set({
   stars:Number(adminStars.value)
 });
 alert(ar?"تم الحفظ":"Saved");
}

applyLang();
</script>

</body>
</html>
