[index(1).html](https://github.com/user-attachments/files/30457742/index.1.html)
# shiftsnap-demo
シフト自動作成・希望休管理デモ
<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>ShiftSnap Demo</title>
<style>
  :root{
    --bg:#f5f7fb;
    --panel:#ffffff;
    --text:#172033;
    --muted:#6b7280;
    --line:#e5e7eb;
    --blue:#2563eb;
    --blue2:#dbeafe;
    --green:#16a34a;
    --amber:#d97706;
    --red:#dc2626;
    --early:#e0f2fe;
    --day:#dcfce7;
    --late:#fef3c7;
    --off:#fee2e2;
    --shadow:0 10px 30px rgba(23,32,51,.08);
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI","Noto Sans JP",sans-serif;background:var(--bg);color:var(--text)}
  button,input,select{font:inherit}
  .app{max-width:1480px;margin:auto;padding:24px}
  .hero{display:flex;justify-content:space-between;gap:16px;align-items:flex-end;margin-bottom:18px}
  .hero h1{font-size:28px;margin:0 0 6px}
  .hero p{margin:0;color:var(--muted)}
  .badge{background:#eef2ff;color:#4338ca;border:1px solid #c7d2fe;padding:7px 10px;border-radius:999px;font-size:12px;font-weight:700}
  .toolbar,.panel{background:var(--panel);border:1px solid var(--line);border-radius:16px;box-shadow:var(--shadow)}
  .toolbar{padding:14px;display:flex;gap:10px;align-items:center;flex-wrap:wrap;margin-bottom:14px}
  .toolbar label{font-size:12px;color:var(--muted);font-weight:700}
  .field{display:flex;flex-direction:column;gap:4px}
  input[type="month"], input[type="number"], input[type="text"]{border:1px solid var(--line);border-radius:10px;padding:9px 10px;background:white}
  input[type="number"]{width:84px}
  .spacer{flex:1}
  .btn{border:0;border-radius:10px;padding:10px 14px;font-weight:800;cursor:pointer}
  .btn.primary{background:var(--blue);color:white}
  .btn.secondary{background:#eef2f7;color:#273244}
  .btn.danger{background:#fff1f2;color:#be123c;border:1px solid #fecdd3}
  .tabs{display:flex;gap:8px;margin:14px 0}
  .tab{border:1px solid var(--line);background:white;padding:10px 14px;border-radius:10px;cursor:pointer;font-weight:800;color:#475569}
  .tab.active{background:#111827;color:white;border-color:#111827}
  .panel{padding:16px;overflow:hidden}
  .panel-head{display:flex;justify-content:space-between;gap:12px;align-items:center;margin-bottom:12px}
  .panel-head h2{font-size:18px;margin:0}
  .hint{font-size:12px;color:var(--muted)}
  .staff-list{display:flex;gap:8px;flex-wrap:wrap;margin:10px 0 14px}
  .staff-chip{display:flex;align-items:center;gap:6px;background:#f8fafc;border:1px solid var(--line);padding:6px 8px;border-radius:999px}
  .staff-chip button{border:0;background:transparent;color:#94a3b8;cursor:pointer}
  .add-staff{display:flex;gap:8px}
  .table-wrap{overflow:auto;border:1px solid var(--line);border-radius:12px}
  table{border-collapse:separate;border-spacing:0;min-width:max-content;width:100%}
  th,td{border-right:1px solid var(--line);border-bottom:1px solid var(--line);text-align:center;padding:0}
  th{position:sticky;top:0;background:#f8fafc;z-index:3;font-size:12px}
  th:first-child, td:first-child{position:sticky;left:0;z-index:2;background:white}
  th:first-child{z-index:4;background:#f8fafc}
  .name-cell{min-width:120px;padding:10px 12px;text-align:left;font-weight:800}
  .day-head{width:42px;min-width:42px;padding:7px 2px}
  .day-head.weekend{background:#fff7ed}
  .day-num{display:block;font-size:12px;font-weight:900}
  .dow{display:block;font-size:10px;color:var(--muted)}
  .leave-cell,.shift-cell{width:42px;height:42px;cursor:pointer;font-size:11px;font-weight:900;user-select:none}
  .leave-cell:hover,.shift-cell:hover{outline:2px solid #93c5fd;outline-offset:-2px}
  .leave-cell.off{background:var(--off);color:#b91c1c}
  .leave-cell.ok{background:white;color:#cbd5e1}
  .shift-cell.early{background:var(--early);color:#0369a1}
  .shift-cell.day{background:var(--day);color:#166534}
  .shift-cell.late{background:var(--late);color:#92400e}
  .shift-cell.off{background:#f8fafc;color:#94a3b8}
  .shift-cell.requested{background:var(--off);color:#b91c1c}
  .legend{display:flex;gap:10px;flex-wrap:wrap;margin-top:12px;font-size:12px;color:var(--muted)}
  .legend span{display:flex;align-items:center;gap:6px}
  .swatch{width:12px;height:12px;border-radius:3px;border:1px solid var(--line)}
  .summary{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px;margin-top:14px}
  .card{background:#f8fafc;border:1px solid var(--line);border-radius:12px;padding:12px}
  .card strong{font-size:18px}
  .card small{display:block;color:var(--muted);margin-top:2px}
  .warning{margin-top:12px;padding:10px 12px;background:#fff7ed;border:1px solid #fed7aa;border-radius:10px;color:#9a3412;font-size:13px}
  .subsection{margin-top:22px}
  .subsection-title{display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:10px}
  .subsection-title h3{font-size:16px;margin:0}
  .daily-table th,.daily-table td{padding:9px 12px;min-width:120px;text-align:left}
  .daily-table th:first-child,.daily-table td:first-child{min-width:92px;text-align:center}
  .shift-pill{display:inline-block;margin:2px 4px 2px 0;padding:4px 8px;border-radius:999px;font-size:12px;font-weight:800}
  .shift-pill.early{background:var(--early);color:#0369a1}
  .shift-pill.day{background:var(--day);color:#166534}
  .shift-pill.late{background:var(--late);color:#92400e}
  .shift-pill.off{background:#f1f5f9;color:#64748b}
  .line-box{display:grid;grid-template-columns:minmax(0,1fr) auto;gap:10px;align-items:start}
  .line-box textarea{width:100%;min-height:220px;border:1px solid var(--line);border-radius:12px;padding:12px;resize:vertical;line-height:1.65;background:#fbfdff}
  .line-actions{display:flex;flex-direction:column;gap:8px}
  .toast{position:fixed;right:20px;bottom:20px;background:#111827;color:#fff;padding:10px 14px;border-radius:10px;font-weight:800;opacity:0;transform:translateY(8px);pointer-events:none;transition:.2s;z-index:99}
  .toast.show{opacity:1;transform:translateY(0)}
  .hidden{display:none}
  @media print{
    body{background:white}
    .hero,.toolbar,.tabs,#leavePanel,.panel-head .hint,.legend,.summary,#warnings,.line-section{display:none!important}
    .app{max-width:none;padding:0}
    #schedulePanel{display:block!important;border:0;box-shadow:none;padding:0}
    .panel-head{display:block}
    .panel-head h2{font-size:20px;margin-bottom:14px}
    #scheduleTable{display:none}
    .daily-table{font-size:11px}
    .table-wrap{border:0;overflow:visible}
    .daily-table th,.daily-table td{padding:6px 8px}
  }
  @media (max-width:780px){.app{padding:12px}.hero{align-items:flex-start;flex-direction:column}.spacer{display:none}}
</style>
</head>
<body>
<div class="app">
  <div class="hero">
    <div>
      <h1>ShiftSnap <span style="font-size:14px;color:#64748b">demo</span></h1>
      <p>希望休を入れて、ワンクリックで1か月分のシフトを自動作成。共有URLなら入力内容ごと引き継げます。</p>
    </div>
    <div class="badge">ブラウザだけで動くデモ</div>
  </div>

  <div class="toolbar">
    <div class="field">
      <label>対象月</label>
      <input id="month" type="month" value="2026-08">
    </div>
    <div class="field">
      <label>1日の必要人数</label>
      <input id="required" type="number" value="3" min="1" max="20">
    </div>
    <div class="field">
      <label>最大連勤</label>
      <input id="maxConsecutive" type="number" value="5" min="1" max="10">
    </div>
    <div class="spacer"></div>
    <button class="btn secondary" id="resetDemo">デモ初期化</button>
    <button class="btn primary" id="generate">シフト自動作成</button>
    <button class="btn secondary" id="copyLineTop">LINE文面コピー</button>
    <button class="btn secondary" id="copyShareUrl">共有URLコピー</button>
    <button class="btn secondary" id="printShift">表を印刷 / PDF</button>
    <button class="btn secondary" id="exportCsv">CSV出力</button>
  </div>

  <div class="tabs">
    <button class="tab active" data-tab="leave">① 休み希望入力</button>
    <button class="tab" data-tab="schedule">② 自動作成結果</button>
  </div>

  <section class="panel" id="leavePanel">
    <div class="panel-head">
      <div>
        <h2>休み希望入力</h2>
        <div class="hint">日付セルをクリックすると「出勤可 ↔ 希望休」を切り替えます。</div>
      </div>
      <div class="add-staff">
        <input id="newStaff" type="text" placeholder="スタッフ名">
        <button class="btn secondary" id="addStaff">追加</button>
      </div>
    </div>
    <div class="staff-list" id="staffList"></div>
    <div class="table-wrap">
      <table id="leaveTable"></table>
    </div>
    <div class="legend">
      <span><i class="swatch" style="background:white"></i>出勤可</span>
      <span><i class="swatch" style="background:var(--off)"></i>希望休</span>
    </div>
  </section>

  <section class="panel hidden" id="schedulePanel">
    <div class="panel-head">
      <div>
        <h2>自動作成結果</h2>
        <div class="hint">結果セルをクリックすると「休 → 早 → 日 → 遅」を手修正できます。</div>
      </div>
    </div>
    <div class="table-wrap">
      <table id="scheduleTable"></table>
    </div>
    <div class="legend">
      <span><i class="swatch" style="background:var(--early)"></i>早番</span>
      <span><i class="swatch" style="background:var(--day)"></i>日勤</span>
      <span><i class="swatch" style="background:var(--late)"></i>遅番</span>
      <span><i class="swatch" style="background:#f8fafc"></i>休み</span>
      <span><i class="swatch" style="background:var(--off)"></i>希望休</span>
    </div>
    <div id="warnings"></div>

    <div class="subsection">
      <div class="subsection-title">
        <div>
          <h3>日別シフト表</h3>
          <div class="hint">LINE共有や印刷ではこちらの表が見やすいです。</div>
        </div>
      </div>
      <div class="table-wrap">
        <table id="dailyTable" class="daily-table"></table>
      </div>
    </div>

    <div class="subsection line-section">
      <div class="subsection-title">
        <div>
          <h3>LINE送信用文面</h3>
          <div class="hint">自動生成された文面をそのままコピーできます。</div>
        </div>
      </div>
      <div class="line-box">
        <textarea id="lineText"></textarea>
        <div class="line-actions">
          <button class="btn primary" id="copyLine">文面をコピー</button>
          <button class="btn secondary" id="shareLine">共有する</button>
        </div>
      </div>
    </div>

    <div class="summary" id="summary"></div>
  </section>
</div>
<div id="toast" class="toast">コピーしました</div>

<script>
const state = {
  staff: ["ハッブス","坂本","大江","田中","佐藤","鈴木"],
  leave: {},
  schedule: {}
};

const monthEl = document.getElementById("month");
const requiredEl = document.getElementById("required");
const maxConsecutiveEl = document.getElementById("maxConsecutive");

function monthInfo(){
  const [y,m] = monthEl.value.split("-").map(Number);
  const days = new Date(y,m,0).getDate();
  return {y,m,days};
}
function key(name,day){ return `${name}__${day}`; }
function isLeave(name,day){ return !!state.leave[key(name,day)]; }
function getShift(name,day){ return state.schedule[key(name,day)] || "off"; }
function setShift(name,day,v){ state.schedule[key(name,day)] = v; }

function dowLabel(y,m,d){
  return ["日","月","火","水","木","金","土"][new Date(y,m-1,d).getDay()];
}
function isWeekend(y,m,d){
  const w = new Date(y,m-1,d).getDay();
  return w===0 || w===6;
}

function renderStaff(){
  const el = document.getElementById("staffList");
  el.innerHTML = "";
  state.staff.forEach(name=>{
    const chip = document.createElement("div");
    chip.className = "staff-chip";
    chip.innerHTML = `<span>${name}</span><button title="削除">×</button>`;
    chip.querySelector("button").onclick=()=>{
      if(state.staff.length<=1) return;
      state.staff = state.staff.filter(x=>x!==name);
      Object.keys(state.leave).filter(k=>k.startsWith(name+"__")).forEach(k=>delete state.leave[k]);
      Object.keys(state.schedule).filter(k=>k.startsWith(name+"__")).forEach(k=>delete state.schedule[k]);
      renderAll();
      syncHash();
    };
    el.appendChild(chip);
  });
}

function buildHeader(){
  const {y,m,days}=monthInfo();
  let s="<thead><tr><th class='name-cell'>スタッフ</th>";
  for(let d=1;d<=days;d++){
    s += `<th class="day-head ${isWeekend(y,m,d)?"weekend":""}"><span class="day-num">${d}</span><span class="dow">${dowLabel(y,m,d)}</span></th>`;
  }
  return s+"</tr></thead>";
}

function renderLeave(){
  const {days}=monthInfo();
  let s=buildHeader()+"<tbody>";
  state.staff.forEach(name=>{
    s+=`<tr><td class="name-cell">${name}</td>`;
    for(let d=1;d<=days;d++){
      const off=isLeave(name,d);
      s+=`<td class="leave-cell ${off?"off":"ok"}" data-name="${name}" data-day="${d}">${off?"休":"・"}</td>`;
    }
    s+="</tr>";
  });
  s+="</tbody>";
  const table=document.getElementById("leaveTable");
  table.innerHTML=s;
  table.querySelectorAll(".leave-cell").forEach(td=>{
    td.onclick=()=>{
      const k=key(td.dataset.name,td.dataset.day);
      state.leave[k]=!state.leave[k];
      renderLeave();
      syncHash();
    };
  });
}

function consecutiveBefore(name,day){
  let c=0;
  for(let d=day-1;d>=1;d--){
    if(getShift(name,d)!=="off" && !isLeave(name,d)) c++;
    else break;
  }
  return c;
}

function countsFor(name, uptoDay=999){
  let total=0, early=0, day=0, late=0;
  const {days}=monthInfo();
  for(let d=1;d<=Math.min(days,uptoDay);d++){
    const s=getShift(name,d);
    if(s!=="off"){total++; if(s==="early")early++; if(s==="day")day++; if(s==="late")late++;}
  }
  return {total,early,day,late};
}


function encodeStateToHash(){
  const payload = {
    month: monthEl.value,
    required: requiredEl.value,
    maxConsecutive: maxConsecutiveEl.value,
    staff: state.staff,
    leave: state.leave,
    schedule: state.schedule
  };
  const json = JSON.stringify(payload);
  const bytes = new TextEncoder().encode(json);
  let binary = "";
  bytes.forEach(b => binary += String.fromCharCode(b));
  return btoa(binary).replace(/\+/g,"-").replace(/\//g,"_").replace(/=+$/,"");
}

function decodeStateFromHash(hash){
  try{
    const raw = hash.replace(/^#s=/,"").replace(/-/g,"+").replace(/_/g,"/");
    const padded = raw + "=".repeat((4 - raw.length % 4) % 4);
    const binary = atob(padded);
    const bytes = Uint8Array.from(binary, c => c.charCodeAt(0));
    return JSON.parse(new TextDecoder().decode(bytes));
  }catch(e){
    return null;
  }
}

function applySharedState(payload){
  if(!payload) return false;
  if(payload.month) monthEl.value = payload.month;
  if(payload.required) requiredEl.value = payload.required;
  if(payload.maxConsecutive) maxConsecutiveEl.value = payload.maxConsecutive;
  if(Array.isArray(payload.staff) && payload.staff.length) state.staff = payload.staff;
  state.leave = payload.leave && typeof payload.leave==="object" ? payload.leave : {};
  state.schedule = payload.schedule && typeof payload.schedule==="object" ? payload.schedule : {};
  renderAll();
  return true;
}

function syncHash(){
  try{
    const hash = "#s=" + encodeStateToHash();
    history.replaceState(null,"",hash);
  }catch(e){}
}

async function copyShareUrl(){
  syncHash();
  const url = location.href;
  try{
    await navigator.clipboard.writeText(url);
    showToast("共有URLをコピーしました");
  }catch(e){
    const ta=document.createElement("textarea");
    ta.value=url;
    document.body.appendChild(ta);
    ta.select();
    document.execCommand("copy");
    ta.remove();
    showToast("共有URLをコピーしました");
  }
}

function generateSchedule(){
  state.schedule={};
  const {days}=monthInfo();
  const required=Math.min(parseInt(requiredEl.value||3), state.staff.length);
  const maxC=parseInt(maxConsecutiveEl.value||5);
  const shiftTypes=["early","day","late"];

  for(let d=1;d<=days;d++){
    let candidates = state.staff.filter(n=>!isLeave(n,d));

    candidates.sort((a,b)=>{
      const ca=countsFor(a,d-1), cb=countsFor(b,d-1);
      const consecA=consecutiveBefore(a,d), consecB=consecutiveBefore(b,d);
      const penaltyA = consecA>=maxC ? 1000 : 0;
      const penaltyB = consecB>=maxC ? 1000 : 0;
      return (ca.total+penaltyA) - (cb.total+penaltyB);
    });

    let chosen=candidates.slice(0,required);

    // If max-consecutive restriction makes staffing impossible, allow the least-loaded fallback.
    if(chosen.length<required){
      const extra=state.staff.filter(n=>!isLeave(n,d)&&!chosen.includes(n));
      chosen=chosen.concat(extra.slice(0, required-chosen.length));
    }

    chosen.forEach((name,i)=>{
      // Balance shift types by giving each person their least-used shift.
      const c=countsFor(name,d-1);
      const options=[
        ["early",c.early],
        ["day",c.day],
        ["late",c.late]
      ].sort((a,b)=>a[1]-b[1]);
      let selected=options[0][0];
      // spread the same day's shifts when exactly 3 are needed
      if(required>=3 && i<3) selected=shiftTypes[i];
      setShift(name,d,selected);
    });

    state.staff.filter(n=>!chosen.includes(n)).forEach(n=>setShift(n,d,"off"));
  }

  renderSchedule();
  syncHash();
  setTab("schedule");
}

function renderSchedule(){
  const {days}=monthInfo();
  let s=buildHeader()+"<tbody>";
  const label={early:"早",day:"日",late:"遅",off:"休"};
  state.staff.forEach(name=>{
    s+=`<tr><td class="name-cell">${name}</td>`;
    for(let d=1;d<=days;d++){
      const requested=isLeave(name,d);
      const sh=getShift(name,d);
      const cls=requested?"requested":sh;
      s+=`<td class="shift-cell ${cls}" data-name="${name}" data-day="${d}">${requested?"希望休":label[sh]}</td>`;
    }
    s+="</tr>";
  });
  s+="</tbody>";
  const table=document.getElementById("scheduleTable");
  table.innerHTML=s;
  table.querySelectorAll(".shift-cell").forEach(td=>{
    td.onclick=()=>{
      const name=td.dataset.name, d=parseInt(td.dataset.day);
      if(isLeave(name,d)) return;
      const seq=["off","early","day","late"];
      const cur=getShift(name,d);
      setShift(name,d,seq[(seq.indexOf(cur)+1)%seq.length]);
      renderSchedule();
      syncHash();
    };
  });
  renderDailyTable();
  updateLineText();
  renderSummary();
}


function formatJPDate(y,m,d){
  const dow=dowLabel(y,m,d);
  return `${m}/${d}(${dow})`;
}

function namesByShift(day, shift){
  return state.staff.filter(n=>!isLeave(n,day) && getShift(n,day)===shift);
}
function offNames(day){
  return state.staff.filter(n=>isLeave(n,day) || getShift(n,day)==="off");
}

function renderDailyTable(){
  const {y,m,days}=monthInfo();
  let s=`<thead><tr>
    <th>日付</th><th>早番</th><th>日勤</th><th>遅番</th><th>休み</th>
  </tr></thead><tbody>`;
  for(let d=1;d<=days;d++){
    const early=namesByShift(d,"early");
    const day=namesByShift(d,"day");
    const late=namesByShift(d,"late");
    const off=offNames(d);
    const pills=(arr,cls)=>arr.length ? arr.map(n=>`<span class="shift-pill ${cls}">${n}</span>`).join("") : "—";
    s+=`<tr>
      <td><strong>${formatJPDate(y,m,d)}</strong></td>
      <td>${pills(early,"early")}</td>
      <td>${pills(day,"day")}</td>
      <td>${pills(late,"late")}</td>
      <td>${pills(off,"off")}</td>
    </tr>`;
  }
  s+="</tbody>";
  document.getElementById("dailyTable").innerHTML=s;
}

function generateLineText(){
  const {y,m,days}=monthInfo();
  const lines=[];
  lines.push(`【${y}年${m}月 シフト共有】`);
  lines.push("");
  lines.push("お疲れさまです！");
  lines.push(`${m}月のシフトを共有します。`);
  lines.push("");
  for(let d=1;d<=days;d++){
    const e=namesByShift(d,"early");
    const da=namesByShift(d,"day");
    const l=namesByShift(d,"late");
    const parts=[];
    if(e.length) parts.push(`早：${e.join("・")}`);
    if(da.length) parts.push(`日：${da.join("・")}`);
    if(l.length) parts.push(`遅：${l.join("・")}`);
    lines.push(`${formatJPDate(y,m,d)}　${parts.length?parts.join(" / "):"出勤者なし"}`);
  }
  lines.push("");
  lines.push("各自ご確認をお願いします！");
  return lines.join("\n");
}

function updateLineText(){
  const el=document.getElementById("lineText");
  if(el) el.value=generateLineText();
}

function showToast(text="コピーしました"){
  const el=document.getElementById("toast");
  el.textContent=text;
  el.classList.add("show");
  setTimeout(()=>el.classList.remove("show"),1400);
}

async function copyLineText(){
  const text=generateLineText();
  const el=document.getElementById("lineText");
  if(el) el.value=text;
  try{
    await navigator.clipboard.writeText(text);
    showToast("LINE文面をコピーしました");
  }catch(e){
    if(el){
      el.focus();
      el.select();
      document.execCommand("copy");
      showToast("LINE文面をコピーしました");
    }
  }
}

async function shareLineText(){
  const text=generateLineText();
  if(navigator.share){
    try{
      await navigator.share({text});
      return;
    }catch(e){}
  }
  await copyLineText();
}

function renderSummary(){
  const {days}=monthInfo();
  const required=Math.min(parseInt(requiredEl.value||3), state.staff.length);
  const summary=document.getElementById("summary");
  summary.innerHTML="";
  state.staff.forEach(name=>{
    const c=countsFor(name,days);
    const requested = Array.from({length:days},(_,i)=>i+1).filter(d=>isLeave(name,d)).length;
    const card=document.createElement("div");
    card.className="card";
    card.innerHTML=`<strong>${c.total}日</strong><small>${name} / 出勤日数</small><small>早${c.early}・日${c.day}・遅${c.late} / 希望休${requested}</small>`;
    summary.appendChild(card);
  });

  const warningDays=[];
  for(let d=1;d<=days;d++){
    const staffed=state.staff.filter(n=>getShift(n,d)!=="off").length;
    if(staffed<required) warningDays.push(`${d}日(${staffed}/${required}名)`);
  }
  const w=document.getElementById("warnings");
  w.innerHTML = warningDays.length
    ? `<div class="warning">⚠ 必要人数を満たしていない日：${warningDays.join("、")}</div>`
    : `<div class="warning" style="background:#f0fdf4;border-color:#bbf7d0;color:#166534">✓ 全日、必要人数を満たしています。</div>`;
}

function setTab(tab){
  document.querySelectorAll(".tab").forEach(b=>b.classList.toggle("active",b.dataset.tab===tab));
  document.getElementById("leavePanel").classList.toggle("hidden",tab!=="leave");
  document.getElementById("schedulePanel").classList.toggle("hidden",tab!=="schedule");
}
document.querySelectorAll(".tab").forEach(b=>b.onclick=()=>setTab(b.dataset.tab));

document.getElementById("addStaff").onclick=()=>{
  const input=document.getElementById("newStaff");
  const name=input.value.trim();
  if(!name || state.staff.includes(name)) return;
  state.staff.push(name);
  input.value="";
  renderAll();
  syncHash();
};
document.getElementById("newStaff").addEventListener("keydown",e=>{if(e.key==="Enter")document.getElementById("addStaff").click();});
document.getElementById("generate").onclick=generateSchedule;
document.getElementById("copyLine").onclick=copyLineText;
document.getElementById("copyLineTop").onclick=copyLineText;
document.getElementById("copyShareUrl").onclick=copyShareUrl;
document.getElementById("shareLine").onclick=shareLineText;
document.getElementById("printShift").onclick=()=>{ setTab("schedule"); window.print(); };

function resetDemo(shouldSync=true){
  state.staff=["ハッブス","坂本","大江","田中","佐藤","鈴木"];
  state.leave={};
  state.schedule={};

  // Demo leave requests
  const demo={
    "ハッブス":[2,9,16,23,30],
    "坂本":[4,10,18,25],
    "大江":[1,7,14,21,28],
    "田中":[3,11,17,24,31],
    "佐藤":[6,13,20,27],
    "鈴木":[5,12,19,26]
  };
  Object.entries(demo).forEach(([name,days])=>days.forEach(d=>state.leave[key(name,d)]=true));
  renderAll();
  if(shouldSync) syncHash();
}
document.getElementById("resetDemo").onclick=()=>resetDemo(true);
monthEl.onchange=()=>{state.leave={};state.schedule={};renderAll();syncHash();};

document.getElementById("exportCsv").onclick=()=>{
  const {y,m,days}=monthInfo();
  const label={early:"早番",day:"日勤",late:"遅番",off:"休み"};
  let rows=[["スタッフ",...Array.from({length:days},(_,i)=>`${m}/${i+1}(${dowLabel(y,m,i+1)})`)]];
  state.staff.forEach(name=>{
    rows.push([name,...Array.from({length:days},(_,i)=>{
      const d=i+1;
      return isLeave(name,d)?"希望休":label[getShift(name,d)];
    })]);
  });
  rows.push([]);
  rows.push(["日別一覧","早番","日勤","遅番","休み"]);
  for(let d=1;d<=days;d++){
    rows.push([
      `${m}/${d}(${dowLabel(y,m,d)})`,
      namesByShift(d,"early").join("・"),
      namesByShift(d,"day").join("・"),
      namesByShift(d,"late").join("・"),
      offNames(d).join("・")
    ]);
  }
  const csv="\ufeff"+rows.map(r=>r.map(v=>`"${String(v).replace(/"/g,'""')}"`).join(",")).join("\r\n");
  const blob=new Blob([csv],{type:"text/csv;charset=utf-8"});
  const url=URL.createObjectURL(blob);
  const a=document.createElement("a");
  a.href=url;
  a.download=`shift_${y}-${String(m).padStart(2,"0")}.csv`;
  document.body.appendChild(a);a.click();a.remove();URL.revokeObjectURL(url);
};

function renderAll(){renderStaff();renderLeave();renderSchedule();}
const sharedPayload = location.hash.startsWith("#s=") ? decodeStateFromHash(location.hash) : null;
if(sharedPayload){
  applySharedState(sharedPayload);
}else{
  resetDemo(false);
}
</script>
</body>
</html>
