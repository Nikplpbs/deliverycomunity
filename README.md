<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Community Delivery · ชุมชน</title>
<link href="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box}
body{font-family:'Sarabun',sans-serif;background:#f0f4ff;-webkit-tap-highlight-color:transparent;font-size:15px;line-height:1.6;-webkit-font-smoothing:antialiased}
@keyframes shake{0%,100%{transform:translateX(0)}20%{transform:translateX(-8px)}40%{transform:translateX(8px)}60%{transform:translateX(-4px)}80%{transform:translateX(4px)}}
.shake{animation:shake 0.45s ease}
@keyframes cardIn{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}
.card-in{animation:cardIn 0.3s ease both}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
.fade-in{animation:fadeIn 0.3s ease both}
#progressBar{position:fixed;top:0;left:0;height:3px;background:linear-gradient(90deg,#6366f1,#8b5cf6,#a78bfa);width:0%;z-index:9999;transition:width .3s ease,opacity .4s ease;border-radius:0 99px 99px 0;box-shadow:0 0 10px #6366f166}
#mainContent{display:block;opacity:1}
.tab-btn{transition:all .2s ease;border-bottom:3px solid transparent;opacity:.45;flex-shrink:0;white-space:nowrap}
.tab-btn.active{border-bottom-color:#6366f1;color:#6366f1;opacity:1;font-weight:800}
.comm-card{transition:all .2s ease;cursor:pointer}
.comm-card.selected{transform:scale(1.02)}
.freq-bar{height:5px;border-radius:99px;transition:width .6s cubic-bezier(.4,0,.2,1)}
.hide-scroll::-webkit-scrollbar{display:none}
.hide-scroll{-ms-overflow-style:none;scrollbar-width:none}
.glass{background:rgba(255,255,255,0.85);backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px)}
input[type="date"]::-webkit-calendar-picker-indicator{position:absolute;inset:0;width:100%;height:100%;opacity:0;cursor:pointer}
.segment-pill{border-radius:99px;padding:2px 10px;font-size:11px;font-weight:800;display:inline-flex;align-items:center;gap:4px}
.ring-indigo{box-shadow:0 0 0 3px #6366f133}
</style>
</head>
<body class="pb-24 text-slate-800">

<div id="progressBar"></div>


<!-- ═══ MAIN APP ═══ -->
<div id="mainContent">
<div class="max-w-md mx-auto px-4 pt-5">

  <!-- HEADER -->
  <div class="rounded-3xl p-5 mb-5 text-white relative overflow-hidden shadow-xl" style="background:linear-gradient(135deg,#4f46e5 0%,#7c3aed 60%,#6d28d9 100%)">
    <div class="absolute -top-8 -right-8 w-36 h-36 rounded-full bg-white/10"></div>
    <div class="absolute -bottom-6 left-8 w-24 h-24 rounded-full bg-white/5"></div>
    <div class="relative z-10">
      <p class="text-indigo-200 text-[10px] font-bold uppercase tracking-widest mb-0.5">Delivery Community Analytics</p>
      <h1 class="text-xl font-black tracking-wide leading-tight">ความถี่ลูกค้า &amp; สินค้า</h1>
    </div>
  </div>

  <!-- COMMUNITY SELECTOR -->
  <div class="bg-white rounded-3xl p-4 shadow-sm border border-indigo-50 mb-4">
    <div class="flex items-center justify-between mb-3">
      <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest">เลือกชุมชน</p>
      <button onclick="clearCommunity()" id="clearCommBtn" class="hidden text-[10px] font-bold text-indigo-500 bg-indigo-50 px-3 py-1 rounded-full">✕ ล้าง</button>
    </div>
    <div class="grid grid-cols-2 gap-2.5" id="communityGrid"></div>
  </div>


  <!-- SUMMARY KPIs -->
  <div id="kpiCards" class="hidden mb-4">
    <div class="grid grid-cols-4 gap-2">
      <div class="rounded-2xl p-3 text-center text-white shadow-md" style="background:linear-gradient(135deg,#6366f1,#4f46e5)">
        <p class="text-[9px] font-bold opacity-80 uppercase tracking-wide">ออเดอร์</p>
        <p id="kpiOrders" class="text-xl font-black mt-0.5">0</p>
      </div>
      <div class="rounded-2xl p-3 text-center text-white shadow-md" style="background:linear-gradient(135deg,#7c3aed,#6d28d9)">
        <p class="text-[9px] font-bold opacity-80 uppercase tracking-wide">ลูกค้า</p>
        <p id="kpiCustomers" class="text-xl font-black mt-0.5">0</p>
      </div>
      <div class="rounded-2xl p-3 text-center text-white shadow-md" style="background:linear-gradient(135deg,#0ea5e9,#0284c7)">
        <p class="text-[9px] font-bold opacity-80 uppercase tracking-wide">ประจำ</p>
        <p id="kpiRepeat" class="text-xl font-black mt-0.5">0</p>
      </div>
      <div class="rounded-2xl p-3 text-center text-white shadow-md" style="background:linear-gradient(135deg,#10b981,#059669)">
        <p class="text-[9px] font-bold opacity-80 uppercase tracking-wide">ยอดรวม</p>
        <p id="kpiRevenue" class="text-[13px] font-black mt-0.5">฿0</p>
      </div>
    </div>
  </div>

  <!-- TABS -->
  <div class="flex gap-1 border-b-2 border-gray-100 mb-4 overflow-x-auto hide-scroll">
    <button onclick="switchView('freq')"    id="tab-freq"    class="tab-btn active px-4 py-3 text-sm">👥 ความถี่</button>
    <button onclick="switchView('product')" id="tab-product" class="tab-btn px-4 py-3 text-sm">🛒 สินค้า</button>
    <button onclick="switchView('store')"   id="tab-store"   class="tab-btn px-4 py-3 text-sm">🏪 สาขา</button>
    <button onclick="switchView('compare')" id="tab-compare" class="tab-btn px-4 py-3 text-sm">📊 เปรียบเทียบ</button>
  </div>

  <!-- CONTENT -->
  <div id="contentArea">
    <div class="text-center py-24 bg-white rounded-3xl border-2 border-dashed border-gray-200">
      <div class="text-4xl mb-3">📦</div>
      <p class="text-gray-400 font-bold">กรุณาเลือกชุมชนและวันที่</p>
    </div>
  </div>

</div>
</div>

<!-- STATUS TOAST -->
<div id="statusToast" class="hidden fixed bottom-6 left-4 right-4 max-w-md mx-auto rounded-2xl p-4 text-sm font-bold text-center shadow-2xl z-50 text-white transition-all"></div>

<!-- ═══ CUSTOMER PRODUCT MODAL ═══ -->
<div id="modalOverlay" class="hidden fixed inset-0 z-40 bg-black/40 backdrop-blur-sm" onclick="closeModal()"></div>
<div id="modalSheet" class="fixed bottom-0 left-0 right-0 z-50 max-w-md mx-auto rounded-t-3xl bg-white shadow-2xl transition-transform duration-300 translate-y-full" style="max-height:85vh">
  <div class="sticky top-0 bg-white rounded-t-3xl px-5 pt-4 pb-3 border-b border-gray-100 z-10">
    <div class="w-10 h-1 bg-gray-200 rounded-full mx-auto mb-4"></div>
    <div class="flex justify-between items-start">
      <div>
        <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest">สินค้าที่ซื้อ</p>
        <p id="modalPhone" class="text-lg font-black text-gray-900 mt-0.5">—</p>
        <p id="modalMeta"  class="text-[11px] text-gray-400 mt-0.5">—</p>
      </div>
      <button onclick="closeModal()" class="bg-gray-100 hover:bg-gray-200 active:scale-95 transition-all p-2 rounded-xl">
        <svg class="h-5 w-5 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M6 18L18 6M6 6l12 12"/></svg>
      </button>
    </div>
  </div>
  <div id="modalBody" class="overflow-y-auto px-5 py-4 hide-scroll" style="max-height:calc(85vh - 110px)"></div>
</div>

<script>
// ═══ CONFIG ═══
const API_URL = "https://script.google.com/macros/s/AKfycbyv4xdJ1l8Yk4qeTv4OhXf6DbXx3tH7gzaRhVSbcCKYjIsVx2SSossn-8Es7kSuvLJncQ/exec";

// ═══ COMMUNITIES ═══
const COMMUNITIES = {
  'ชุมชนบางบ่อ':            { emoji:'🏘️', color:'blue'   },
  'ชุมชนบัวนครินทร์':       { emoji:'🌿', color:'green'  },
  'ชุมชนพลมานีย์ลาดกระบัง': { emoji:'🏡', color:'purple' },
  'ชุมชนนนทรี14':           { emoji:'🌳', color:'orange' },
};
const COMM_STYLE = {
  blue:   { bg:'#eff6ff', border:'#3b82f6', text:'#1d4ed8', accent:'#3b82f6', light:'#dbeafe' },
  green:  { bg:'#f0fdf4', border:'#22c55e', text:'#15803d', accent:'#22c55e', light:'#dcfce7' },
  purple: { bg:'#faf5ff', border:'#a855f7', text:'#7e22ce', accent:'#a855f7', light:'#f3e8ff' },
  orange: { bg:'#fff7ed', border:'#f97316', text:'#c2410c', accent:'#f97316', light:'#ffedd5' },
};
const TYPE_COLOR = {
  'on shelf':       '#3b82f6',
  'เครื่องดื่มชง': '#f59e0b',
  'อุ่นร้อน':       '#ef4444',
  'อาหารสด':        '#10b981',
  'เบเกอรี่':       '#ec4899',
};
function typeColor(t){ return TYPE_COLOR[t]||'#94a3b8'; }

// ═══ STATE ═══
// allData คือข้อมูลจริงจาก GAS (pre-aggregated แล้ว)
let allData = {
  customers:          [],   // {community, รหัสร้าน, store_name, เบอร์ผู้สั่ง, order_count, total_revenue, avg_order, last_order, first_order, customer_group}
  customerProducts:   [],   // {เบอร์ผู้สั่ง, community, ชื่อสินค้า, ประเภทสินค้า, times_ordered, total_qty, total_spend}
  storeSummary:       [],   // {community, รหัสร้าน, store_name, order_count, unique_customers, total_revenue, avg_order}
  communitySummary:   [],   // {community, order_count, unique_customers, total_revenue, avg_order, repeat_customers, repeat_rate}
  productsByCommunity:[],   // {community, ชื่อสินค้า, ประเภทสินค้า, order_count, total_revenue}
  productsByType:     [],   // {community, ประเภทสินค้า, order_count, total_revenue}
};
let currentCommunity = null;
let currentView      = 'freq';

const $ = id => document.getElementById(id);

// ═══ UTILS ═══
function esc(s){ if(s==null)return''; return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;'); }
function numFmt(n){ return Number(n||0).toLocaleString('th-TH'); }
function fmtTH(s){ if(!s)return'—'; try{ const[y,m,d]=String(s).split('-'); const M=['ม.ค.','ก.พ.','มี.ค.','เม.ย.','พ.ค.','มิ.ย.','ก.ค.','ส.ค.','ก.ย.','ต.ค.','พ.ย.','ธ.ค.']; return`${+d} ${M[+m-1]} ${+y+543}`; }catch(e){return s;} }
function pct(v,max){ return max>0?Math.round((v/max)*100):0; }
function fmtRev(n){ n=Number(n||0); return n>=100000?`฿${(n/1000).toFixed(1)}K`:n>=10000?`฿${(n/1000).toFixed(1)}K`:`฿${numFmt(n)}`; }

function progress(v,done=false){
  const pb=$('progressBar'); if(!pb)return;
  pb.style.width=v+'%'; pb.style.opacity='1';
  if(done)setTimeout(()=>{pb.style.opacity='0';},400);
}
function toast(msg,type='info',dur=3000){
  const el=$('statusToast'); if(!el)return;
  const c={success:'#16a34a',error:'#dc2626',info:'#6366f1'};
  el.textContent=msg; el.style.background=c[type]||c.info;
  el.classList.remove('hidden');
  if(dur>0)setTimeout(()=>el.classList.add('hidden'),dur);
}

// ═══ FILTER BY COMMUNITY ═══
function filterBy(arr, commField='community'){
  if(!currentCommunity) return arr;
  return arr.filter(r=>r[commField]===currentCommunity);
}

// ═══ FETCH DATA ═══
async function fetchData(){
  progress(30);
  try{
    const r = await fetch(`${API_URL}?action=all`);
    const d = await r.json();
    if(d.error) throw new Error(d.error);
    allData.customers           = d.customers           || [];
    allData.customerProducts    = d.customer_products   || [];
    allData.storeSummary        = d.store_summary        || [];
    allData.communitySummary    = d.community_summary    || [];
    allData.productsByCommunity = d.products_by_community|| [];
    allData.productsByType      = d.products_by_type     || [];
    const total = allData.customers.length;
    if(!total) throw new Error('empty');
    toast(`✅ โหลด ${numFmt(total)} ลูกค้า`,'success');
    progress(100,true);
  } catch(e){
    progress(100,true);
    toast('⚠️ Demo Mode — ยังไม่ได้เชื่อม GAS','info');
    allData = generateMockData();
  }
  renderKPIs();
  applyFilters();
}

// ═══ MOCK DATA (ใช้ตอน GAS ยังไม่พร้อม) ═══
function generateMockData(){
  const comms=Object.keys(COMMUNITIES);
  const types=['on shelf','เครื่องดื่มชง','อุ่นร้อน'];
  const products=['กล้วยหอมทอง','น้ำอัดลม','ขนมปัง','ไข่ต้ม','ชานมเย็น','บะหมี่กึ่งสำเร็จรูป','สแนก','น้ำดื่ม'];
  const segs=['VIP','Loyal','Regular','One-time'];
  const phones=Array.from({length:60},(_,i)=>`08${String(i).padStart(2,'0')}xxxx${String(i%100).padStart(2,'0')}`);

  const customers=[];
  phones.forEach((p,i)=>{
    const comm=comms[i%comms.length];
    const cnt=[12,7,3,1][i%4];
    customers.push({ community:comm, รหัสร้าน:'1375', store_name:'โครงการ PST',
      เบอร์ผู้สั่ง:p, order_count:cnt, total_revenue:cnt*150, avg_order:150,
      last_order:'2026-04-03', first_order:'2026-03-01', customer_group:segs[i%4] });
  });

  const customerProducts=[];
  phones.forEach(p=>{
    products.slice(0,3).forEach(name=>{
      customerProducts.push({ เบอร์ผู้สั่ง:p, community:comms[0], ชื่อสินค้า:name,
        ประเภทสินค้า:types[0], times_ordered:2, total_qty:2, total_spend:80 });
    });
  });

  const storeSummary=comms.flatMap(c=>[
    {community:c,รหัสร้าน:'1375',store_name:'สาขา A',order_count:120,unique_customers:80,total_revenue:18000,avg_order:150},
    {community:c,รหัสร้าน:'1376',store_name:'สาขา B',order_count:90, unique_customers:60,total_revenue:13500,avg_order:150},
  ]);

  const communitySummary=comms.map(c=>({
    community:c, order_count:210, unique_customers:140, total_revenue:31500,
    avg_order:150, repeat_customers:42, repeat_rate:30
  }));

  const productsByCommunity=comms.flatMap(c=>
    products.map(name=>({ community:c, ชื่อสินค้า:name, ประเภทสินค้า:types[0],
      order_count:Math.floor(Math.random()*50)+5, total_revenue:Math.floor(Math.random()*3000)+500 }))
  );

  const productsByType=comms.flatMap(c=>
    types.map(t=>({ community:c, ประเภทสินค้า:t, order_count:Math.floor(Math.random()*100)+20, total_revenue:Math.floor(Math.random()*10000) }))
  );

  return {customers,customerProducts,storeSummary,communitySummary,productsByCommunity,productsByType};
}

// ═══ KPI CARDS ═══
function renderKPIs(){
  const custs = filterBy(allData.customers);
  const comms = filterBy(allData.communitySummary);
  if(!custs.length){ $('kpiCards').classList.add('hidden'); return; }
  $('kpiCards').classList.remove('hidden');
  const totalOrders   = comms.reduce((a,r)=>a+(Number(r.order_count)||0),0);
  const uniqueCustomers = custs.length;
  const repeatCount   = custs.filter(r=>r.customer_group==='VIP'||r.customer_group==='Loyal'||r.customer_group==='Regular').length;
  const totalRev      = comms.reduce((a,r)=>a+(Number(r.total_revenue)||0),0);
  $('kpiOrders').textContent    = numFmt(totalOrders);
  $('kpiCustomers').textContent = numFmt(uniqueCustomers);
  $('kpiRepeat').textContent    = numFmt(repeatCount);
  $('kpiRevenue').textContent   = fmtRev(totalRev);
}

// ═══ COMMUNITY GRID ═══
function buildCommunityGrid(){
  const g=$('communityGrid'); g.innerHTML='';
  Object.entries(COMMUNITIES).forEach(([name,cfg])=>{
    const st=COMM_STYLE[cfg.color];
    const isActive=currentCommunity===name;
    const btn=document.createElement('button');
    btn.className=`comm-card rounded-2xl p-3.5 text-left border-2 transition-all`;
    btn.style.cssText=`background:${isActive?st.light:st.bg};border-color:${isActive?st.accent:st.light};color:${st.text};${isActive?`box-shadow:0 0 0 3px ${st.accent}33`:''}`;
    // นับ order_count จาก storeSummary
    const storeRows=allData.storeSummary.filter(r=>r.community===name);
    const totalOrders=storeRows.reduce((a,r)=>a+(Number(r.order_count)||0),0);
    btn.innerHTML=`
      <div class="flex items-start justify-between">
        <span class="text-xl leading-none">${cfg.emoji}</span>
        ${isActive?`<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/></svg>`:''}
      </div>
      <p class="font-black text-[13px] mt-2 leading-snug">${esc(name.replace('ชุมชน',''))}</p>
      <p class="text-[10px] mt-0.5 opacity-60">${totalOrders?numFmt(totalOrders)+' ออเดอร์':'—'}</p>`;
    btn.onclick=()=>{ currentCommunity=currentCommunity===name?null:name; buildCommunityGrid(); updateClearBtn(); renderKPIs(); applyFilters(); };
    g.appendChild(btn);
  });
}
function clearCommunity(){ currentCommunity=null; buildCommunityGrid(); updateClearBtn(); renderKPIs(); applyFilters(); }
function updateClearBtn(){ $('clearCommBtn').classList.toggle('hidden',!currentCommunity); }
function switchView(v){
  currentView=v;
  ['freq','product','store','compare'].forEach(t=>$(`tab-${t}`)?.classList.toggle('active',t===v));
  applyFilters();
}
function applyFilters(){
  const area=$('contentArea');
  if(currentView==='freq')    renderFreq();
  else if(currentView==='product') renderProduct();
  else if(currentView==='store')   renderStore();
  else if(currentView==='compare') renderCompare();
}

// ═══ VIEW: ความถี่ลูกค้า ═══
function renderFreq(){
  const data=filterBy(allData.customers).sort((a,b)=>(Number(b.order_count)||0)-(Number(a.order_count)||0));
  if(!data.length){ $('contentArea').innerHTML=emptyState(); return; }

  const maxC=Number(data[0].order_count)||1;
  const segs={VIP:0,Loyal:0,Regular:0,'One-time':0};
  data.forEach(r=>{ if(segs[r.customer_group]!==undefined) segs[r.customer_group]++; });

  const segCards=[
    {k:'VIP',    label:'🏆 VIP',       grad:'linear-gradient(135deg,#f59e0b,#d97706)', cnt:segs['VIP']},
    {k:'Loyal',  label:'💙 Loyal',     grad:'linear-gradient(135deg,#6366f1,#4f46e5)', cnt:segs['Loyal']},
    {k:'Regular',label:'💚 Regular',   grad:'linear-gradient(135deg,#10b981,#059669)', cnt:segs['Regular']},
    {k:'One-time',label:'⚪ ครั้งเดียว',grad:'linear-gradient(135deg,#94a3b8,#64748b)',cnt:segs['One-time']},
  ].map(s=>`
    <div class="rounded-2xl p-3 text-white text-center shadow-md card-in" style="background:${s.grad}">
      <p class="text-[9px] font-bold opacity-80 uppercase tracking-wide leading-none">${s.label}</p>
      <p class="text-xl font-black mt-1">${s.cnt}</p>
    </div>`).join('');

  const total=data.length;
  const repeatPct=total?Math.round((segs.VIP+segs.Loyal+segs.Regular)/total*100):0;
  const repColor=repeatPct>=50?'#16a34a':repeatPct>=30?'#d97706':'#dc2626';
  const repEmoji=repeatPct>=50?'🔥':repeatPct>=30?'📈':'📉';

  const listRows=data.slice(0,100).map((r,i)=>{
    const cnt=Number(r.order_count)||0;
    const barColor=r.customer_group==='VIP'?'#f59e0b':r.customer_group==='Loyal'?'#6366f1':r.customer_group==='Regular'?'#10b981':'#94a3b8';
    const tagStyle=r.customer_group==='VIP'?'background:#fef3c7;color:#92400e':r.customer_group==='Loyal'?'background:#e0e7ff;color:#3730a3':r.customer_group==='Regular'?'background:#d1fae5;color:#065f46':'background:#f1f5f9;color:#64748b';
    const tagLabel=r.customer_group==='VIP'?'🏆 VIP':r.customer_group==='Loyal'?'💙 Loyal':r.customer_group==='Regular'?'💚 Regular':'⚪';
    return `
    <div class="bg-white rounded-2xl p-4 mb-2.5 shadow-sm border border-gray-100 card-in cursor-pointer active:scale-[0.98] transition-transform select-none"
      style="animation-delay:${i*10}ms" onclick="openModal(${JSON.stringify(String(r['เบอร์ผู้สั่ง']))})">
      <div class="flex justify-between items-start mb-2.5">
        <div class="flex items-center gap-2.5">
          <div class="w-7 h-7 rounded-full flex items-center justify-center text-[11px] font-black text-white flex-shrink-0" style="background:${barColor}">${i+1}</div>
          <div>
            <p class="font-black text-gray-900 text-[15px]">${esc(r['เบอร์ผู้สั่ง'])}</p>
            <p class="text-[10px] text-gray-400 mt-0.5">${esc(r.store_name||r['รหัสร้าน']||'')} · ล่าสุด ${fmtTH(r.last_order)}</p>
          </div>
        </div>
        <div class="flex items-center gap-1.5">
          <span class="text-[10px] font-black px-2 py-1 rounded-full" style="${tagStyle}">${tagLabel}</span>
          <svg class="h-4 w-4 text-gray-300" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M9 5l7 7-7 7"/></svg>
        </div>
      </div>
      <div class="flex items-center gap-3">
        <div class="flex-1 bg-gray-100 rounded-full overflow-hidden h-1.5">
          <div class="freq-bar h-full rounded-full" style="width:${pct(cnt,maxC)}%;background:${barColor}"></div>
        </div>
        <span class="text-sm font-black text-gray-700 w-16 text-right flex-shrink-0">${numFmt(cnt)} ออเดอร์</span>
      </div>
      <div class="flex justify-between mt-1.5">
        <span class="text-[10px] text-gray-400">AOV ฿${numFmt(Number(r.avg_order||0).toFixed(0))}</span>
        <span class="text-[11px] font-black text-emerald-600">${fmtRev(r.total_revenue)}</span>
      </div>
    </div>`;
  }).join('');

  $('contentArea').innerHTML=`
    <div class="fade-in">
      <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest mb-2.5">กลุ่มลูกค้า · ${esc(currentCommunity||'ทุกชุมชน')}</p>
      <div class="grid grid-cols-4 gap-2 mb-4">${segCards}</div>
      <div class="bg-white rounded-2xl p-4 mb-4 shadow-sm border border-gray-100 flex items-center justify-between">
        <div>
          <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest">อัตราลูกค้าประจำ</p>
          <p class="text-2xl font-black mt-1" style="color:${repColor}">${repEmoji} ${repeatPct}%</p>
        </div>
        <div class="text-right text-[11px] text-gray-500">
          <p>จาก ${numFmt(total)} คน</p>
        </div>
      </div>
      <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest mb-2.5">TOP ลูกค้าบ่อยที่สุด</p>
      ${listRows}
    </div>`;
}

// ═══ VIEW: สินค้า ═══
function renderProduct(){
  const byType = filterBy(allData.productsByType).sort((a,b)=>b.order_count-a.order_count);
  const byProd = filterBy(allData.productsByCommunity).sort((a,b)=>b.order_count-a.order_count).slice(0,30);
  if(!byType.length && !byProd.length){ $('contentArea').innerHTML=emptyState(); return; }

  const maxT=byType[0]?.order_count||1;
  const typeBars=byType.map(r=>{
    const col=typeColor(r['ประเภทสินค้า']);
    return `
    <div class="bg-white rounded-2xl p-4 mb-2.5 shadow-sm border border-gray-100">
      <div class="flex justify-between items-center mb-2">
        <span class="font-black text-gray-800 text-sm">${esc(r['ประเภทสินค้า']||'ไม่ระบุ')}</span>
        <div class="text-right">
          <span class="text-xs font-black text-gray-600">${numFmt(r.order_count)} ออเดอร์</span>
          <span class="text-[10px] text-emerald-600 font-bold block">${fmtRev(r.total_revenue)}</span>
        </div>
      </div>
      <div class="bg-gray-100 rounded-full h-2 overflow-hidden">
        <div class="h-full rounded-full" style="width:${pct(r.order_count,maxT)}%;background:${col}"></div>
      </div>
    </div>`;
  }).join('');

  const prodRows=byProd.map((r,i)=>{
    const col=typeColor(r['ประเภทสินค้า']);
    return `
    <div class="flex items-center gap-3 py-2.5 border-b border-gray-50 last:border-0">
      <div class="w-6 h-6 rounded-full flex items-center justify-center text-[10px] font-black text-white flex-shrink-0" style="background:${col}">${i+1}</div>
      <div class="flex-1 min-w-0">
        <p class="font-bold text-gray-800 text-sm truncate">${esc(r['ชื่อสินค้า'])}</p>
        <p class="text-[10px] text-gray-400">${esc(r['ประเภทสินค้า']||'—')}</p>
      </div>
      <div class="text-right flex-shrink-0">
        <p class="text-sm font-black text-gray-700">${numFmt(r.order_count)}x</p>
        <p class="text-[10px] text-emerald-600 font-bold">${fmtRev(r.total_revenue)}</p>
      </div>
    </div>`;
  }).join('');

  $('contentArea').innerHTML=`
    <div class="fade-in">
      <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest mb-2.5">ประเภทสินค้า · ${esc(currentCommunity||'ทุกชุมชน')}</p>
      ${typeBars}
      <div class="bg-white rounded-2xl p-5 shadow-sm border border-gray-100 mt-2">
        <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest mb-3">TOP 30 สินค้าขายดี</p>
        ${prodRows||'<p class="text-gray-400 text-sm font-medium">ไม่พบข้อมูล</p>'}
      </div>
    </div>`;
}

// ═══ VIEW: สาขา ═══
function renderStore(){
  const data = filterBy(allData.storeSummary);
  if(!data.length){ $('contentArea').innerHTML=emptyState(); return; }

  const commsToShow = currentCommunity
    ? {[currentCommunity]: COMMUNITIES[currentCommunity]}
    : COMMUNITIES;

  const html=Object.entries(commsToShow).map(([cName,cfg])=>{
    const st=COMM_STYLE[cfg.color];
    const rows=data.filter(r=>r.community===cName);
    const totOrders=rows.reduce((a,r)=>a+(Number(r.order_count)||0),0);
    const totRev=rows.reduce((a,r)=>a+(Number(r.total_revenue)||0),0);
    const totCust=rows.reduce((a,r)=>a+(Number(r.unique_customers)||0),0);

    const storeRows=rows.sort((a,b)=>b.order_count-a.order_count).map(r=>`
      <div class="flex items-center justify-between py-3 border-b border-gray-50 last:border-0">
        <div class="flex-1 min-w-0 pr-3">
          <p class="font-black text-gray-800 text-sm">${esc(r.store_name||r['รหัสร้าน'])}</p>
          <p class="text-[10px] font-bold mt-0.5" style="color:${st.text}">รหัส ${esc(r['รหัสร้าน'])} · ${numFmt(r.unique_customers)} ลูกค้า</p>
        </div>
        <div class="text-right flex-shrink-0">
          <p class="font-black text-gray-800">${numFmt(r.order_count)}</p>
          <p class="text-[10px] text-emerald-600 font-bold">${fmtRev(r.total_revenue)}</p>
        </div>
      </div>`).join('');

    return `
    <div class="bg-white rounded-2xl p-5 mb-3 shadow-sm border-2 card-in" style="border-color:${st.border}">
      <div class="flex justify-between items-start mb-3">
        <div>
          <p class="font-black" style="color:${st.text}">${cfg.emoji} ${esc(cName)}</p>
          <p class="text-[10px] text-gray-400 mt-0.5">${rows.length} สาขา · ${numFmt(totCust)} ลูกค้า</p>
        </div>
        <div class="text-right">
          <p class="font-black text-gray-800 text-lg">${numFmt(totOrders)}</p>
          <p class="text-[10px] text-emerald-600 font-bold">${fmtRev(totRev)}</p>
        </div>
      </div>
      <div class="border-t border-gray-100 pt-1">${storeRows||'<p class="text-gray-400 text-sm py-2">ไม่มีข้อมูล</p>'}</div>
    </div>`;
  }).join('');

  $('contentArea').innerHTML=`<div class="fade-in">${html}</div>`;
}

// ═══ VIEW: เปรียบเทียบ ═══
function renderCompare(){
  const data=allData.communitySummary;
  if(!data.length){ $('contentArea').innerHTML=emptyState(); return; }

  const sorted=[...data].sort((a,b)=>b.order_count-a.order_count);
  const maxO=sorted[0]?.order_count||1;

  const rankHtml=sorted.map((r,i)=>{
    const cfg=COMMUNITIES[r.community]||{emoji:'🏪',color:'blue'};
    const st=COMM_STYLE[cfg.color];
    const medal=['🥇','🥈','🥉','4️⃣'][i]||`${i+1}.`;
    return `
    <div class="flex items-center gap-3 py-2.5 border-b border-gray-50 last:border-0">
      <span class="text-base w-6 text-center flex-shrink-0">${medal}</span>
      <span class="font-black text-sm flex-shrink-0" style="color:${st.text}">${cfg.emoji} ${esc(r.community.replace('ชุมชน',''))}</span>
      <div class="flex-1 bg-gray-100 rounded-full h-1.5 overflow-hidden">
        <div class="h-full rounded-full" style="width:${pct(r.order_count,maxO)}%;background:${st.accent}"></div>
      </div>
      <span class="text-sm font-black text-gray-700 flex-shrink-0">${numFmt(r.order_count)}</span>
    </div>`;
  }).join('');

  const cards=sorted.map(r=>{
    const cfg=COMMUNITIES[r.community]||{emoji:'🏪',color:'blue'};
    const st=COMM_STYLE[cfg.color];
    // สินค้านิยม
    const topProd=allData.productsByCommunity.filter(p=>p.community===r.community).sort((a,b)=>b.order_count-a.order_count)[0];
    return `
    <div class="bg-white rounded-2xl p-4 mb-3 shadow-sm border-2 card-in" style="border-color:${st.border}20">
      <div class="flex items-center gap-2 mb-3">
        <span class="text-xl">${cfg.emoji}</span>
        <p class="font-black text-sm" style="color:${st.text}">${esc(r.community)}</p>
      </div>
      <div class="grid grid-cols-3 gap-2 mb-3">
        <div class="rounded-xl p-2.5 text-center" style="background:${st.bg}">
          <p class="text-[9px] font-bold text-gray-500 uppercase">ออเดอร์</p>
          <p class="text-base font-black" style="color:${st.text}">${numFmt(r.order_count)}</p>
        </div>
        <div class="rounded-xl p-2.5 text-center" style="background:${st.bg}">
          <p class="text-[9px] font-bold text-gray-500 uppercase">ลูกค้า</p>
          <p class="text-base font-black" style="color:${st.text}">${numFmt(r.unique_customers)}</p>
        </div>
        <div class="rounded-xl p-2.5 text-center" style="background:${st.bg}">
          <p class="text-[9px] font-bold text-gray-500 uppercase">AOV</p>
          <p class="text-base font-black" style="color:${st.text}">฿${numFmt(Number(r.avg_order||0).toFixed(0))}</p>
        </div>
      </div>
      <div class="flex justify-between items-center text-[11px] text-gray-500 mt-1">
        <span>ลูกค้าประจำ <strong class="text-gray-800">${r.repeat_rate||0}%</strong></span>
        ${topProd?`<span>ขายดี: <strong class="text-gray-800">${esc(topProd['ชื่อสินค้า'])}</strong></span>`:''}
      </div>
    </div>`;
  }).join('');

  $('contentArea').innerHTML=`
    <div class="fade-in">
      <div class="bg-white rounded-2xl p-5 mb-4 shadow-sm border border-gray-100">
        <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest mb-3">จัดอันดับออเดอร์</p>
        ${rankHtml}
      </div>
      <p class="text-[10px] font-black text-gray-400 uppercase tracking-widest mb-2.5">รายละเอียดชุมชน</p>
      ${cards}
    </div>`;
}

// ═══ MODAL: สินค้าที่ลูกค้าซื้อ ═══
function openModal(phone){
  const custProds = allData.customerProducts.filter(r=>String(r['เบอร์ผู้สั่ง'])===phone);
  const custInfo  = allData.customers.find(r=>String(r['เบอร์ผู้สั่ง'])===phone)||{};

  const sorted=custProds.sort((a,b)=>b.times_ordered-a.times_ordered);
  const maxCount=sorted[0]?.times_ordered||1;

  // จัดกลุ่มตาม ประเภทสินค้า
  const typeMap={};
  sorted.forEach(r=>{
    const t=r['ประเภทสินค้า']||'ไม่ระบุ';
    if(!typeMap[t]) typeMap[t]=[];
    typeMap[t].push(r);
  });

  const groupHtml=Object.entries(typeMap).map(([type,prods])=>{
    const col=typeColor(type);
    const rows=prods.map(r=>`
      <div class="mb-3">
        <div class="flex justify-between items-center mb-1">
          <span class="text-sm font-bold text-gray-800 leading-snug flex-1 pr-2">${esc(r['ชื่อสินค้า'])}</span>
          <div class="text-right flex-shrink-0">
            <span class="text-sm font-black text-gray-700">${numFmt(r.times_ordered)}x</span>
            <span class="text-[10px] text-emerald-600 font-bold block">${fmtRev(r.total_spend)}</span>
          </div>
        </div>
        <div class="bg-gray-100 rounded-full h-1.5 overflow-hidden">
          <div class="h-full rounded-full" style="width:${pct(r.times_ordered,maxCount)}%;background:${col}"></div>
        </div>
      </div>`).join('');
    return `
    <div class="mb-4">
      <p class="text-[10px] font-black uppercase tracking-widest mb-2.5 flex items-center gap-1.5" style="color:${col}">
        <span class="w-2 h-2 rounded-full flex-shrink-0" style="background:${col}"></span>${esc(type)}
      </p>
      ${rows}
    </div>`;
  }).join('');

  $('modalPhone').textContent = phone;
  $('modalMeta').textContent  = `${numFmt(custInfo.order_count||0)} ออเดอร์ · ${fmtRev(custInfo.total_revenue)} · ${esc(custInfo.customer_group||'')}`;
  $('modalBody').innerHTML    = groupHtml||'<p class="text-gray-400 font-medium text-sm">ไม่พบข้อมูลสินค้า</p>';
  $('modalOverlay').classList.remove('hidden');
  $('modalSheet').classList.remove('translate-y-full');
  document.body.style.overflow='hidden';
}
function closeModal(){
  $('modalOverlay').classList.add('hidden');
  $('modalSheet').classList.add('translate-y-full');
  document.body.style.overflow='';
}

function emptyState(){
  return `<div class="text-center py-24 bg-white rounded-3xl border-2 border-dashed border-gray-200"><div class="text-4xl mb-3">🔍</div><p class="text-gray-400 font-bold">ไม่พบข้อมูล</p></div>`;
}

// ═══ BOOT ═══
window.onload=()=>{ buildCommunityGrid(); fetchData(); };
</script>
</body>
</html>
