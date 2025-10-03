<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>MapaHub LAC — Downloads</title>
<style>
  :root{--bg:#0f1720;--card:#0b1220;--accent:#10b981;--muted:#9ca3af;--glass:rgba(255,255,255,0.03)}
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,Arial;background:linear-gradient(180deg,#071129,#0b1724);color:#e6eef6;-webkit-font-smoothing:antialiased}
  header{display:flex;gap:12px;align-items:center;padding:18px;max-width:1200px;margin:18px auto}
  h1{margin:0;font-size:20px}
  .container{max-width:1200px;margin:0 auto;padding:0 16px 70px}
  .controls{display:flex;gap:10px;flex-wrap:wrap;align-items:center;margin:8px 0}
  .search{flex:1;min-width:180px}
  input,select,button,textarea{font-family:inherit}
  input[type=text],select,textarea{background:var(--card);border:1px solid rgba(255,255,255,0.04);color:inherit;padding:10px;border-radius:8px;outline:none}
  button{background:var(--accent);border:none;color:#042018;padding:10px 12px;border-radius:8px;cursor:pointer;font-weight:700}
  button.secondary{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--muted);font-weight:600}
  .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:12px;margin-top:14px}
  .card{background:linear-gradient(180deg,rgba(255,255,255,0.02),var(--glass));padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.03)}
  .card h3{margin:0 0 6px 0;font-size:16px}
  .meta{color:var(--muted);font-size:13px;margin-bottom:6px}
  .tags{display:flex;gap:6px;flex-wrap:wrap;margin-top:8px}
  .tag{background:rgba(255,255,255,0.04);padding:6px 8px;border-radius:999px;font-size:12px;color:var(--muted)}
  .card .row{display:flex;gap:8px;align-items:center;justify-content:space-between;margin-top:10px}
  .download{background:#0ea5a3;color:#021; padding:8px 10px;border-radius:8px;font-weight:700}
  footer{max-width:1200px;margin:20px auto;color:var(--muted);text-align:center;padding:20px 16px}
  /* admin modal */
  .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:linear-gradient(180deg,rgba(0,0,0,0.55),rgba(0,0,0,0.6));z-index:999}
  .modal .box{width:100%;max-width:760px;background:#071424;padding:16px;border-radius:12px;border:1px solid rgba(255,255,255,0.04)}
  label{display:block;margin-top:8px;font-size:13px;color:var(--muted)}
  textarea{min-height:80px}
  .small{font-size:13px;color:var(--muted)}
  .topbar{display:flex;gap:10px;align-items:center;justify-content:space-between}
  .hint{font-size:13px;color:var(--muted);margin-left:8px}
  .ads-space{margin:12px 0;padding:10px;border-radius:8px;background:linear-gradient(180deg,rgba(255,255,255,0.015),rgba(255,255,255,0.01));text-align:center;color:var(--muted)}
  @media (max-width:640px){ header{padding:12px} .controls{flex-direction:column;align-items:stretch}}
</style>
</head>
<body>

<header>
  <div>
    <h1>MapaHub LAC</h1>
    <div class="small">Central de downloads de mapas — cole o link do MediaFire e compartilhe.</div>
  </div>
  <div style="margin-left:auto;display:flex;gap:8px;align-items:center">
    <button id="openAdmin">Admin (local)</button>
    <button class="secondary" id="exportBtn">Exportar JSON</button>
    <button class="secondary" id="importBtn">Importar JSON</button>
  </div>
</header>

<div class="container">
  <!-- espaço para anúncios (cole aqui o script do AdSense ou outro) -->
  <div class="ads-space" id="adTop">
    <!-- COLE AQUI O CÓDIGO DO SEU AD (ex: AdSense) -->
    <div class="small">Espaço para anúncio — cole o script do AdSense ou outro aqui.</div>
  </div>

  <div class="controls">
    <input type="text" id="searchInput" class="search" placeholder="Buscar por nome, tags ou autor...">
    <select id="filterTag">
      <option value="">Filtrar por tag</option>
    </select>
    <select id="sortBy">
      <option value="new">Mais recentes</option>
      <option value="name">Nome A→Z</option>
      <option value="popular">Mais acessados (estimado)</option>
    </select>
    <button id="clearFilter" class="secondary">Limpar</button>
  </div>

  <div id="list" class="grid" aria-live="polite">
    <!-- cards são renderizados por JS -->
  </div>

  <div class="ads-space" id="adBottom">
    <div class="small">Rodapé de anúncio — insira seu script de anúncios aqui.</div>
  </div>
</div>

<footer>
  <div class="small">Use este site apenas para conteúdo que você tem direito de distribuir. Para monetizar com anúncios (AdSense), use domínio próprio e siga as políticas do provedor de anúncios.</div>
</footer>

<!-- Admin modal (localStorage) -->
<div id="modal" class="modal" role="dialog" aria-modal="true">
  <div class="box">
    <div class="topbar">
      <strong>Admin — Adicionar / Editar mapa</strong>
      <div>
        <button id="closeModal" class="secondary">Fechar</button>
      </div>
    </div>

    <label>Título</label>
    <input type="text" id="mTitle" placeholder="Nome do mapa (ex: Centro da cidade)">

    <label>Descrição</label>
    <textarea id="mDesc" placeholder="Descrição curta"></textarea>

    <label>Tags (separe por vírgula)</label>
    <input type="text" id="mTags" placeholder="ex: cidade, rp, pvp">

    <label>Link MediaFire</label>
    <input type="text" id="mLink" placeholder="Cole o link do MediaFire aqui (começa com https://www.mediafire.com/...)">

    <div style="display:flex;gap:8px;margin-top:12px;align-items:center">
      <button id="saveMap">Salvar</button>
      <button id="deleteMap" class="secondary" style="display:none">Excluir</button>
      <div class="hint">Você pode importar/exportar a lista com JSON.</div>
    </div>

    <hr style="margin:12px 0;border-color:rgba(255,255,255,0.03)">

    <div class="small">Importar JSON:</div>
    <textarea id="importArea" placeholder='Cole aqui JSON exportado (array de objetos)'></textarea>
    <div style="display:flex;gap:8px;margin-top:8px">
      <button id="doImport" class="secondary">Importar agora (substitui lista)</button>
      <button id="appendImport" class="secondary">Importar (adicionar)</button>
    </div>
  </div>
</div>

<script>
(() => {
  // ---------- storage keys ----------
  const STORAGE_KEY = 'mapahub_lac_v1';

  // ---------- sample data (empty at start) ----------
  let items = [
    // example structure:
    // { id:'uuid', title:'Mapa X', desc:'...', tags:['cidade','rp'], link:'https://www.mediafire.com/..', created:timestamp, views:0 }
  ];

  // ---------- helpers ----------
  function uid(){ return 'm_'+Math.random().toString(36).slice(2,9); }
  function save(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(items)); render(); }
  function load(){ try{ const j = JSON.parse(localStorage.getItem(STORAGE_KEY)||'[]'); if(Array.isArray(j)) items = j; }catch(e){ items=[] } }
  function formatDate(ts){ const d=new Date(ts||Date.now()); return d.toLocaleString(); }

  // ---------- DOM refs ----------
  const listEl = document.getElementById('list');
  const searchInput = document.getElementById('searchInput');
  const filterTag = document.getElementById('filterTag');
  const sortBy = document.getElementById('sortBy');
  const clearFilter = document.getElementById('clearFilter');

  const modal = document.getElementById('modal');
  const openAdmin = document.getElementById('openAdmin');
  const closeModal = document.getElementById('closeModal');
  const mTitle = document.getElementById('mTitle');
  const mDesc = document.getElementById('mDesc');
  const mTags = document.getElementById('mTags');
  const mLink = document.getElementById('mLink');
  const saveMapBtn = document.getElementById('saveMap');
  const deleteMapBtn = document.getElementById('deleteMap');
  const importArea = document.getElementById('importArea');
  const doImport = document.getElementById('doImport');
  const appendImport = document.getElementById('appendImport');
  const exportBtn = document.getElementById('exportBtn');
  const importBtn = document.getElementById('importBtn');

  let editingId = null;

  // ---------- init ----------
  load(); render(); populateFilterTags();

  // ---------- render ----------
  function render(){
    listEl.innerHTML = '';
    // get filtered list
    const q = (searchInput.value||'').toLowerCase().trim();
    const tag = filterTag.value;
    let arr = items.slice();

    // sort
    if(sortBy.value === 'new') arr.sort((a,b)=> (b.created||0)-(a.created||0));
    else if(sortBy.value === 'name') arr.sort((a,b)=> a.title.localeCompare(b.title));
    else if(sortBy.value === 'popular') arr.sort((a,b)=> (b.views||0)-(a.views||0));

    // filter by tag
    if(tag){
      arr = arr.filter(it => (it.tags||[]).includes(tag));
    }
    // search
    if(q){
      arr = arr.filter(it => {
        return (it.title||'').toLowerCase().includes(q) ||
               (it.desc||'').toLowerCase().includes(q) ||
               (it.tags||[]).join(' ').toLowerCase().includes(q);
      });
    }

    if(arr.length === 0){
      listEl.innerHTML = '<div style="grid-column:1/-1;color:var(--muted);padding:20px;text-align:center">Nenhum mapa encontrado — adicione no Admin.</div>';
      return;
    }

    arr.forEach(it => {
      const card = document.createElement('div'); card.className='card';
      card.innerHTML = `
        <h3>${escapeHtml(it.title)}</h3>
        <div class="meta small">Adicionado: ${formatDate(it.created)} • ${it.views||0} downloads (estimado)</div>
        <div class="small">${escapeHtml(it.desc||'Sem descrição')}</div>
        <div class="tags">${(it.tags||[]).map(t => `<span class="tag">${escapeHtml(t)}</span>`).join('')}</div>
        <div class="row">
          <div style="display:flex;gap:8px;align-items:center">
            <button class="download" data-id="${it.id}">DOWNLOAD</button>
            <button class="secondary copy" data-id="${it.id}">Copiar link</button>
          </div>
          <div style="font-size:13px;color:var(--muted)"><button class="secondary edit" data-id="${it.id}">Editar</button></div>
        </div>
      `;
      listEl.appendChild(card);
    });

    // attach events
    listEl.querySelectorAll('button.download').forEach(b=>{
      b.addEventListener('click', e=>{
        const id = e.currentTarget.dataset.id;
        const it = items.find(x=>x.id===id); if(!it) return;
        // increment views (estimativa)
        it.views = (it.views||0) + 1;
        save();
        window.open(it.link, '_blank');
      });
    });
    listEl.querySelectorAll('button.copy').forEach(b=>{
      b.addEventListener('click', e=>{
        const id = e.currentTarget.dataset.id; const it = items.find(x=>x.id===id); if(!it) return;
        navigator.clipboard?.writeText(it.link).then(()=> alert('Link copiado para área de transferência.'));
      });
    });
    listEl.querySelectorAll('button.edit').forEach(b=>{
      b.addEventListener('click', e=>{
        const id = e.currentTarget.dataset.id; const it = items.find(x=>x.id===id); if(!it) return;
        openModalForEdit(it);
      });
    });

    populateFilterTags();
  }

  function populateFilterTags(){
    const set = new Set();
    items.forEach(it => (it.tags||[]).forEach(t => t && set.add(t)));
    const arr = Array.from(set).sort();
    filterTag.innerHTML = '<option value="">Filtrar por tag</option>' + arr.map(t => `<option value="${escapeHtmlAttr(t)}">${escapeHtml(t)}</option>`).join('');
  }

  // ---------- modal / admin ----------
  openAdmin.addEventListener('click', ()=> { openModalForNew(); });
  closeModal.addEventListener('click', ()=> { modal.style.display='none'; clearModal(); });

  function openModalForNew(){
    editingId = null;
    deleteMapBtn.style.display='none';
    mTitle.value=''; mDesc.value=''; mTags.value=''; mLink.value='';
    modal.style.display='flex';
  }
  function openModalForEdit(it){
    editingId = it.id;
    deleteMapBtn.style.display='inline-block';
    mTitle.value = it.title || '';
    mDesc.value = it.desc || '';
    mTags.value = (it.tags||[]).join(',');
    mLink.value = it.link || '';
    modal.style.display='flex';
  }

  saveMapBtn.addEventListener('click', ()=>{
    const title = mTitle.value.trim();
    const desc = mDesc.value.trim();
    const link = mLink.value.trim();
    const tags = mTags.value.split(',').map(x=>x.trim()).filter(Boolean);
    if(!title || !link){ alert('Título e link são obrigatórios.'); return; }
    if(!isMediafireLink(link)){ if(!confirm('Link não parece ser MediaFire. Continuar?')) return; }

    if(editingId){
      const it = items.find(x=>x.id===editingId);
      if(it){
        it.title = title; it.desc = desc; it.tags = tags; it.link = normalizeLink(link);
        save();
      }
    } else {
      items.unshift({ id: uid(), title, desc, tags, link:normalizeLink(link), created:Date.now(), views:0 });
      save();
    }
    modal.style.display='none'; clearModal();
  });

  deleteMapBtn.addEventListener('click', ()=>{
    if(!editingId) return;
    if(confirm('Excluir este mapa?')) {
      items = items.filter(x=>x.id!==editingId); save(); modal.style.display='none'; clearModal();
    }
  });

  function clearModal(){ editingId=null; mTitle.value=''; mDesc.value=''; mTags.value=''; mLink.value=''; importArea.value=''; }

  // ---------- import / export ----------
  exportBtn.addEventListener('click', ()=>{
    const blob = new Blob([JSON.stringify(items, null, 2)], {type:'application/json'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a'); a.href = url; a.download = 'mapas_lac_export.json'; document.body.appendChild(a); a.click(); a.remove();
    URL.revokeObjectURL(url);
  });

  importBtn.addEventListener('click', ()=> {
    // open modal import area
    modal.style.display='flex';
  });

  doImport.addEventListener('click', ()=> {
    try{
      const j = JSON.parse(importArea.value);
      if(!Array.isArray(j)) throw 'JSON deve ser um array';
      // map objects to our shape
      items = j.map(it => ({
        id: it.id || uid(),
        title: it.title || 'Sem título',
        desc: it.desc || '',
        tags: (it.tags||[]).map(String),
        link: normalizeLink(it.link||''),
        created: it.created || Date.now(),
        views: it.views || 0
      }));
      save(); modal.style.display='none'; clearModal();
    }catch(e){ alert('Erro ao importar JSON: ' + e); }
  });

  appendImport.addEventListener('click', ()=> {
    try{
      const j = JSON.parse(importArea.value);
      if(!Array.isArray(j)) throw 'JSON deve ser um array';
      const added = j.map(it => ({
        id: it.id || uid(),
        title: it.title || 'Sem título',
        desc: it.desc || '',
        tags: (it.tags||[]).map(String),
        link: normalizeLink(it.link||''),
        created: it.created || Date.now(),
        views: it.views || 0
      }));
      items = items.concat(added); save(); modal.style.display='none'; clearModal();
    }catch(e){ alert('Erro ao importar JSON: ' + e); }
  });

  // ---------- search / filters ----------
  searchInput.addEventListener('input', ()=> render());
  filterTag.addEventListener('change', ()=> render());
  sortBy.addEventListener('change', ()=> render());
  clearFilter.addEventListener('click', ()=> { searchInput.value=''; filterTag.value=''; sortBy.value='new'; render(); });

  // ---------- utilities ----------
  function isMediafireLink(u){
    if(!u) return false;
    try{ const url = new URL(u); return url.hostname.includes('mediafire.com'); }catch(e){ return false; }
  }
  function normalizeLink(u){
    // if mediafire file page has redirect link, keep as-is. No heavy checks.
    return u.trim();
  }
  function escapeHtml(s){ return String(s||'').replace(/[&<>"']/g, (m)=> ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }
  function escapeHtmlAttr(s){ return escapeHtml(s).replace(/"/g,'&quot;'); }

  // ---------- initial sample (only if empty) ----------
  if(items.length === 0){
    items.push({
      id: uid(),
      title: 'Exemplo — Centro RP',
      desc: 'Mapa de exemplo. Substitua pelo seu link do MediaFire.',
      tags:['centro','rp'],
      link: 'https://www.mediafire.com/example_link',
      created: Date.now(),
      views: 0
    });
    save();
  }

  // ---------- click outside modal to close ----------
  modal.addEventListener('click', e => { if(e.target === modal) { modal.style.display='none'; clearModal(); } });

  // ---------- small UX: open edit from URL param ?edit=id ----------
  const params = new URLSearchParams(location.search);
  if(params.get('edit')){
    const id = params.get('edit');
    const it = items.find(x=>x.id===id);
    if(it) openModalForEdit(it);
  }

  // ---------- end ----------
})();
</script>
</body>
</html>