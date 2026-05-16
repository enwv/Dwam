<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سجل الحضور والغياب</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;900&family=Tajawal:wght@300;400;500;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #07090f;
  --bg2: #0d1117;
  --bg3: #161b22;
  --bg4: #21262d;
  --border: #30363d;
  --border2: #1a2238;
  --gold: #e3a008;
  --gold2: #fcd34d;
  --green: #3fb950;
  --green2: #56d364;
  --red: #f85149;
  --blue: #58a6ff;
  --text: #e6edf3;
  --muted: #7d8590;
  --r: 16px;
}
* { margin:0; padding:0; box-sizing:border-box; }
body {
  font-family: 'Cairo', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
}
body::before {
  content:''; position:fixed; inset:0;
  background:
    radial-gradient(ellipse 70% 50% at 20% 0%, rgba(88,166,255,.07) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 80% 100%, rgba(63,185,80,.06) 0%, transparent 60%);
  pointer-events:none; z-index:0;
}

.screen { display:none; min-height:100vh; position:relative; z-index:1; }
.screen.active { display:flex; flex-direction:column; align-items:center; justify-content:center; }

/* ── LANDING ── */
#landing { padding:40px 20px; gap:44px; }
.brand { text-align:center; }
.brand-icon {
  width:84px; height:84px;
  background:linear-gradient(135deg,#1f6feb,#388bfd);
  border-radius:22px;
  display:inline-flex; align-items:center; justify-content:center;
  font-size:40px; margin-bottom:18px;
  box-shadow:0 0 40px rgba(56,139,253,.3);
  animation:float 3s ease-in-out infinite;
}
@keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-7px)} }
.brand h1 {
  font-size:2.2rem; font-weight:900; margin-bottom:6px;
  background:linear-gradient(135deg,#fff,var(--blue));
  -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text;
}
.brand p { color:var(--muted); font-family:'Tajawal',sans-serif; }

.cards { display:flex; gap:18px; flex-wrap:wrap; justify-content:center; }
.lcard {
  background:var(--bg2); border:1px solid var(--border);
  border-radius:22px; padding:34px 28px;
  width:240px; text-align:center; cursor:pointer;
  transition:all .3s cubic-bezier(.34,1.56,.64,1);
  position:relative; overflow:hidden;
}
.lcard::before {
  content:''; position:absolute; inset:0; opacity:0;
  transition:opacity .3s; border-radius:22px;
}
.lcard.admin::before  { background:radial-gradient(circle at 50% 0%,rgba(227,160,8,.12),transparent 60%); }
.lcard.member::before { background:radial-gradient(circle at 50% 0%,rgba(63,185,80,.12),transparent 60%); }
.lcard:hover { transform:translateY(-7px); }
.lcard.admin:hover  { border-color:var(--gold);  box-shadow:0 20px 50px rgba(227,160,8,.2); }
.lcard.member:hover { border-color:var(--green); box-shadow:0 20px 50px rgba(63,185,80,.2); }
.lcard:hover::before { opacity:1; }
.lcard-icon { font-size:46px; margin-bottom:14px; display:block; }
.lcard h2 { font-size:1.3rem; font-weight:700; margin-bottom:6px; }
.lcard p  { font-size:.82rem; color:var(--muted); font-family:'Tajawal',sans-serif; }
.lcard-btn {
  margin-top:18px; padding:9px 24px;
  border:none; border-radius:30px;
  font-family:'Cairo',sans-serif; font-size:.88rem; font-weight:700;
  cursor:pointer; transition:all .2s;
}
.admin  .lcard-btn { background:linear-gradient(135deg,var(--gold),var(--gold2)); color:#000; }
.member .lcard-btn { background:linear-gradient(135deg,var(--green),var(--green2)); color:#000; }
.lcard-btn:hover { transform:scale(1.06); }

/* ── MODAL ── */
.overlay {
  display:none; position:fixed; inset:0;
  background:rgba(0,0,0,.75); backdrop-filter:blur(10px);
  z-index:200; align-items:center; justify-content:center;
}
.overlay.show { display:flex; }
.modal {
  background:var(--bg2); border:1px solid var(--border);
  border-radius:22px; padding:36px 32px;
  width:90%; max-width:380px; text-align:center;
  animation:slideUp .3s ease;
  max-height:90vh; overflow-y:auto;
}
@keyframes slideUp { from{transform:translateY(28px);opacity:0} to{transform:translateY(0);opacity:1} }
.modal-icon { font-size:44px; margin-bottom:12px; }
.modal h3 { font-size:1.3rem; margin-bottom:6px; }
.modal .sub { color:var(--muted); font-size:.82rem; font-family:'Tajawal',sans-serif; margin-bottom:20px; }

.member-select-wrap {
  display:grid; grid-template-columns:1fr 1fr; gap:9px; margin-bottom:18px;
}
.member-opt {
  padding:13px 10px;
  background:var(--bg3); border:1px solid var(--border);
  border-radius:11px; cursor:pointer;
  font-family:'Cairo',sans-serif; font-size:.92rem; font-weight:600;
  color:var(--text); transition:all .2s;
}
.member-opt:hover { border-color:var(--green); background:rgba(63,185,80,.08); color:var(--green); }
.member-opt.selected { border-color:var(--green); background:rgba(63,185,80,.12); color:var(--green); }

.inp-group { margin-bottom:13px; text-align:right; }
.inp-group label { display:block; font-size:.82rem; color:var(--muted); margin-bottom:5px; }
.inp-group input {
  width:100%; padding:11px 15px;
  background:var(--bg3); border:1px solid var(--border);
  border-radius:10px; color:var(--text);
  font-family:'Cairo',sans-serif; font-size:.95rem;
  outline:none; transition:border-color .2s; text-align:right;
}
.inp-group input:focus { border-color:var(--blue); }
.err { color:var(--red); font-size:.78rem; margin-bottom:10px; display:none; }
.err.show { display:block; }

.btn-main {
  width:100%; padding:12px;
  background:linear-gradient(135deg,var(--blue),#1f6feb);
  border:none; border-radius:11px; color:#fff;
  font-family:'Cairo',sans-serif; font-size:.95rem; font-weight:700;
  cursor:pointer; transition:all .2s; margin-bottom:10px;
}
.btn-main:hover { transform:scale(1.02); }
.btn-cancel {
  width:100%; padding:10px; background:none;
  border:1px solid var(--border); border-radius:11px;
  color:var(--muted); font-family:'Cairo',sans-serif;
  font-size:.88rem; cursor:pointer; transition:all .2s;
}
.btn-cancel:hover { border-color:var(--muted); color:var(--text); }

/* ── MEMBER PANEL ── */
#member-panel {
  padding:28px 18px; align-items:stretch;
  justify-content:flex-start;
  max-width:700px; margin:0 auto; width:100%;
}
.panel-top {
  display:flex; align-items:center; justify-content:space-between;
  margin-bottom:22px; flex-wrap:wrap; gap:12px;
}
.panel-top-left h2 { font-size:1.45rem; font-weight:900; }
.panel-top-left p  { color:var(--muted); font-size:.8rem; font-family:'Tajawal',sans-serif; margin-top:2px; }
.btn-out {
  padding:8px 18px; background:var(--bg3);
  border:1px solid var(--border); border-radius:30px;
  color:var(--muted); font-family:'Cairo',sans-serif;
  font-size:.82rem; cursor:pointer; transition:all .2s;
}
.btn-out:hover { border-color:var(--red); color:var(--red); }

/* today card */
.today-card {
  background:var(--bg2); border:1px solid var(--border);
  border-radius:var(--r); padding:20px 22px;
  display:flex; align-items:center; justify-content:space-between;
  margin-bottom:16px; flex-wrap:wrap; gap:14px;
}
.today-info .lbl { font-size:.76rem; color:var(--muted); margin-bottom:4px; }
.today-info .dval { font-size:1.05rem; font-weight:700; color:var(--gold2); }

.attend-btns { display:flex; gap:10px; flex-wrap:wrap; }
.btn-attend {
  padding:11px 24px;
  background:linear-gradient(135deg,var(--green),var(--green2));
  border:none; border-radius:30px; color:#000;
  font-family:'Cairo',sans-serif; font-size:.9rem; font-weight:700;
  cursor:pointer; transition:all .25s; white-space:nowrap;
}
.btn-attend:hover:not(:disabled) { transform:scale(1.05); box-shadow:0 8px 24px rgba(63,185,80,.3); }
.btn-attend:disabled {
  background:var(--bg4); color:var(--muted);
  border:1px solid var(--border); cursor:default; transform:none;
}
.btn-attend.done {
  background:rgba(63,185,80,.15)!important; color:var(--green)!important;
  border:1px solid rgba(63,185,80,.3)!important;
}

/* stats */
.stats-mini {
  display:grid; grid-template-columns:repeat(3,1fr); gap:12px; margin-bottom:18px;
}
.stat-box {
  background:var(--bg2); border:1px solid var(--border);
  border-radius:14px; padding:16px 12px; text-align:center;
}
.stat-box .n { font-size:1.8rem; font-weight:900; line-height:1; margin-bottom:4px; }
.stat-box .l { font-size:.72rem; color:var(--muted); font-family:'Tajawal',sans-serif; }
.stat-box.p .n { color:var(--green); }
.stat-box.a .n { color:var(--red); }
.stat-box.t .n { color:var(--gold2); }

/* hist tabs */
.hist-tabs { display:flex; gap:8px; margin-bottom:14px; flex-wrap:wrap; }
.hist-tab {
  padding:7px 18px; border-radius:30px; font-size:.85rem; font-weight:600;
  background:var(--bg3); border:1px solid var(--border);
  color:var(--muted); cursor:pointer; transition:all .2s;
  font-family:'Cairo',sans-serif;
}
.hist-tab.active { background:var(--blue); border-color:var(--blue); color:#fff; }

.section-title {
  font-size:.95rem; font-weight:700; color:var(--muted);
  margin-bottom:12px; display:flex; align-items:center; gap:8px;
}
.section-title span {
  font-size:.78rem; background:var(--bg3); border:1px solid var(--border);
  border-radius:20px; padding:2px 12px; color:var(--text);
}

/* days grid */
.days-grid {
  display:grid; grid-template-columns:repeat(auto-fill,minmax(150px,1fr)); gap:9px;
}
.day-row {
  background:var(--bg3); border:1px solid var(--border);
  border-radius:11px; padding:11px 13px;
  display:flex; align-items:center; justify-content:space-between;
  transition:background .15s;
}
.day-row:hover { background:var(--bg4); }
.day-row .dr-left .dr-date { font-size:.72rem; color:var(--muted); font-family:'Tajawal',sans-serif; }
.day-row .dr-left .dr-name { font-size:.86rem; font-weight:600; }
.day-row .dr-left .dr-time { font-size:.7rem; color:var(--muted); }
.day-row.present { border-color:rgba(63,185,80,.25); }
.day-row.absent  { border-color:rgba(248,81,73,.15); }

.badge-sm {
  font-size:.7rem; font-weight:700; padding:3px 10px;
  border-radius:20px; white-space:nowrap;
}
.badge-sm.p { background:rgba(63,185,80,.15); color:var(--green); border:1px solid rgba(63,185,80,.3); }
.badge-sm.a { background:rgba(248,81,73,.12); color:var(--red);   border:1px solid rgba(248,81,73,.25); }

.empty { text-align:center; padding:40px; color:var(--muted); font-family:'Tajawal',sans-serif; }
.empty big { font-size:2.5rem; display:block; margin-bottom:10px; }

/* ── ADMIN ── */
#admin-panel {
  padding:28px 18px; align-items:stretch;
  justify-content:flex-start;
  max-width:980px; margin:0 auto; width:100%;
}
.adm-members { display:grid; grid-template-columns:repeat(auto-fill,minmax(260px,1fr)); gap:16px; margin-bottom:22px; }
.adm-mc {
  background:var(--bg2); border:1px solid var(--border);
  border-radius:var(--r); padding:18px 20px;
}
.adm-mc-head { display:flex; align-items:center; justify-content:space-between; margin-bottom:12px; }
.adm-mc-name { font-size:1rem; font-weight:700; }
.adm-mc-rate { font-size:.76rem; padding:3px 12px; border-radius:20px; font-weight:700; }
.adm-mc-rate.hi { background:rgba(63,185,80,.15); color:var(--green); }
.adm-mc-rate.md { background:rgba(227,160,8,.15);  color:var(--gold2); }
.adm-mc-rate.lo { background:rgba(248,81,73,.15);  color:var(--red); }
.adm-bar-wrap { background:var(--bg4); border-radius:30px; height:5px; margin-bottom:11px; overflow:hidden; }
.adm-bar { height:100%; border-radius:30px; background:linear-gradient(90deg,var(--green),var(--green2)); transition:width .6s; }
.adm-mc-nums { display:flex; gap:12px; }
.adm-mc-num { font-size:.76rem; color:var(--muted); display:flex; align-items:center; gap:4px; }
.adm-mc-num b { color:var(--text); font-size:.85rem; }

.adm-filter {
  display:flex; gap:10px; flex-wrap:wrap; align-items:center; margin-bottom:14px;
}
.adm-filter label { font-size:.84rem; color:var(--muted); }
.adm-filter input[type=date] {
  padding:8px 13px; background:var(--bg3);
  border:1px solid var(--border); border-radius:10px;
  color:var(--text); font-family:'Cairo',sans-serif; font-size:.86rem; outline:none;
}
.adm-filter input[type=date]:focus { border-color:var(--gold); }
.btn-sm {
  padding:8px 16px; border-radius:10px;
  font-family:'Cairo',sans-serif; font-size:.84rem; cursor:pointer; transition:all .2s;
}
.btn-sm.gold { background:var(--gold); border:none; color:#000; font-weight:700; }
.btn-sm.gold:hover { background:var(--gold2); }
.btn-sm.plain { background:var(--bg3); border:1px solid var(--border); color:var(--text); }
.btn-sm.plain:hover { border-color:var(--gold); }
.btn-sm.danger { background:rgba(248,81,73,.1); border:1px solid rgba(248,81,73,.3); color:var(--red); }
.btn-sm.danger:hover { background:rgba(248,81,73,.2); }

.tbl-wrap {
  background:var(--bg2); border:1px solid var(--border);
  border-radius:var(--r); overflow:hidden; overflow-x:auto;
}
table { width:100%; border-collapse:collapse; font-size:.87rem; }
thead { background:var(--bg3); }
th { padding:12px 17px; text-align:right; font-weight:700; color:var(--gold2); border-bottom:1px solid var(--border); white-space:nowrap; }
td { padding:11px 17px; border-bottom:1px solid rgba(48,54,61,.5); text-align:right; }
tr:last-child td { border-bottom:none; }
tr:hover td { background:rgba(255,255,255,.02); }

.toast {
  position:fixed; bottom:24px; left:50%;
  transform:translateX(-50%) translateY(70px);
  background:var(--bg2); border:1px solid var(--border);
  border-radius:30px; padding:12px 24px;
  font-size:.88rem; z-index:300;
  transition:transform .4s cubic-bezier(.34,1.56,.64,1);
  box-shadow:0 10px 40px rgba(0,0,0,.5);
}
.toast.show { transform:translateX(-50%) translateY(0); }
.toast.ok  { border-color:var(--green); color:var(--green); }
.toast.err { border-color:var(--red);   color:var(--red); }

@media(max-width:520px) {
  .brand h1 { font-size:1.8rem; }
  .cards { flex-direction:column; align-items:center; }
  .lcard { width:100%; max-width:300px; }
  .days-grid { grid-template-columns:1fr 1fr; }
  .member-select-wrap { grid-template-columns:1fr 1fr; }
}
</style>
</head>
<body>

<!-- LANDING -->
<div id="landing" class="screen active">
  <div class="brand">
    <div class="brand-icon">📋</div>
    <h1>سجل الحضور</h1>
    <p>نظام متابعة الحضور والغياب</p>
  </div>
  <div class="cards">
    <div class="lcard admin" onclick="openModal('adminModal')">
      <span class="lcard-icon">👑</span>
      <h2>المدير</h2>
      <p>عرض إحصائيات وسجلات الجميع</p>
      <button class="lcard-btn">دخول المدير</button>
    </div>
    <div class="lcard member" onclick="openModal('memberModal')">
      <span class="lcard-icon">🧒</span>
      <h2>الأبناء</h2>
      <p>سجّل حضورك وشوف أيامك</p>
      <button class="lcard-btn">دخول الأبناء</button>
    </div>
  </div>
</div>

<!-- ADMIN MODAL -->
<div class="overlay" id="adminModal">
  <div class="modal">
    <div class="modal-icon">🔐</div>
    <h3>دخول المدير</h3>
    <p class="sub">أدخل كلمة المرور</p>
    <div class="inp-group">
      <label>كلمة المرور</label>
      <input type="password" id="adminPass" placeholder="••••••••" onkeydown="if(event.key==='Enter')checkAdmin()">
    </div>
    <div class="err" id="adminErr">❌ كلمة المرور غير صحيحة</div>
    <button class="btn-main" onclick="checkAdmin()">دخول</button>
    <button class="btn-cancel" onclick="closeModal('adminModal')">إلغاء</button>
  </div>
</div>

<!-- MEMBER MODAL -->
<div class="overlay" id="memberModal">
  <div class="modal">
    <div class="modal-icon">🧒</div>
    <h3>اختر حسابك</h3>
    <p class="sub">اختر اسمك ثم أدخل كلمة المرور</p>
    <div class="member-select-wrap" id="memberSelectWrap"></div>
    <div class="inp-group">
      <label>كلمة المرور</label>
      <input type="password" id="memberPass" placeholder="••••••••" onkeydown="if(event.key==='Enter')checkMember()">
    </div>
    <div class="err" id="memberErr">❌ كلمة المرور غير صحيحة</div>
    <button class="btn-main" onclick="checkMember()">دخول</button>
    <button class="btn-cancel" onclick="closeModal('memberModal')">إلغاء</button>
  </div>
</div>

<!-- MEMBER PANEL -->
<div id="member-panel" class="screen">
  <div class="panel-top">
    <div class="panel-top-left">
      <h2 id="mpTitle"></h2>
      <p id="mpSub">مرحباً! سجّل حضورك وتابع أيامك</p>
    </div>
    <button class="btn-out" onclick="logout()">🚪 خروج</button>
  </div>
  <div class="today-card" id="todayCard"></div>
  <div class="stats-mini" id="mpStats"></div>
  <div id="histTabsWrap" style="display:none">
    <div class="hist-tabs" id="histTabs"></div>
  </div>
  <div class="section-title">📅 سجل أيامك <span id="histCount">0</span></div>
  <div id="histContent"></div>
</div>

<!-- ADMIN PANEL -->
<div id="admin-panel" class="screen">
  <div class="panel-top">
    <div class="panel-top-left">
      <h2>👑 لوحة المدير</h2>
      <p style="color:var(--muted);font-size:.8rem;font-family:'Tajawal',sans-serif;">متابعة حضور وغياب الجميع</p>
    </div>
    <button class="btn-out" onclick="logout()">🚪 خروج</button>
  </div>
  <div class="stats-mini" id="admStats"></div>
  <div class="section-title" style="margin-bottom:14px">👥 نظرة عامة</div>
  <div class="adm-members" id="admMembers"></div>
  <div class="section-title" style="margin-bottom:12px">📋 السجل التفصيلي <span id="admRecCount">0</span></div>
  <div class="adm-filter">
    <label>📅 تصفية:</label>
    <input type="date" id="admDate">
    <button class="btn-sm gold" onclick="admFilter()">عرض</button>
    <button class="btn-sm plain" onclick="admShowAll()">الكل</button>
    <button class="btn-sm danger" onclick="admClear()">🗑️ مسح الكل</button>
  </div>
  <div class="tbl-wrap">
    <table>
      <thead><tr><th>#</th><th>الاسم</th><th>التاريخ</th><th>الوقت</th><th>الحالة</th></tr></thead>
      <tbody id="admBody"></tbody>
    </table>
    <div class="empty" id="admEmpty" style="display:none"><big>📭</big>لا توجد سجلات</div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ══ CONFIG ══
const ADMIN_PASS = "admin@2024";
const ACCOUNTS = [
  { id:"ayad_hour",  names:["اياد","حور"],  pass:"ayad2024"   },
  { id:"reem_lina",  names:["ريم","لينة"],   pass:"reem2024"   },
  { id:"sari",       names:["ساري"],          pass:"sari2024"   },
  { id:"emad",       names:["عماد"],          pass:"emad2024"   },
  { id:"khalid",     names:["خالد"],          pass:"khalid2024" },
  { id:"tamim",      names:["تميم"],          pass:"tamim2024"  },
];
const ALL_MEMBERS = ACCOUNTS.flatMap(a => a.names);

// ══ STATE ══
let currentAccount = null;
let selectedAccId  = null;
let activeHistName = null;

// ══ STORAGE ══
function getRecs() { return JSON.parse(localStorage.getItem('att_v3')||'[]'); }
function saveRecs(r){ localStorage.setItem('att_v3', JSON.stringify(r)); }

// ══ DATE ══
function todayKey() {
  const d = new Date();
  return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}`;
}
function pad(n){ return String(n).padStart(2,'0'); }
function nowTime(){ return new Date().toLocaleTimeString('ar-SA',{hour:'2-digit',minute:'2-digit'}); }
function todayAr(){ return new Date().toLocaleDateString('ar-SA',{weekday:'long',year:'numeric',month:'long',day:'numeric'}); }
function fmtFull(k){
  if(!k) return '-';
  return new Date(k+'T00:00:00').toLocaleDateString('ar-SA',{weekday:'short',year:'numeric',month:'long',day:'numeric'});
}
function fmtShort(k){
  if(!k) return '-';
  return new Date(k+'T00:00:00').toLocaleDateString('ar-SA',{weekday:'short',day:'numeric',month:'short'});
}
function getAllDays(){
  const recs = getRecs();
  if(!recs.length) return [];
  const sorted = [...new Set(recs.map(r=>r.date))].sort();
  const first = sorted[0];
  const days = [];
  let cur = new Date(first+'T00:00:00');
  const today = new Date(todayKey()+'T00:00:00');
  while(cur<=today){ days.push(cur.toISOString().slice(0,10)); cur.setDate(cur.getDate()+1); }
  return days;
}

// ══ UI ══
function showScreen(id){ document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active')); document.getElementById(id).classList.add('active'); window.scrollTo(0,0); }
function openModal(id){
  document.getElementById(id).classList.add('show');
  if(id==='memberModal') buildMemberSelect();
  setTimeout(()=>{ const i=document.querySelector(`#${id} input[type=password]`); if(i) i.focus(); },120);
}
function closeModal(id){
  document.getElementById(id).classList.remove('show');
  document.querySelectorAll(`#${id} .err`).forEach(e=>e.classList.remove('show'));
  document.querySelectorAll(`#${id} input`).forEach(e=>e.value='');
  selectedAccId=null;
  document.querySelectorAll('.member-opt').forEach(b=>b.classList.remove('selected'));
}
function toast(msg,type='ok'){
  const t=document.getElementById('toast');
  t.textContent=msg; t.className=`toast ${type} show`;
  setTimeout(()=>t.classList.remove('show'),2800);
}
function logout(){ currentAccount=null; showScreen('landing'); }
document.querySelectorAll('.overlay').forEach(o=>o.addEventListener('click',e=>{if(e.target===o)closeModal(o.id);}));

// ══ MEMBER SELECT ══
function buildMemberSelect(){
  const w=document.getElementById('memberSelectWrap');
  w.innerHTML='';
  ACCOUNTS.forEach(acc=>{
    const label=acc.names.join(' + ');
    const btn=document.createElement('button');
    btn.className='member-opt';
    btn.textContent=label;
    btn.onclick=()=>{
      document.querySelectorAll('.member-opt').forEach(b=>b.classList.remove('selected'));
      btn.classList.add('selected'); selectedAccId=acc.id;
    };
    w.appendChild(btn);
  });
}

// ══ AUTH ══
function checkAdmin(){
  if(document.getElementById('adminPass').value===ADMIN_PASS){
    closeModal('adminModal'); loadAdmin(); showScreen('admin-panel');
  } else {
    document.getElementById('adminErr').classList.add('show');
    document.getElementById('adminPass').value='';
  }
}
function checkMember(){
  if(!selectedAccId){ toast('اختر اسمك أولاً','err'); return; }
  const acc=ACCOUNTS.find(a=>a.id===selectedAccId);
  if(document.getElementById('memberPass').value===acc.pass){
    currentAccount=acc; closeModal('memberModal'); loadMember(); showScreen('member-panel');
  } else {
    document.getElementById('memberErr').classList.add('show');
    document.getElementById('memberPass').value='';
  }
}

// ══ MEMBER PANEL ══
function loadMember(){
  const acc=currentAccount;
  const shared=acc.names.length>1;
  const recs=getRecs();
  const today=todayKey();

  document.getElementById('mpTitle').textContent = shared ? `👨‍👧 ${acc.names.join(' و ')}` : `👤 ${acc.names[0]}`;

  // Today card
  const card=document.getElementById('todayCard');
  card.innerHTML=`
    <div class="today-info">
      <div class="lbl">📅 اليوم</div>
      <div class="dval">${todayAr()}</div>
    </div>
    <div class="attend-btns">
      ${acc.names.map(name=>{
        const done=recs.some(r=>r.date===today&&r.name===name);
        return `<button class="btn-attend${done?' done':''}" ${done?'disabled':''} onclick="attend('${name}')">
          ${done?`✅ ${name} حاضر`:(shared?`تسجيل ${name}`:'تسجيل حضوري')}
        </button>`;
      }).join('')}
    </div>`;

  // Stats (per active history person, default first)
  activeHistName = acc.names[0];
  buildMemberStats();
  buildHistTabs();
  buildHistContent();
}

function buildMemberStats(){
  const acc=currentAccount;
  const days=getAllDays();
  const total=days.length;
  const recs=getRecs();
  const name=activeHistName;
  const present=recs.filter(r=>r.name===name).length;
  const absent=Math.max(0,total-present);
  const pct=total?Math.round(present/total*100):0;
  document.getElementById('mpStats').innerHTML=`
    <div class="stat-box p"><div class="n">${present}</div><div class="l">أيام الحضور</div></div>
    <div class="stat-box a"><div class="n">${absent}</div><div class="l">أيام الغياب</div></div>
    <div class="stat-box t"><div class="n">${pct}%</div><div class="l">نسبة الحضور</div></div>`;
}

function buildHistTabs(){
  const acc=currentAccount;
  const shared=acc.names.length>1;
  const wrap=document.getElementById('histTabsWrap');
  const tabs=document.getElementById('histTabs');
  if(shared){
    wrap.style.display='block';
    tabs.innerHTML=acc.names.map(name=>
      `<button class="hist-tab${name===activeHistName?' active':''}" onclick="switchTab('${name}',this)">${name}</button>`
    ).join('');
  } else {
    wrap.style.display='none';
  }
}

function switchTab(name,btn){
  activeHistName=name;
  document.querySelectorAll('.hist-tab').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  buildMemberStats();
  buildHistContent();
}

function buildHistContent(){
  const recs=getRecs();
  const days=getAllDays().reverse(); // newest first
  document.getElementById('histCount').textContent=days.length;
  if(!days.length){
    document.getElementById('histContent').innerHTML='<div class="empty"><big>📭</big>لا توجد سجلات بعد<br><small>سجّل حضورك اليوم لتبدأ قائمتك!</small></div>';
    return;
  }
  const name=activeHistName;
  const myRecs=recs.filter(r=>r.name===name);
  const presentSet=new Set(myRecs.map(r=>r.date));
  const html=days.map(d=>{
    const present=presentSet.has(d);
    const rec=myRecs.find(r=>r.date===d);
    return `<div class="day-row ${present?'present':'absent'}">
      <div class="dr-left">
        <div class="dr-name">${fmtShort(d)}</div>
        ${present&&rec?`<div class="dr-time">⏰ ${rec.time}</div>`:''}
      </div>
      <span class="badge-sm ${present?'p':'a'}">${present?'✅ حاضر':'❌ غائب'}</span>
    </div>`;
  }).join('');
  document.getElementById('histContent').innerHTML=`<div class="days-grid">${html}</div>`;
}

function attend(name){
  const today=todayKey();
  const recs=getRecs();
  if(recs.some(r=>r.date===today&&r.name===name)){ toast(`${name} سجّل حضوره مسبقاً`,'err'); return; }
  recs.push({name,date:today,time:nowTime(),status:'present'});
  saveRecs(recs);
  toast(`✅ تم تسجيل حضور ${name}`,'ok');
  loadMember();
}

// ══ ADMIN ══
function loadAdmin(){
  const recs=getRecs();
  const today=todayKey();
  const days=getAllDays();

  const todayPresent=new Set(recs.filter(r=>r.date===today).map(r=>r.name)).size;
  document.getElementById('admStats').innerHTML=`
    <div class="stat-box t"><div class="n">${ALL_MEMBERS.length}</div><div class="l">إجمالي الأعضاء</div></div>
    <div class="stat-box p"><div class="n">${todayPresent}</div><div class="l">حاضرون اليوم</div></div>
    <div class="stat-box a"><div class="n">${ALL_MEMBERS.length-todayPresent}</div><div class="l">غائبون اليوم</div></div>`;

  document.getElementById('admMembers').innerHTML=ALL_MEMBERS.map(name=>{
    const present=recs.filter(r=>r.name===name).length;
    const total=days.length||1;
    const absent=Math.max(0,total-present);
    const pct=Math.round(present/total*100);
    const cls=pct>=80?'hi':pct>=50?'md':'lo';
    return `<div class="adm-mc">
      <div class="adm-mc-head">
        <span class="adm-mc-name">👤 ${name}</span>
        <span class="adm-mc-rate ${cls}">${pct}%</span>
      </div>
      <div class="adm-bar-wrap"><div class="adm-bar" style="width:${pct}%"></div></div>
      <div class="adm-mc-nums">
        <span class="adm-mc-num">✅ <b>${present}</b></span>
        <span class="adm-mc-num">❌ <b>${absent}</b></span>
        <span class="adm-mc-num">📅 <b>${total}</b></span>
      </div>
    </div>`;
  }).join('');

  document.getElementById('admDate').value=today;
  admRender(recs,today);
}

function admFilter(){ const d=document.getElementById('admDate').value; if(d) admRender(getRecs(),d); }
function admShowAll(){ document.getElementById('admDate').value=''; admRender(getRecs(),null); }

function admRender(recs,filterDate){
  let rows=[];
  if(filterDate){
    const presentNames=new Set(recs.filter(r=>r.date===filterDate).map(r=>r.name));
    ALL_MEMBERS.forEach(name=>{
      if(presentNames.has(name)){
        const rec=recs.find(r=>r.date===filterDate&&r.name===name);
        rows.push({name,date:filterDate,time:rec?.time||'-',present:true});
      } else {
        rows.push({name,date:filterDate,time:'-',present:false});
      }
    });
  } else {
    const days=getAllDays();
    days.slice().reverse().forEach(d=>{
      ALL_MEMBERS.forEach(name=>{
        const rec=recs.find(r=>r.date===d&&r.name===name);
        rows.push({name,date:d,time:rec?.time||'-',present:!!rec});
      });
    });
  }
  document.getElementById('admRecCount').textContent=rows.length;
  const empty=document.getElementById('admEmpty');
  const body=document.getElementById('admBody');
  if(!rows.length){ body.innerHTML=''; empty.style.display='block'; return; }
  empty.style.display='none';
  body.innerHTML=rows.map((r,i)=>`<tr>
    <td>${i+1}</td><td><strong>${r.name}</strong></td>
    <td>${fmtFull(r.date)}</td><td>${r.time}</td>
    <td><span class="badge-sm ${r.present?'p':'a'}">${r.present?'✅ حاضر':'❌ غائب'}</span></td>
  </tr>`).join('');
}

function admClear(){
  if(!confirm('هل أنت متأكد من مسح جميع بيانات الحضور؟')) return;
  saveRecs([]); loadAdmin(); toast('تم مسح جميع البيانات','err');
}
</script>
</body>
</html>
