<!DOCTYPE html>
<html lang="ar" dir="rtl">

<head>
<meta charset="UTF-8">
<title>كلية العلوم الصحية - جامعة دمشق</title>

<style>
:root{
--primary:#6366f1;
--secondary:#22c55e;
--bg:#f1f5f9;
--card:#ffffff;
--text:#1e293b;
}
.dark{
--bg:#0f172a;
--card:#1e293b;
--text:#f8fafc;
--primary:#38bdf8;
--secondary:#a78bfa;
}
body{
margin:0;
font-family:Tahoma;
background:var(--bg);
color:var(--text);
transition:.3s;
}
nav{
background:linear-gradient(90deg,var(--primary),var(--secondary));
color:white;
display:flex;
justify-content:space-between;
align-items:center;
padding:15px 40px;
}
nav ul{
display:flex;
gap:20px;
list-style:none;
}
nav li{
cursor:pointer;
}
header{
text-align:center;
padding:70px;
background:linear-gradient(120deg,var(--primary),var(--secondary));
color:white;
}
.bigBtn{
padding:25px 60px;
font-size:22px;
margin:20px;
border:none;
border-radius:12px;
background:var(--primary);
color:white;
cursor:pointer;
transition:.3s;
}
.bigBtn:hover{
transform:scale(1.08);
background:var(--secondary);
}
.page{
display:none;
padding:40px;
text-align:center;
}
.active{
display:block;
}
.card{
background:var(--card);
padding:20px;
margin:10px auto;
width:260px;
border-radius:12px;
box-shadow:0 5px 15px rgba(0,0,0,.1);
cursor:pointer;
transition:.3s;
}
.card:hover{
transform:translateY(-5px);
}
#lectures li{
background:var(--card);
margin:10px auto;
padding:15px;
width:60%;
border-radius:10px;
display:flex;
justify-content:space-between;
}
.panel{
background:var(--card);
padding:20px;
margin:20px auto;
width:60%;
border-radius:10px;
}
input,select{
padding:10px;
margin:5px;
}
footer{
text-align:center;
padding:20px;
font-weight:bold;
}
</style>
</head>

<body>

<nav>
<h3>كلية العلوم الصحية</h3>
<ul>
<li onclick="showPage('home')">الرئيسية</li>
<li onclick="showPage('semester1')">الفصل الأول</li>
<li onclick="showPage('semester2')">الفصل الثاني</li>
<li onclick="showPage('login')">غرفة المدير</li>
<li onclick="toggleTheme()">🌙</li>
</ul>
</nav>

<header>
<h1>جامعة دمشق</h1>
<p>قسم تقويم الكلام واللغة - السنة الأولى</p>
</header>

<!-- HOME -->
<section id="home" class="page active">
<h2>اختر الفصل</h2>
<button class="bigBtn" onclick="showPage('semester1')">الفصل الأول</button>
<button class="bigBtn" onclick="showPage('semester2')">الفصل الثاني</button>
</section>

<!-- SEMESTER 1 -->
<section id="semester1" class="page">
<h2>مواد الفصل الأول</h2>
<div id="subjects1"></div>
<h3>الأخبار</h3>
<div id="news1"></div>
</section>

<!-- SEMESTER 2 -->
<section id="semester2" class="page">
<h2>مواد الفصل الثاني</h2>
<div id="subjects2"></div>
<h3>الأخبار</h3>
<div id="news2"></div>
</section>

<!-- SUBJECT PAGE -->
<section id="subjectPage" class="page">
<button onclick="goBack()">⬅ رجوع</button>
<h2 id="subjectTitle"></h2>
<input placeholder="بحث عن محاضرة..." onkeyup="searchLecture(this.value)">
<ul id="lectures"></ul>
</section>

<!-- LOGIN -->
<section id="login" class="page">
<h3>دخول المدير</h3>
<input type="password" id="pass">
<button onclick="login()">دخول</button>
</section>

<!-- ADMIN / غرفة المدير -->
<section id="admin" class="page">
<h2>غرفة المدير</h2>

<div class="panel">
<h3>إضافة مادة</h3>
<input id="subjectName" placeholder="اسم المادة">
<select id="subjectSemester">
<option value="1">الفصل الأول</option>
<option value="2">الفصل الثاني</option>
</select>
<button onclick="addSubject()">إضافة</button>
</div>

<div class="panel">
<h3>إضافة محاضرة</h3>
<select id="lectureSubject"></select>
<input id="lectureTitle" placeholder="اسم المحاضرة">
<input id="lectureNumber" placeholder="رقم المحاضرة">
<input type="file" id="lectureFile">
<button onclick="addLecture()">إضافة</button>
</div>

<div class="panel">
<h3>إضافة خبر</h3>
<select id="newsSemester">
<option value="1">الفصل الأول</option>
<option value="2">الفصل الثاني</option>
</select>
<input id="newsText" placeholder="نص الخبر">
<button onclick="addNews()">إضافة</button>
</div>

<div class="panel">
<h3>حذف مادة</h3>
<select id="deleteSubjectList"></select>
<button onclick="deleteSubjectAdmin()">حذف المادة</button>
</div>

<div class="panel">
<h3>حذف محاضرة</h3>
<select id="deleteLectureList"></select>
<button onclick="deleteLectureAdmin()">حذف المحاضرة</button>
</div>

<div class="panel">
<h3>حذف خبر</h3>
<select id="deleteNewsList"></select>
<button onclick="deleteNewsAdmin()">حذف الخبر</button>
</div>

</section>

<footer>
✦ بإشراف صديقكم عمر زينو ✦
</footer>

<script>
let currentSubject=""

function showPage(id){
document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"))
document.getElementById(id).classList.add("active")
}
function toggleTheme(){document.body.classList.toggle("dark")}

/* LOGIN */
function login(){
if(document.getElementById("pass").value=="admin123"){
showPage("admin")
refreshDeleteLists()
}else{alert("كلمة السر خاطئة")}
}

/* SUBJECTS */
function loadSubjects(){
let subs=JSON.parse(localStorage.getItem("subjects"))||[]
let s1=document.getElementById("subjects1")
let s2=document.getElementById("subjects2")
s1.innerHTML=""; s2.innerHTML=""
subs.forEach(s=>{
let div=document.createElement("div")
div.className="card"
div.innerHTML=`<strong>${s.name}</strong>`
div.onclick=()=>openSubject(s.name)
if(s.semester=="1") s1.appendChild(div)
else s2.appendChild(div)
})
let select=document.getElementById("lectureSubject")
select.innerHTML=""
subs.forEach(s=>{
let opt=document.createElement("option")
opt.value=s.name
opt.textContent=s.name
select.appendChild(opt)
})
refreshDeleteLists()
}

/* ADD SUBJECT */
function addSubject(){
let name=document.getElementById("subjectName").value
let sem=document.getElementById("subjectSemester").value
let subs=JSON.parse(localStorage.getItem("subjects"))||[]
subs.push({name:name,semester:sem})
localStorage.setItem("subjects",JSON.stringify(subs))
loadSubjects()
}

/* SUBJECT PAGE */
function openSubject(name){
currentSubject=name
document.getElementById("subjectTitle").textContent=name
showPage("subjectPage")
loadLectures()
}

function goBack(){
let subs=JSON.parse(localStorage.getItem("subjects"))||[]
let sub=subs.find(s=>s.name==currentSubject)
if(sub.semester=="1") showPage("semester1")
else showPage("semester2")
}

/* LECTURES */
function addLecture(){
let subject=document.getElementById("lectureSubject").value
let title=document.getElementById("lectureTitle").value
let num=document.getElementById("lectureNumber").value
let file=document.getElementById("lectureFile").files[0]
if(!file){alert("اختر ملف المحاضرة"); return}
let reader=new FileReader()
reader.onload=function(){
let data=JSON.parse(localStorage.getItem("lectures"))||[]
data.push({subject:subject,title:title,number:num,file:reader.result,downloads:0})
localStorage.setItem("lectures",JSON.stringify(data))
alert("تمت إضافة المحاضرة")
loadLectures()
refreshDeleteLists()
}
reader.readAsDataURL(file)
}

function loadLectures(){
let data=JSON.parse(localStorage.getItem("lectures"))||[]
let list=document.getElementById("lectures")
list.innerHTML=""

let filtered = data.filter(l=>l.subject==currentSubject).sort((a,b)=>a.number-b.number)

if(filtered.length === 0){
let li = document.createElement("li")
li.textContent = "⚠ لم تتم إضافة محاضرات بعد"
li.style.fontStyle="italic"
li.style.color="gray"
list.appendChild(li)
return
}

filtered.forEach(l=>{
let li=document.createElement("li")
li.innerHTML=`محاضرة ${l.number} - ${l.title} 
<button onclick="downloadLecture('${l.title}')">تحميل (${l.downloads||0})</button>`
list.appendChild(li)
})
}

/* DOWNLOAD */
function downloadLecture(title){
let data=JSON.parse(localStorage.getItem("lectures"))||[]
let lec=data.find(l=>l.title==title)
lec.downloads++
localStorage.setItem("lectures",JSON.stringify(data))
window.open(lec.file)
loadLectures()
}

/* NEWS */
function addNews(){
let sem=document.getElementById("newsSemester").value
let text=document.getElementById("newsText").value
if(!text || text.trim()===""){ alert("لا يمكن إضافة خبر فارغ!"); return; }
let news=JSON.parse(localStorage.getItem("news"))||[]
news.push({semester:sem,text:text})
localStorage.setItem("news",JSON.stringify(news))
loadNews()
refreshDeleteLists()
}

function loadNews(){
let n1=document.getElementById("news1")
let n2=document.getElementById("news2")
if(!n1||!n2) return

let news = JSON.parse(localStorage.getItem("news")) || [];
news = news.filter(n => n.text && n.text.trim() !== "");
localStorage.setItem("news", JSON.stringify(news));

n1.innerHTML=""; n2.innerHTML=""

let news1 = news.filter(n=>n.semester=="1")
let news2 = news.filter(n=>n.semester=="2")

if(news1.length===0){
let div=document.createElement("div")
div.textContent="⚠ لم تتم إضافة أخبار بعد"
div.style.fontStyle="italic"
div.style.color="gray"
n1.appendChild(div)
}else{
news1.forEach(n=>{
let div=document.createElement("div")
div.className="card"
div.innerHTML=`${n.text}`
n1.appendChild(div)
})
}

if(news2.length===0){
let div=document.createElement("div")
div.textContent="⚠ لم تتم إضافة أخبار بعد"
div.style.fontStyle="italic"
div.style.color="gray"
n2.appendChild(div)
}else{
news2.forEach(n=>{
let div=document.createElement("div")
div.className="card"
div.innerHTML=`${n.text}`
n2.appendChild(div)
})
}
}

/* DELETE LISTS FOR ADMIN */
function refreshDeleteLists(){
let subs=JSON.parse(localStorage.getItem("subjects"))||[]
let lecs=JSON.parse(localStorage.getItem("lectures"))||[]
let news=JSON.parse(localStorage.getItem("news"))||[]
let sList=document.getElementById("deleteSubjectList")
let lList=document.getElementById("deleteLectureList")
let nList=document.getElementById("deleteNewsList")
if(!sList) return
sList.innerHTML=""; lList.innerHTML=""; nList.innerHTML=""
subs.forEach(s=>{let o=document.createElement("option"); o.value=s.name; o.textContent=s.name; sList.appendChild(o)})
lecs.forEach(l=>{let o=document.createElement("option"); o.value=l.title; o.textContent=l.title; lList.appendChild(o)})
news.forEach(n=>{let o=document.createElement("option"); o.value=n.text; o.textContent=n.text; nList.appendChild(o)})
}

/* DELETE ADMIN */
function deleteSubjectAdmin(){
let name=document.getElementById("deleteSubjectList").value
let subs=JSON.parse(localStorage.getItem("subjects"))||[]
subs=subs.filter(s=>s.name!=name)
localStorage.setItem("subjects",JSON.stringify(subs))
loadSubjects()
refreshDeleteLists()
}

function deleteLectureAdmin(){
let title=document.getElementById("deleteLectureList").value
let data=JSON.parse(localStorage.getItem("lectures"))||[]
data=data.filter(l=>l.title!=title)
localStorage.setItem("lectures",JSON.stringify(data))
refreshDeleteLists()
}

function deleteNewsAdmin(){
let text=document.getElementById("deleteNewsList").value
let news=JSON.parse(localStorage.getItem("news"))||[]
news=news.filter(n=>n.text!=text)
localStorage.setItem("news",JSON.stringify(news))
loadNews()
refreshDeleteLists()
}

/* SEARCH */
function searchLecture(txt){
let items=document.querySelectorAll("#lectures li")
items.forEach(i=>{i.style.display=i.textContent.includes(txt)?"flex":"none"})
}

loadSubjects()
loadNews()
refreshDeleteLists()
</script>

</body>
</html>
