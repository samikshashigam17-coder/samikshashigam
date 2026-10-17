<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Electrical Safety Complaint Portal</title>
<style>
:root{--bg:#f3f5f0;--surface:#fff;--ink:#1d2a22;--muted:#5d6b62;--line:#d7ddd3;--brand:#1f5c46;--brand-ink:#fff;--open:#b4541a;--inv:#2a5d9f;--res:#2e7d4f;--high:#b3261e;--med:#b97a00;--low:#4f7a5a;
box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#121a15;--surface:#1b261f;--ink:#e8efe9;--muted:#9fb0a5;--line:#2e3d33;--brand:#5fbf95;--brand-ink:#0c1a12;--open:#e8975c;--inv:#7aa9e6;--res:#6fcf97;--high:#ff8a80;--med:#e7b04a;--low:#8cc79a}}
:root[data-theme="dark"]{--bg:#121a15;--surface:#1b261f;--ink:#e8efe9;--muted:#9fb0a5;--line:#2e3d33;--brand:#5fbf95;--brand-ink:#0c1a12;--open:#e8975c;--inv:#7aa9e6;--res:#6fcf97;--high:#ff8a80;--med:#e7b04a;--low:#8cc79a}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.5 "Source Sans 3",system-ui,-apple-system,"Segoe UI",sans-serif}
header{background:var(--brand);color:var(--brand-ink);padding:14px 20px}
header h1{margin:0;font:700 1.25rem/1.2 Georgia,"Times New Roman",serif}
nav{display:flex;gap:4px;flex-wrap:wrap;margin-top:10px}
nav button{background:transparent;border:1px solid transparent;color:inherit;padding:8px 14px;border-radius:6px;font:inherit;cursor:pointer;opacity:.85}
nav button[aria-current="page"]{background:var(--brand-ink);color:var(--brand);opacity:1;font-weight:600}
button:focus-visible,input:focus-visible,select:focus-visible,textarea:focus-visible{outline:3px solid #e0a800;outline-offset:2px}
main{max-width:980px;margin:0 auto;padding:20px}
h2{font:700 1.5rem/1.2 Georgia,serif;margin:0 0 16px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px;margin-bottom:24px}
.stat{background:var(--surface);border:1px solid var(--line);border-radius:8px;padding:14px}
.stat b{display:block;font:700 2rem/1 Georgia,serif}
.stat span{color:var(--muted);font-size:.9rem}
.stat.alert b{color:var(--high)}
.panel{background:var(--surface);border:1px solid var(--line);border-radius:8px;padding:16px;margin-bottom:16px}
.bar{display:flex;align-items:center;gap:10px;margin:8px 0}
.bar i{display:block;height:14px;border-radius:3px}
.bar label{width:70px}
form label{display:block;font-weight:600;margin:12px 0 4px}
form .opt{font-weight:400;color:var(--muted)}
input,select,textarea{width:100%;padding:9px 10px;border:1px solid var(--line);border-radius:6px;background:var(--bg);color:var(--ink);font:inherit}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
@media(max-width:560px){.row2{grid-template-columns:1fr}}
.btn{background:var(--brand);color:var(--brand-ink);border:0;padding:10px 18px;border-radius:6px;font:600 1rem inherit;cursor:pointer;margin-top:16px}
.btn.sec{background:transparent;color:var(--ink);border:1px solid var(--line)}
.btn.sm{padding:5px 10px;margin:0;font-size:.85rem}
.filters{display:grid;grid-template-columns:2fr 1fr 1fr;gap:10px;margin-bottom:14px}
@media(max-width:560px){.filters{grid-template-columns:1fr}}
.item{display:block;width:100%;text-align:left;background:var(--surface);border:1px solid var(--line);border-radius:8px;padding:12px 14px;margin-bottom:8px;color:inherit;font:inherit;cursor:pointer}
.item:hover{border-color:var(--brand)}
.item .t{font-weight:600}
.item .m{color:var(--muted);font-size:.9rem;margin-top:2px}
.tag{display:inline-block;padding:1px 8px;border-radius:99px;font-size:.8rem;font-weight:600;border:1px solid currentColor;margin-right:6px}
.s-Open{color:var(--open)}.s-Investigating{color:var(--inv)}.s-Resolved{color:var(--res)}
.v-High{color:var(--high)}.v-Medium{color:var(--med)}.v-Low{color:var(--low)}
.over{color:var(--high);font-weight:600}
dl{display:grid;grid-template-columns:110px 1fr;gap:6px 12px;margin:12px 0}
dt{color:var(--muted)}dd{margin:0}
.act{display:flex;justify-content:space-between;align-items:center;gap:10px;border-top:1px solid var(--line);padding:10px 0;flex-wrap:wrap}
.empty{color:var(--muted);padding:20px;text-align:center;border:1px dashed var(--line);border-radius:8px}
.ok{background:var(--surface);border-left:4px solid var(--res);padding:12px 14px;margin-bottom:16px;border-radius:4px}
.hide{display:none}
</style>
</head>
<body>
<header>
<h1>Electrical Safety Complaint Portal</h1>
<nav id="nav"></nav>
</header>
<main id="app"></main>
<script>
(function(){
var KEY="es-complaints-v1",STATES=["Open","Investigating","Resolved"],SEV=["High","Medium","Low"],TYPES=["Exposed wiring","Sparking or short circuit","Overloaded socket or board","Faulty earthing","Damaged switchboard","Water near electrics","Fire risk","Other"];
var view="dash",sel=null,flt={q:"",st:"",sv:""},notice="";
function today(){return new Date().toISOString().slice(0,10)}
function addDays(n){var d=new Date();d.setDate(d.getDate()+n);return d.toISOString().slice(0,10)}
function seed(){return [
{id:"ES-001",title:"Exposed wires hanging in the corridor",type:"Exposed wiring",severity:"High",location:"Block B, 2nd floor corridor",desc:"A cable has lost its insulation near the ceiling light and hangs at head height. Children use this corridor.",name:"",contact:"",status:"Investigating",created:addDays(-9),actions:[{id:1,text:"Switch off the circuit and tape off the area",who:"Electrician",due:addDays(-2),state:"In Progress"}]},
{id:"ES-002",title:"Sparks from the main switchboard",type:"Damaged switchboard",severity:"High",location:"Ground floor meter room",desc:"Sparking and a burning smell when the pump motor starts.",name:"R. Patel",contact:"",status:"Open",created:addDays(-4),actions:[]},
{id:"ES-003",title:"Loose socket in the office",type:"Overloaded socket or board",severity:"Low",location:"Office 3, near the window",desc:"The socket is loose and gets warm with the heater plugged in.",name:"",contact:"",status:"Resolved",created:addDays(-20),actions:[{id:1,text:"Replace the socket and check the load",who:"Maintenance",due:addDays(-12),state:"Done"}]}]}
function load(){try{var s=localStorage.getItem(KEY);if(s)return JSON.parse(s)}catch(e){}return seed()}
var data=load();
function save(){try{localStorage.setItem(KEY,JSON.stringify(data))}catch(e){}}
function esc(s){return String(s==null?"":s).replace(/[&<>"]/g,function(c){return{"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]})}
function nextId(){var m=0;data.forEach(function(c){var n=parseInt(c.id.slice(3),10);if(n>m)m=n});return "ES-"+String(m+1).padStart(3,"0")}
function isOver(a){return a.state!=="Done"&&a.due&&a.due<today()}
function overdueCount(){var n=0;data.forEach(function(c){c.actions.forEach(function(a){if(isOver(a))n++})});return n}
function nav(){var items=[["dash","Dashboard"],["report","Report an electrical hazard"],["list","Complaints"]];
document.getElementById("nav").innerHTML=items.map(function(i){return '<button data-v="'+i[0]+'"'+(view===i[0]||(view==="detail"&&i[0]==="list")?' aria-current="page"':'')+'>'+i[1]+'</button>'}).join("");}
function dash(){var c=data,cnt=function(s){return c.filter(function(x){return x.status===s}).length},od=overdueCount();
var max=Math.max(1,Math.max.apply(null,SEV.map(function(s){return c.filter(function(x){return x.severity===s}).length})));
var col={High:"var(--high)",Medium:"var(--med)",Low:"var(--low)"};
return '<h2>Dashboard</h2><div class="grid"><div class="stat"><b>'+c.length+'</b><span>Total complaints</span></div><div class="stat"><b>'+cnt("Open")+'</b><span>Open</span></div><div class="stat"><b>'+cnt("Investigating")+'</b><span>Investigating</span></div><div class="stat"><b>'+cnt("Resolved")+'</b><span>Resolved</span></div><div class="stat'+(od?' alert':'')+'"><b>'+od+'</b><span>Overdue actions</span></div></div><div class="panel"><h3 style="margin:0 0 8px">Complaints by severity</h3>'+SEV.map(function(s){var n=c.filter(function(x){return x.severity===s}).length;return '<div class="bar"><label>'+s+'</label><i style="width:'+Math.round(n/max*60)+'%;min-width:4px;background:'+col[s]+'"></i><span>'+n+'</span></div>'}).join("")+'</div>';}
function report(){var o=function(a){return a.map(function(x){return '<option>'+x+'</option>'}).join("")};
return '<h2>Report an electrical hazard</h2>'+(notice?'<div class="ok" role="status">'+notice+'</div>':'')+'<form id="f" class="panel"><label for="t">Title</label><input id="t" required maxlength="120"><div class="row2"><div><label for="ty">Electrical hazard type</label><select id="ty">'+o(TYPES)+'</select></div><div><label for="sv">Severity</label><select id="sv">'+o(SEV)+'</select></div></div><label for="lo">Location</label><input id="lo" required><label for="de">Description</label><textarea id="de" rows="4" required></textarea><div class="row2"><div><label for="nm">Name <span class="opt">(optional)</span></label><input id="nm"></div><div><label for="co">Contact <span class="opt">(optional)</span></label><input id="co"></div></div><button class="btn" type="submit">Submit complaint</button></form>';}
function list(){var r=data.filter(function(c){var q=flt.q.toLowerCase();return(!flt.st||c.status===flt.st)&&(!flt.sv||c.severity===flt.sv)&&(!q||(c.id+c.title+c.location+c.desc).toLowerCase().indexOf(q)>-1)});
var so=function(a,v){return '<option value="">'+v+'</option>'+a.map(function(x){return '<option'+(x===(v==="All statuses"?flt.st:flt.sv)?' selected':'')+'>'+x+'</option>'}).join("")};
return '<h2>Complaints</h2><div class="filters"><input id="q" type="search" placeholder="Search by ID, title or location" aria-label="Search" value="'+esc(flt.q)+'"><select id="fs" aria-label="Status">'+so(STATES,"All statuses")+'</select><select id="fv" aria-label="Severity">'+so(SEV,"All severities")+'</select></div><div id="res">'+rows(r)+'</div>';}
function rows(r){if(!r.length)return '<div class="empty">No complaints match. Clear the filters or report a new hazard.</div>';
return r.map(function(c){return '<button class="item" data-id="'+c.id+'"><div class="t">'+c.id+' · '+esc(c.title)+'</div><div class="m"><span class="tag s-'+c.status+'">'+c.status+'</span><span class="tag v-'+c.severity+'">'+c.severity+'</span>'+esc(c.location)+'</div></button>'}).join("");}
function detail(){var c=data.find(function(x){return x.id===sel});if(!c)return list();
var acts=c.actions.length?c.actions.map(function(a){var nx=a.state==="Pending"?"In Progress":a.state==="In Progress"?"Done":null;
return '<div class="act"><div><div>'+esc(a.text)+'</div><div class="m" style="color:var(--muted);font-size:.9rem">'+esc(a.who||"Unassigned")+' · due '+esc(a.due||"no date")+' · '+a.state+(isOver(a)?' <span class="over">Overdue</span>':'')+'</div></div>'+(nx?'<button class="btn sec sm" data-adv="'+a.id+'">Mark '+nx+'</button>':'')+'</div>'}).join(""):'<div class="empty">No corrective actions yet. Add the first one below.</div>';
return '<button class="btn sec sm" data-v="list">Back to complaints</button><h2 style="margin-top:14px">'+c.id+' · '+esc(c.title)+'</h2><div class="panel"><dl><dt>Status</dt><dd><select id="st" style="max-width:200px">'+STATES.map(function(s){return '<option'+(s===c.status?' selected':'')+'>'+s+'</option>'}).join("")+'</select></dd><dt>Severity</dt><dd><span class="tag v-'+c.severity+'">'+c.severity+'</span></dd><dt>Type</dt><dd>'+esc(c.type)+'</dd><dt>Location</dt><dd>'+esc(c.location)+'</dd><dt>Reported</dt><dd>'+c.created+'</dd><dt>Reporter</dt><dd>'+esc(c.name||"Anonymous")+(c.contact?' ('+esc(c.contact)+')':'')+'</dd><dt>Details</dt><dd>'+esc(c.desc)+'</dd></dl></div><div class="panel"><h3 style="margin:0 0 4px">Corrective actions</h3>'+acts+'<form id="af" style="margin-top:12px"><div class="row2"><div><label for="at">Action</label><input id="at" required></div><div><label for="aw">Assignee</label><input id="aw"></div></div><label for="ad">Due date</label><input id="ad" type="date" style="max-width:200px"><br><button class="btn" type="submit">Add action</button></form></div>';}
function render(){nav();var h=view==="dash"?dash():view==="report"?report():view==="detail"?detail():list();document.getElementById("app").innerHTML=h;notice=""}
function sync(c){if(c.actions.length&&c.actions.every(function(a){return a.state==="Done"}))c.status="Resolved"}
document.addEventListener("click",function(e){var b=e.target.closest("button");if(!b)return;
if(b.dataset.v){view=b.dataset.v;render()}
else if(b.dataset.id){sel=b.dataset.id;view="detail";render()}
else if(b.dataset.adv){var c=data.find(function(x){return x.id===sel}),a=c.actions.find(function(x){return x.id==b.dataset.adv});a.state=a.state==="Pending"?"In Progress":"Done";sync(c);save();render()}});
document.addEventListener("input",function(e){if(e.target.id==="q"){flt.q=e.target.value;document.getElementById("res").innerHTML=rows(filtered())}});
function filtered(){return data.filter(function(c){var q=flt.q.toLowerCase();return(!flt.st||c.status===flt.st)&&(!flt.sv||c.severity===flt.sv)&&(!q||(c.id+c.title+c.location+c.desc).toLowerCase().indexOf(q)>-1)})}
document.addEventListener("change",function(e){var t=e.target;
if(t.id==="fs"){flt.st=t.value;document.getElementById("res").innerHTML=rows(filtered())}
else if(t.id==="fv"){flt.sv=t.value;document.getElementById("res").innerHTML=rows(filtered())}
else if(t.id==="st"){var c=data.find(function(x){return x.id===sel});c.status=t.value;save();render()}});
document.addEventListener("submit",function(e){e.preventDefault();var f=e.target,v=function(i){return document.getElementById(i).value.trim()};
if(f.id==="f"){var id=nextId();data.unshift({id:id,title:v("t"),type:v("ty"),severity:v("sv"),location:v("lo"),desc:v("de"),name:v("nm"),contact:v("co"),status:"Open",created:today(),actions:[]});save();view="report";notice="Complaint "+id+" submitted. Its status is Open.";render()}
else if(f.id==="af"){var c=data.find(function(x){return x.id===sel});c.actions.push({id:Date.now(),text:v("at"),who:v("aw"),due:v("ad"),state:"Pending"});if(c.status==="Resolved")c.status="Investigating";save();render()}});
render();
})();
</script>
</body>
</html>
