# PCG-Pathway
Pathway -Timeline - Milestones
!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>PCG — Full Interactive Dashboard (v2.6.0)</title>
<style>
  :root{ --brand:#003D7A; --ink:#0b2033; --bg:#f5f8fc; --card:#fff; --muted:#7a8aa0; --gold:#ffd54d; --shadow:0 10px 30px rgba(0,0,0,.08); }
  html,body{height:100%}
  body{margin:0;background:linear-gradient(180deg,var(--bg),#eaf1f9 40%);font-family:system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;color:var(--ink)}
  .wrap{max-width:1200px;margin:0 auto;padding:18px}
  header{position:sticky;top:0;z-index:5;background:linear-gradient(180deg,rgba(255,255,255,.9),rgba(255,255,255,.7));backdrop-filter:saturate(1.2) blur(6px);border-bottom:1px solid #e8eef8}
  .toolbar{display:flex;gap:12px;align-items:center;justify-content:space-between;padding:14px 0}
  h1{font-size:20px;margin:0;color:var(--brand)}
  .chip{border:1px dashed #c8d8f0;border-radius:999px;padding:6px 10px;font-size:12px;color:#335}
  .tabs{display:flex;gap:8px;flex-wrap:wrap}
  .tab{padding:8px 10px;border:1px solid #cfe0ff;border-radius:999px;background:#fff;font-size:12px;cursor:pointer}
  .tab.active{background:var(--brand);border-color:var(--brand);color:#fff}
  .card{background:#fff;border:1px solid #e6eef9;border-radius:16px;box-shadow:var(--shadow);padding:14px;margin:14px 0}
  .row{display:flex;gap:10px;flex-wrap:wrap;align-items:center}
  button,input,select,textarea{border:1px solid #cfe0ff;border-radius:10px;background:#fff;padding:8px 10px;font-size:14px}
  textarea{width:100%}
  button{cursor:pointer}
  .btn{background:var(--brand);color:#fff;border-color:var(--brand)}
  .ghost{background:#fff;color:var(--brand)}
  .warn{background:#d9480f;border-color:#d9480f;color:#fff}
  .muted{color:var(--muted)}
  /* Calendar */
  .calbar{display:flex;align-items:center;gap:8px;margin-bottom:10px}
  .calbar .title{font-weight:700}
  .calendar{display:grid;grid-template-columns:repeat(7,1fr);gap:8px}
  .calday{min-height:110px;background:#fff;border:1px solid #e6eef9;border-radius:12px;padding:8px;display:flex;flex-direction:column;gap:6px;cursor:pointer}
  .calday:hover{outline:2px dashed #c9dcff;outline-offset:2px}
  .calday .d{font-size:11px;color:#5a6a80}
  .pill{font-size:11px;border-radius:999px;padding:4px 8px;border:1px solid #d7e3ff;background:#eef4ff;cursor:pointer;user-select:none}
  /* Phases */
  .phase{border:1px solid #e6eef9;border-radius:16px;background:#fff;box-shadow:0 6px 18px rgba(0,0,0,.06);padding:12px}
  .phase-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:6px}
  .phase-title{font-weight:700;color:var(--brand)}
  .phase-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px}
  .ms-card{border:1px solid #e7eef8;border-radius:14px;background:#fff;box-shadow:0 2px 8px rgba(0,0,0,.05);padding:12px;display:flex;flex-direction:column;gap:8px;position:relative}
  .ms-card::before{content:"";position:absolute;left:0;top:0;bottom:0;width:6px;border-radius:14px 0 0 14px;background:var(--ms-accent,#003d7a20)}
  .badge{font-size:11px;border-radius:999px;padding:4px 8px;background:#eef4fb;border:1px solid #d5e2f3;color:#134}
  .linkpill{font-size:11px;padding:4px 8px;border-radius:999px;background:#003d7a10;border:1px solid #003d7a30}
  /* Kanban */
  .kanban{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
  .kcol{background:#fafcff;border:1px solid #e6eef9;border-radius:14px;padding:10px}
  .kcol h3{margin:6px 0 10px 0;font-size:14px}
  /* Timeline */
  .timeline{display:grid;gap:12px}
  .bar{position:relative;padding:10px;border-radius:12px;background:linear-gradient(90deg,#fff,#f7fbff);border:1px solid #e3edf8}
  .bar>i{position:absolute;left:0;top:0;bottom:0;border-radius:12px 0 0 12px;background:linear-gradient(90deg,var(--gold),#ffe680);opacity:.55}
  /* Modal (custom) */
  .modal-backdrop{position:fixed;inset:0;background:rgba(0,0,0,.4);display:none;align-items:center;justify-content:center;z-index:1000}
  .modal{width:min(720px,92vw);background:#fff;border-radius:14px;border:1px solid #e6eef9;box-shadow:0 20px 60px rgba(0,0,0,.25);padding:14px}
  .modal h3{margin:6px 0 12px 0}
  .hidden{display:none !important}
</style>
</head>
<body>
<header>
  <div class="wrap toolbar">
    <div class="row">
      <h1>PCG — Full Interactive Dashboard</h1>
      <span class="chip" id="todayChip">NZT</span>
    </div>
    <div class="tabs" id="tabs">
      <div class="tab" data-view="phases">Phases</div>
      <div class="tab" data-view="list">List</div>
      <div class="tab active" data-view="calendar">Calendar</div>
      <div class="tab" data-view="kanban">Kanban</div>
      <div class="tab" data-view="timeline">Timeline</div>
      <button class="ghost" id="export">Export</button>
      <label class="ghost" for="importFile" style="cursor:pointer">Import</label>
      <input id="importFile" type="file" accept="application/json" class="hidden" />
      <button class="warn" id="reset">Reset</button>
    </div>
  </div>
</header>

<main class="wrap">
  <!-- Filters & Obsidian -->
  <section class="card">
    <div class="row">
      <span class="muted">Filters:</span>
      <select id="filterPhase"><option value="">All phases</option></select>
      <input id="filterOwner" placeholder="Owner contains…" />
      <select id="filterStatus"><option value="">All statuses</option><option>Planned</option><option>In Progress</option><option>Blocked</option><option>Done</option></select>
      <button class="ghost" id="clearFilters">Clear</button>
      <span style="flex:1"></span>
      <button class="btn" id="addPhase">Add phase</button>
    </div>
    <div class="row" style="margin-top:10px">
      <strong>Obsidian:</strong>
      <input id="vaultName" placeholder="Pacific Compass Group Vault" style="min-width:260px" />
      <input id="defaultNote" placeholder="PCG Work Programme/" style="min-width:260px" />
      <button class="ghost" id="openDefaultNote">Open Default Note</button>
      <button class="ghost" id="copyObsidianLink">Copy obsidian://</button>
    </div>
  </section>

  <section id="phasesView" class="card hidden"></section>
  <section id="listView" class="card hidden">
    <div id="listContainer"></div>
  </section>
  <section id="calendarView" class="card">
    <div class="calbar">
      <button class="ghost" id="prevM">◀</button>
      <button class="ghost" id="todayM">Today</button>
      <button class="ghost" id="nextM">▶</button>
      <span style="flex:1"></span>
      <div class="title" id="monthTitle"></div>
    </div>
    <div class="calendar" id="calGrid"></div>
  </section>
  <section id="kanbanView" class="card hidden"></section>
  <section id="timelineView" class="card hidden"></section>

  <div class="muted">v2.6.0 — Restored full feature set with the stable calendar & custom modal.</div>
</main>

<!-- Modal -->
<div id="modalBackdrop" class="modal-backdrop">
  <div class="modal">
    <h3 id="modalTitle">Edit Milestone</h3>
    <div class="row" style="gap:8px;flex-wrap:wrap">
      <select id="mPhase" style="flex:1 1 260px"></select>
      <input id="mDue" type="date" />
      <input id="mTitle" placeholder="Title" style="flex:1 1 100%" />
      <input id="mOwner" placeholder="Owner" style="flex:1 1 260px" />
      <select id="mStatus"><option>Planned</option><option>In Progress</option><option>Blocked</option><option>Done</option></select>
      <label>Colour <input id="mColor" type="color" value="#0EA5E9" /></label>
      <input id="mNote" placeholder="Obsidian note path (optional)" style="flex:1 1 100%" />
      <input id="mLinks" placeholder="Links (comma-separated URL or Title|URL)" style="flex:1 1 100%" />
      <textarea id="mTasks" rows="4" placeholder="Tasks (one per line, prefix with [x] for done)" style="flex:1 1 100%"></textarea>
      <div class="row" style="width:100%;justify-content:flex-end;gap:8px">
        <button class="btn" id="saveBtn">Save</button>
        <button class="ghost" id="cancelBtn">Cancel</button>
        <button class="warn" id="deleteBtn">Delete</button>
      </div>
    </div>
  </div>
</div>

<script>
(function(){
  // ======= helpers =======
  const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const $ = s => document.querySelector(s);
  const $$ = s => Array.from(document.querySelectorAll(s));
  const ymd = d => `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
  const dNZ = d => { if(!d) return '—'; const dd=new Date(d); return String(dd.getDate()).padStart(2,'0')+' '+months[dd.getMonth()]+' '+dd.getFullYear(); };
  const parseJSON = (s,f) => { try { return JSON.parse(s) } catch { return f } };
  const KEY='pcg.v260.state'; const CFG='pcg.v260.cfg';

  const today = new Date();
  $('#todayChip').textContent = 'NZT · '+dNZ(today);

  const mk = (title, owner, due, status, color, note='', links=[], tasks=[]) =>
    ({id:Math.random().toString(36).slice(2,10), title, owner, due, status, color, note, links, tasks});

  function seeded(){
    return {
      phases:[
        {id:'p0', title:'Phase 0 — Pre-Establishment', start:'2025-09-01', end:'2025-10-12', color:'#FFE082', milestones:[
          mk('Draft press release (Oct 13 announcement)','Chris','2025-10-12','Planned','#0E7490','PCG Work Programme/Comms/Press Releases/2025-10-13',['Press Release Template|https://example.com/press-template']),
          mk('ASB / wholesale supply arrangements aligned','Chris','2025-10-11','In Progress','#2B6CB0','PCG Work Programme/Finance/ASB',[])
        ]},
        {id:'p1', title:'Phase 1 — Establishment Day', start:'2025-10-13', end:'2025-10-13', color:'#FFD93D', milestones:[
          mk('Register PCG & PIF with Companies Office','Chris','2025-10-13','Planned','#B45309','PCG Work Programme/Establishment',['Companies Office|https://companies-register.companiesoffice.govt.nz/']),
          mk('Execute share transfer — Kaumatua → PCG','Hari / Sourabh','2025-10-13','Planned','#92400E','PCG Work Programme/Establishment/Share Transfer',[]),
          mk('Public announcement of establishment & shareholders','Chris','2025-10-13','Planned','#7C3AED','PCG Work Programme/Comms/Press Releases/2025-10-13',[])
        ]},
        {id:'p2', title:'Phase 2 — Capital Raise to First Close', start:'2025-10-14', end:'2025-12-15', color:'#FBBF24', milestones:[
          mk('LP pipeline & first-close commitments ($5m floor)','Chris / Wayne','2025-12-15','In Progress','#16A34A','PCG Work Programme/Fundraise 2025',[]),
          mk('IM / deck refresh for PIF','Chris','2025-10-31','Planned','#2563EB','PCG Work Programme/Fundraise 2025/IM',[])
        ]},
        {id:'p3', title:'Phase 3 — Customer Growth to 1,000 by Christmas', start:'2025-10-14', end:'2025-12-24', color:'#34D399', milestones:[
          mk('Acquisition sprint — seniors & NFP channels','Kaumatua Team','2025-12-24','In Progress','#10B981','PCG Work Programme/Acquisition Engine',[]),
          mk('Onboarding & CX ops ready (billing, support, comms)','Kaumatua Ops','2025-11-30','Planned','#0EA5E9','PCG Work Programme/Ops/Onboarding',[])
        ]},
        {id:'p4', title:'Phase 4 — 2026–2028 Acquisition Engine Readiness', start:'2025-11-01', end:'2025-12-10', color:'#93C5FD', milestones:[
          mk('Growth engine blueprint (channels, FTE, budget)','Chris','2025-12-10','Planned','#6366F1','PCG Work Programme/Acquisition Engine/Blueprint',[]),
          mk('Data & dashboards — unify HTML resources','Chris','2025-11-15','Planned','#0891B2','PCG Work Programme/Tools Index',[])
        ]}
      ]
    };
  }

  let state = parseJSON(localStorage.getItem(KEY), null) || seeded();
  let cfg = parseJSON(localStorage.getItem(CFG), null) || {
    view:'calendar', calYear: today.getFullYear(), calMonth: today.getMonth(),
    filters:{phaseId:'', owner:'', status:''},
    vault:'Pacific Compass Group Vault', defaultNote:'PCG Work Programme/'
  };
  const save = ()=>{ localStorage.setItem(KEY, JSON.stringify(state)); localStorage.setItem(CFG, JSON.stringify(cfg)); };

  // ======= Obsidian =======
  const obsidianURL = (path)=> 'obsidian://open?vault='+encodeURIComponent(cfg.vault||'Pacific Compass Group Vault')+'&file='+encodeURIComponent(path||cfg.defaultNote||'');
  $('#vaultName').value = cfg.vault; $('#defaultNote').value = cfg.defaultNote;
  $('#vaultName').addEventListener('input', e=>{ cfg.vault=e.target.value; save(); });
  $('#defaultNote').addEventListener('input', e=>{ cfg.defaultNote=e.target.value; save(); });
  $('#openDefaultNote').addEventListener('click', ()=>{ try{ location.href = obsidianURL($('#defaultNote').value||cfg.defaultNote); }catch{} });
  $('#copyObsidianLink').addEventListener('click', async ()=>{
    const url=obsidianURL($('#defaultNote').value||cfg.defaultNote);
    try{ await navigator.clipboard.writeText(url); alert('Copied: '+url); }catch{ prompt('Copy:', url); }
  });

  // ======= Filters =======
  function refreshPhaseFilter(){
    const sel=$('#filterPhase');
    sel.innerHTML='<option value="">All phases</option>' + state.phases.map(p=>`<option value="${p.id}">${p.title}</option>`).join('');
    if(cfg.filters.phaseId) sel.value=cfg.filters.phaseId;
  }
  function hookFilters(){
    refreshPhaseFilter();
    $('#filterOwner').value=cfg.filters.owner||'';
    $('#filterStatus').value=cfg.filters.status||'';
    $('#filterPhase').onchange=e=>{ cfg.filters.phaseId=e.target.value; save(); render(); };
    $('#filterOwner').oninput=e=>{ cfg.filters.owner=e.target.value; save(); render(); };
    $('#filterStatus').onchange=e=>{ cfg.filters.status=e.target.value; save(); render(); };
    $('#clearFilters').onclick=()=>{ cfg.filters={phaseId:'',owner:'',status:''}; save(); hookFilters(); render(); };
  }
  const filtered = ()=>{
    const f=cfg.filters;
    return state.phases.flatMap(p=> p.milestones.map(m=>({...m,phaseId:p.id,phase:p.title}))).filter(m=>
      (!f.phaseId||m.phaseId===f.phaseId) && (!f.owner||(m.owner||'').toLowerCase().includes(f.owner.toLowerCase())) && (!f.status||(m.status||'Planned')===f.status)
    );
  };

  // ======= Modal =======
  const modal=$('#modalBackdrop');
  const M={ open(){modal.style.display='flex'}, close(){modal.style.display='none'} };
  let cur={pid:null, mid:null};

  function openEdit(pid, mid, datePreset){
    cur={pid,mid};
    const ph=state.phases.find(p=>p.id===pid);
    let m = mid? ph.milestones.find(x=>x.id===mid):null;
    if(!m){ m = mk('New Milestone','', datePreset||'', 'Planned', '#0EA5E9', cfg.defaultNote, []); ph.milestones.push(m); save(); }
    // fill
    $('#mPhase').innerHTML = state.phases.map(p=>`<option value="${p.id}">${p.title}</option>`).join('');
    $('#mPhase').value = pid;
    $('#mDue').value = m.due || '';
    $('#mTitle').value = m.title || '';
    $('#mOwner').value = m.owner || '';
    $('#mStatus').value = m.status || 'Planned';
    $('#mColor').value = m.color || '#0EA5E9';
    $('#mNote').value = m.note || '';
    $('#mLinks').value = (m.links||[]).map(l=> (l.t&&l.t!==l.u)? (l.t+'|'+l.u):l.u).join(', ');
    $('#mTasks').value = (m.tasks||[]).map(t=> (t.done?'[x] ':'')+(t.t||t)).join('\n');

    $('#saveBtn').onclick=()=>{
      let npid=$('#mPhase').value;
      if(npid!==pid){ ph.milestones=ph.milestones.filter(x=>x.id!==m.id); const ph2=state.phases.find(p=>p.id===npid); ph2.milestones.push(m); cur.pid=npid; }
      m.title=$('#mTitle').value.trim()||'Milestone';
      m.owner=$('#mOwner').value.trim();
      m.due=$('#mDue').value||'';
      m.status=$('#mStatus').value||'Planned';
      m.color=$('#mColor').value||'#0EA5E9';
      m.note=$('#mNote').value.trim();
      m.links = ($('#mLinks').value||'').split(',').map(x=>x.trim()).filter(Boolean).map(x=> x.includes('|')?({t:x.split('|')[0].trim(),u:x.split('|')[1].trim()}):({t:x,u:x}));
      m.tasks = ($('#mTasks').value||'').split(/\n/).map(x=>x.trim()).filter(Boolean).map(x=>({t:x.replace(/^\[x\]\s*/i,''),done:/^\[x\]\s*/i.test(x)}));
      save(); render(); M.close();
    };
    $('#cancelBtn').onclick=()=>{ if(!mid){ ph.milestones=ph.milestones.filter(x=>x.id!==m.id); save(); render(); } M.close(); };
    $('#deleteBtn').onclick=()=>{ if(confirm('Delete milestone?')){ ph.milestones=ph.milestones.filter(x=>x.id!==m.id); save(); render(); M.close(); } };
    M.open();
  }

  // ======= Views =======
  function monthTitle(y,m){ return months[m]+' '+y; }
  function gridDays(y,m){ const first=new Date(y,m,1); const mondayStart=(first.getDay()+6)%7; const start=new Date(y,m,1-mondayStart); const days=[]; for(let i=0;i<42;i++){ const d=new Date(start); d.setDate(start.getDate()+i); days.push(d);} return days;}
  function renderCalendar(){
    const y=Number(cfg.calYear), m=Number(cfg.calMonth);
    $('#monthTitle').textContent = monthTitle(y,m);
    const days=gridDays(y,m); const list=filtered();
    $('#calGrid').innerHTML = days.map(d=>{
      const ds=ymd(d);
      const items=list.filter(it=>it.due===ds).map(it=>`<div class="pill" data-phase="${it.phaseId}" data-id="${it.id}" style="background:${(it.color||'#99c2ff')}22;border-color:${(it.color||'#99c2ff')}66">${it.title}</div>`).join('');
      const dim=d.getMonth()!==m ? ' style="opacity:.55"' : '';
      const label=String(d.getDate()).padStart(2,'0')+' '+months[d.getMonth()];
      return `<div class="calday" data-date="${ds}"${dim}><div class="d">${label}</div>${items}</div>`;
    }).join('');
    // hook clicks
    $$('#calGrid .calday').forEach(el=> el.addEventListener('click', ev=>{ if(ev.target.classList.contains('pill')) return; const pid=($('#filterPhase').value||state.phases[0].id); openEdit(pid,null,el.getAttribute('data-date')); }));
    $$('#calGrid .pill').forEach(el=> el.addEventListener('click', ev=>{ ev.stopPropagation(); openEdit(el.dataset.phase, el.dataset.id, null); }));
  }

  function renderPhases(){
    $('#phasesView').innerHTML = state.phases.map(p=>{
      const items = p.milestones.map(m=>({...m,phaseId:p.id})).filter(m=>{
        const f=cfg.filters; return (!f.phaseId||m.phaseId===f.phaseId)&&(!f.owner||(m.owner||'').toLowerCase().includes(f.owner.toLowerCase()))&&(!f.status||(m.status||'Planned')===f.status);
      }).map(m=>{
        const links=(m.links||[]).map(l=>`<a class="linkpill" href="${l.u}" target="_blank" rel="noopener">${l.t||l.u}</a>`).join('');
        const obs = `<a class="linkpill" href="${obsidianURL(m.note||cfg.defaultNote)}">Obsidian</a>`;
        const checks=(m.tasks||[]).map((t,i)=>`<label><input type="checkbox" ${t.done?'checked':''} data-chk data-phase="${p.id}" data-id="${m.id}" data-idx="${i}"> ${t.t}</label>`).join('');
        return `<article class="ms-card" style="--ms-accent:${m.color||'#003d7a'}33">
          <div class="row" style="justify-content:space-between;align-items:center">
            <div class="row" style="gap:10px;align-items:center">
              <span class="badge" style="background:${m.color||'#003d7a'}22;border-color:${m.color||'#003d7a'}44">${m.status||'Planned'}</span>
              <strong>${m.title}</strong>
            </div>
            <span class="badge">Due: ${dNZ(m.due)}</span>
          </div>
          <div class="row" style="flex-direction:column;align-items:flex-start;gap:4px">${checks||'<span class="muted">No tasks</span>'}</div>
          <div class="row" style="flex-wrap:wrap">${links} ${obs}</div>
          <div class="row"><span class="muted">Owner: ${m.owner||'—'}</span><span style="flex:1"></span>
            <button class="ghost" data-edit data-phase="${p.id}" data-id="${m.id}">Edit</button>
            <button class="warn" data-del data-phase="${p.id}" data-id="${m.id}">Delete</button>
          </div>
        </article>`;
      }).join('');
      return `<section class="phase">
        <div class="phase-head"><div class="phase-title">${p.title}</div>
          <div class="row">
            <button class="ghost" data-add data-phase="${p.id}">Add milestone</button>
            <button class="ghost" data-rename data-phase="${p.id}">Rename</button>
            <button class="warn" data-delphase data-phase="${p.id}">Delete</button>
          </div>
        </div>
        <div class="phase-grid">${items || '<div class="muted">No milestones match filters.</div>'}</div>
      </section>`;
    }).join('');

    // hooks
    $$('[data-chk]').forEach(cb=> cb.addEventListener('change', ()=>{
      const ph=state.phases.find(p=>p.id===cb.dataset.phase); const m=ph.milestones.find(x=>x.id===cb.dataset.id);
      m.tasks[+cb.dataset.idx].done = cb.checked; save();
    }));
    $$('[data-edit]').forEach(b=> b.addEventListener('click', ()=> openEdit(b.dataset.phase, b.dataset.id, null)));
    $$('[data-del]').forEach(b=> b.addEventListener('click', ()=>{ const ph=state.phases.find(p=>p.id===b.dataset.phase); ph.milestones=ph.milestones.filter(x=>x.id!==b.dataset.id); save(); render(); }));
    $$('[data-add]').forEach(b=> b.addEventListener('click', ()=> openEdit(b.dataset.phase, null, '')));
    $$('[data-rename]').forEach(b=> b.addEventListener('click', ()=>{ const ph=state.phases.find(p=>p.id===b.dataset.phase); const t=prompt('Phase title:', ph.title); if(t!=null){ ph.title=t.trim()||ph.title; save(); refreshPhaseFilter(); render(); } }));
    $$('[data-delphase]').forEach(b=> b.addEventListener('click', ()=>{ if(confirm('Delete phase and all milestones?')){ state.phases=state.phases.filter(p=>p.id!==b.dataset.phase); save(); refreshPhaseFilter(); render(); } }));
  }

  function renderList(){
    const list = filtered().sort((a,b)=> (a.due||'9999').localeCompare(b.due||'9999'));
    $('#listContainer').innerHTML = list.length? list.map(it=>`<div class="row" style="justify-content:space-between;border:1px solid #e6eef9;border-radius:12px;padding:10px;margin:6px 0;background:#fff">
      <div><div><span class="badge">Phase</span> ${it.phase}</div><div><strong>${it.title}</strong></div>
      <div class="muted">Due ${dNZ(it.due)} · ${it.status||'Planned'} · Owner ${it.owner||'—'}</div></div>
      <div class="row"><a class="linkpill" href="${obsidianURL(it.note||cfg.defaultNote)}">Obsidian</a><button class="ghost" data-edit data-phase="${it.phaseId}" data-id="${it.id}">Edit</button></div>
    </div>`).join('') : '<div class="muted">No milestones match filters.</div>';
    $$('#listContainer [data-edit]').forEach(b=> b.addEventListener('click', ()=> openEdit(b.dataset.phase, b.dataset.id, null)));
  }

  function renderKanban(){
    const cols=['Planned','In Progress','Blocked','Done']; const list=filtered();
    $('#kanbanView').innerHTML = `<div class="kanban">${
      cols.map(c=>{
        const items=list.filter(it=> (it.status||'Planned')===c).map(it=>`<div class="row" style="justify-content:space-between;border:1px solid #e6eef9;border-radius:12px;padding:10px;margin:6px 0;background:#fff">
          <div><div><strong>${it.title}</strong></div><div class="muted">${dNZ(it.due)} · ${it.phase}</div></div>
          <div class="row"><button class="ghost" data-edit data-phase="${it.phaseId}" data-id="${it.id}">Edit</button></div>
        </div>`).join('');
        return `<div class="kcol"><h3>${c}</h3>${items || '<div class="muted">—</div>'}</div>`;
      }).join('')
    }</div>`;
    $$('#kanbanView [data-edit]').forEach(b=> b.addEventListener('click', ()=> openEdit(b.dataset.phase, b.dataset.id, null)));
  }

  function renderTimeline(){
    const list=filtered().sort((a,b)=> (a.due||'9999').localeCompare(b.due||'9999'));
    if(!list.length){ $('#timelineView').innerHTML='<div class="muted">No milestones match filters.</div>'; return; }
    const CHRISTMAS=new Date('2025-12-25T00:00:00'); const now=new Date();
    $('#timelineView').innerHTML = `<div class="timeline">${
      list.map(it=>{
        const total=Math.max(1,(CHRISTMAS-now)/86400000);
        const toDue=it.due? Math.max(0,(new Date(it.due)-now)/86400000): total;
        const pct=Math.max(2,Math.min(100,Math.round(((total-toDue)/total)*100)));
        return `<div class="bar"><i style="width:${pct}%"></i><div class="row" style="position:relative;z-index:1;gap:10px;align-items:center"><span class="badge">${it.phase}</span><strong>${it.title}</strong><span class="muted">${dNZ(it.due)}</span><span class="badge">${it.status||'Planned'}</span><span style="flex:1"></span><button class="ghost" data-edit data-phase="${it.phaseId}" data-id="${it.id}">Edit</button></div></div>`;
      }).join('')
    }</div>`;
    $$('#timelineView [data-edit]').forEach(b=> b.addEventListener('click', ()=> openEdit(b.dataset.phase, b.dataset.id, null)));
  }

  function render(){
    if(cfg.view==='calendar') renderCalendar();
    if(cfg.view==='phases') renderPhases();
    if(cfg.view==='list') renderList();
    if(cfg.view==='kanban') renderKanban();
    if(cfg.view==='timeline') renderTimeline();
  }

  // ======= Tabs & nav =======
  $$('#tabs .tab').forEach(t=> t.addEventListener('click', ()=>{
    $$('#tabs .tab').forEach(x=>x.classList.remove('active')); t.classList.add('active'); cfg.view=t.dataset.view; save();
    ['phases','list','calendar','kanban','timeline'].forEach(v=> $('#'+v+'View').classList.toggle('hidden', v!==cfg.view));
    render();
  }));
  $('#prevM').onclick=()=>{ const d=new Date(cfg.calYear, cfg.calMonth-1, 1); cfg.calYear=d.getFullYear(); cfg.calMonth=d.getMonth(); save(); render(); };
  $('#nextM').onclick=()=>{ const d=new Date(cfg.calYear, cfg.calMonth+1, 1); cfg.calYear=d.getFullYear(); cfg.calMonth=d.getMonth(); save(); render(); };
  $('#todayM').onclick=()=>{ const d=new Date(); cfg.calYear=d.getFullYear(); cfg.calMonth=d.getMonth(); save(); render(); };
  $('#addPhase').onclick=()=>{ const t=prompt('New phase title','New Phase'); if(t){ state.phases.push({id:Math.random().toString(36).slice(2,10), title:t.trim(), start:'', end:'', color:'#FFD93D', milestones:[]}); save(); refreshPhaseFilter(); render(); } };

  // Export/Import/Reset
  $('#export').onclick=()=>{ const blob=new Blob([JSON.stringify({state,cfg},null,2)],{type:'application/json'}); const a=document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='pcg-dashboard-v260.json'; a.click(); URL.revokeObjectURL(a.href); };
  $('#importFile').addEventListener('change', e=>{
    const f=e.target.files[0]; if(!f) return; const r=new FileReader(); r.onload=()=>{
      try{ const data=JSON.parse(r.result); if(!data.state?.phases) throw new Error('Invalid file');
        state=data.state; cfg={...cfg, ...(data.cfg||{})};
        if(!cfg.view) cfg.view='calendar';
        if(!(Number.isInteger(cfg.calYear))) cfg.calYear=today.getFullYear();
        if(!(Number.isInteger(cfg.calMonth))) cfg.calMonth=today.getMonth();
        save(); hookFilters(); // switch to saved view
        ['phases','list','calendar','kanban','timeline'].forEach(v=> $('#'+v+'View').classList.toggle('hidden', v!==cfg.view));
        $$('#tabs .tab').forEach(x=>x.classList.toggle('active', x.dataset.view===cfg.view));
        render();
      }catch(err){ alert('Import failed: '+err.message); }
    }; r.readAsText(f);
  });
  $('#reset').onclick=()=>{ if(confirm('Factory reset?')){ state=seeded(); cfg={view:'calendar',calYear:today.getFullYear(),calMonth:today.getMonth(),filters:{phaseId:'',owner:'',status:''},vault:'Pacific Compass Group Vault',defaultNote:'PCG Work Programme/'}; save(); hookFilters(); ['phases','list','calendar','kanban','timeline'].forEach(v=> $('#'+v+'View').classList.toggle('hidden', v!=='calendar')); $$('#tabs .tab').forEach(x=>x.classList.toggle('active', x.dataset.view==='calendar')); render(); } };

  // init
  hookFilters();
  ['phases','list','calendar','kanban','timeline'].forEach(v=> $('#'+v+'View').classList.toggle('hidden', v!==cfg.view));
  $$('#tabs .tab').forEach(x=>x.classList.toggle('active', x.dataset.view===cfg.view));
  render();
})();
</script>
</body>
</html>
