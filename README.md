<!doctype html>

<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>NEXOS LAC — Protótipo Completo</title>
  <meta name="description" content="NEXOS LAC — repositório de mods, mapas (.txt), comandos e servidores de Los Angeles Crimes. Protótipo mobile-first." />
  <style>
    :root{--bg:#040504;--panel:#071009;--accent:#00ff88;--accent2:#00cc66;--muted:#9aa896;--glass:rgba(255,255,255,0.03);--radius:12px}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,system-ui,Arial;background:linear-gradient(180deg,#001207,#03110a);color:#eaffea}
    .appbar{position:sticky;top:0;z-index:60;display:flex;align-items:center;gap:12px;padding:12px;background:linear-gradient(90deg,var(--accent2),var(--accent));}
    .logo{width:52px;height:52px;border-radius:10px;background:linear-gradient(180deg,#d9ffd9,#aaffcc);display:flex;align-items:center;justify-content:center;font-weight:900;color:#022}
    .brand{display:flex;flex-direction:column}
    .brand h1{margin:0;font-size:18px;color:#022}
    .brand p{margin:0;font-size:12px;color:#022;opacity:0.9}
    .actions{margin-left:auto;display:flex;gap:8px}
    .btn{padding:8px 12px;border-radius:10px;border:none;background:transparent;color:var(--accent);cursor:pointer;font-weight:700}
    .btn.primary{background:linear-gradient(90deg,var(--accent2),var(--accent));color:#022}
    .wrap{max-width:1100px;margin:12px auto;padding:12px}
    .grid{display:grid;grid-template-columns:1fr 360px;gap:16px}
    @media(max-width:920px){.grid{grid-template-columns:1fr}}
    .card{background:var(--panel);padding:14px;border-radius:12px;border:1px solid rgba(255,255,255,0.02);box-shadow:0 6px 18px rgba(0,0,0,0.6)}
    h2{margin:0 0 8px 0}
    input,textarea,select{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:var(--glass);color:#eaffea}
    textarea{min-height:90px}
    .row{display:flex;gap:8px}
    .muted{color:var(--muted);font-size:13px}
    .posts{display:flex;flex-direction:column;gap:12px}
    .post{display:flex;gap:12px;padding:12px;border-radius:12px;background:linear-gradient(180deg,rgba(0,0,0,0.25),rgba(255,255,255,0.01));}
    .p-left{flex:1}
    .p-right{width:160px;display:flex;flex-direction:column;gap:6px;align-items:flex-end}
    .title{font-weight:800;color:var(--accent)}
    .meta{font-size:13px;color:var(--muted)}
    .tags{display:flex;gap:6px;flex-wrap:wrap;margin-top:6px}
    .tag{background:var(--accent2);color:#022;padding:4px 8px;border-radius:8px;font-weight:700;font-size:12px}
    .small{font-size:12px;color:var(--muted)}
    .chat-box{height:300px;overflow:auto;padding:8px;display:flex;flex-direction:column;gap:8px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent);border-radius:8px}
    .msg{padding:8px;border-radius:10px}
    .msg.me{align-self:flex-end;background:rgba(0,255,140,0.12)}
    .msg.other{align-self:flex-start;background:rgba(255,255,255,0.02)}
    .server-card{padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.03);background:rgba(0,0,0,0.2);margin-bottom:8px}
    .bottomnav{position:fixed;left:0;right:0;bottom:0;padding:8px;background:linear-gradient(180deg,rgba(0,0,0,0.4),rgba(0,0,0,0.8));display:flex;gap:8px;justify-content:space-around}
    .bn{padding:10px 14px;border-radius:12px;background:rgba(255,255,255,0.02);color:var(--muted);font-weight:700}
    .bn.active{background:linear-gradient(90deg,var(--accent2),var(--accent));color:#022}
    .ad{height:90px;border-radius:10px;background:linear-gradient(90deg,#002f1a,#003a1f);display:flex;align-items:center;justify-content:center;color:#bfffd0}
    footer{text-align:center;padding:12px;color:var(--muted);margin-bottom:72px}
  </style>
</head>
<body>
  <div class="appbar">
    <div class="logo">NL</div>
    <div class="brand">
      <h1>NEXOS LAC</h1>
      <p>Mods • Mapas (.txt) • Comandos • Servidores</p>
    </div>
    <div class="actions">
      <button class="btn" onclick="changeName()">Nome</button>
      <button class="btn" onclick="promptAdmin()">Admin</button>
    </div>
  </div>  <div class="wrap">
    <div class="grid">
      <main>
        <section class="card" id="postSection">
          <h2>Publicar (Mods / Mapas / Comandos)</h2>
          <div class="row" style="margin-top:8px">
            <select id="category"><option value="mod">Mod</option><option value="map">Mapa (.txt)</option><option value="cmd">Comando</option></select>
            <input id="version" placeholder="Versão (opcional)" />
          </div>
          <input id="title" placeholder="Título" style="margin-top:8px" />
          <textarea id="desc" placeholder="Descrição / instruções"></textarea>
          <input id="tagsInput" placeholder="Tags (separadas por vírgula)" />
          <div class="row" style="align-items:center">
            <input id="fileInput" type="file" accept=".txt,.zip,.json,.lua,.png,.jpg,.jpeg" />
            <button class="btn primary" onclick="publishPost()">Publicar</button>
          </div>
          <div class="muted" style="margin-top:8px">Dica: mapas em .txt serão visualizáveis e pesquisáveis. Arquivos ficam no navegador (localStorage).</div>
        </section><section class="card" id="repoSection" style="margin-top:12px">
      <h2>Repositório</h2>
      <div class="row" style="margin-bottom:8px"><input id="searchInput" placeholder="Pesquisar..." /><select id="filterCat" style="max-width:140px"><option value="all">Todas</option><option value="mod">Mods</option><option value="map">Mapas</option><option value="cmd">Comandos</option></select></div>
      <div class="posts" id="postsList"></div>
      <div class="ad" style="margin-top:12px">Espaço para anúncios</div>
    </section>

    <section class="card" id="serversSection" style="margin-top:12px">
      <h2>Servidores</h2>
      <div style="display:flex;gap:8px;margin-bottom:8px"><button class="btn" onclick="switchServers('rp',this)" id="tab_rp">SERVIDORES ROLEPLAY</button><button class="btn" onclick="switchServers('pvp',this)" id="tab_pvp">SERVIDORES PVP</button></div>
      <div id="serversList"></div>
    </section>

  </main>

  <aside>
    <div class="card">
      <h2>Fórum / Chat</h2>
      <div class="chat-box" id="chatBox"></div>
      <div style="display:flex;gap:8px;margin-top:8px"><input id="chatInput" placeholder="Mensagem..." /><button class="btn primary" onclick="sendChat()">Enviar</button></div>
    </div>

    <div class="card" style="margin-top:12px">
      <h2>Painel Admin</h2>
      <div class="muted">Status: <span id="adminStatus">—</span></div>
      <div style="display:flex;gap:8px;margin-top:8px"><button class="btn" onclick="banByNickPrompt()">Banir por Nick</button><button class="btn" onclick="unbanPrompt()">Remover ban</button><button class="btn" onclick="viewBans()">Ver bans</button></div>
      <div style="margin-top:10px"><button class="btn" onclick="clearAll()">Limpar dados locais</button></div>
    </div>

    <div class="card" style="margin-top:12px">
      <h2>Minha conta</h2>
      <div class="muted">Nick: <span id="displayName"></span></div>
      <div class="muted">ID local: <span id="displayId"></span></div>
      <div style="margin-top:8px"><button class="btn" onclick="changeName()">Trocar nome</button></div>
    </div>

  </aside>
</div>

<footer>Protótipo NEXOS LAC — protótipo mobile-first • Admin code definido</footer>

  </div>  <div class="bottomnav">
    <div class="bn active" data-section="repo" onclick="navTo('repo')">Repositório</div>
    <div class="bn" data-section="post" onclick="navTo('post')">Postar</div>
    <div class="bn" data-section="servers" onclick="navTo('servers')">Servidores</div>
    <div class="bn" data-section="chat" onclick="navTo('chat')">Chat</div>
  </div>  <div id="modal" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,0.6);align-items:center;justify-content:center;z-index:80">
    <div style="background:var(--panel);padding:12px;border-radius:10px;max-width:90%;width:720px;max-height:80vh;overflow:auto" id="modalContent"></div>
  </div>  <script>
    // Keys
    const K_POSTS = 'nexos_posts_v3';
    const K_FILES = 'nexos_files_v3';
    const K_CHAT = 'nexos_chat_v3';
    const K_SERVERS = 'nexos_servers_v3';
    const K_USER = 'nexos_user_v3';
    const K_BANS = 'nexos_bans_v3';

    // State
    let posts = JSON.parse(localStorage.getItem(K_POSTS) || '[]');
    let files = JSON.parse(localStorage.getItem(K_FILES) || '{}');
    let chat = JSON.parse(localStorage.getItem(K_CHAT) || '[]');
    let servers = JSON.parse(localStorage.getItem(K_SERVERS) || '{"rp":[],"pvp":[]}'); if(typeof servers==='string') servers = JSON.parse(servers);
    let bans = JSON.parse(localStorage.getItem(K_BANS) || '{"nicks":[],"posts":[]}');
    let user = JSON.parse(localStorage.getItem(K_USER) || '{}');
    if(!user.id){ user.id = Date.now().toString(36)+Math.random().toString(36).slice(2,6); user.name = prompt('Escolha um nick:') || 'Visitante'; user.admin = false; localStorage.setItem(K_USER, JSON.stringify(user)); }

    // Admin code
    const ADMIN_CODE = '1525'; // atualizado

    // UI init
    document.getElementById('displayName').innerText = user.name; document.getElementById('displayId').innerText = user.id; document.getElementById('adminStatus').innerText = user.admin ? 'Admin (neste navegador)' : 'Não admin';
    document.getElementById('searchInput').addEventListener('input', renderPosts);

    // Navigation bottom
    function navTo(section){ document.querySelectorAll('.bn').forEach(b=>b.classList.remove('active')); document.querySelector('.bn[data-section="'+(section==='repo'?'repo':'repo')+'"]').classList.add('active');
      if(section==='repo'){ document.getElementById('repoSection').scrollIntoView({behavior:'smooth'});} if(section==='post'){ document.getElementById('postSection').scrollIntoView({behavior:'smooth'});} if(section==='servers'){ document.getElementById('serversSection').scrollIntoView({behavior:'smooth'});} if(section==='chat'){ document.getElementById('chatBox').scrollIntoView({behavior:'smooth'});} }

    // Publish post with tags
    function publishPost(){
      const title = document.getElementById('title').value.trim(); const desc = document.getElementById('desc').value.trim(); const cat = document.getElementById('category').value; const version = document.getElementById('version').value.trim();
      const tagStr = document.getElementById('tagsInput').value.trim(); const tags = tagStr?tagStr.split(',').map(t=>t.trim()).filter(t=>t):[];
      const f = document.getElementById('fileInput').files[0]; if(!title||!f){ alert('Título e arquivo são necessários'); return }
      const id = Date.now().toString(36)+Math.random().toString(36).slice(2,6);
      const reader = new FileReader(); reader.onload = (e)=>{
        files[id] = {data:e.target.result,name:f.name,type:f.type,size:f.size}; localStorage.setItem(K_FILES, JSON.stringify(files));
        const p = {id,title,desc,cat,version,fileId:id,fileName:f.name,author:user.name,authorId:user.id,time:new Date().toLocaleString(),ts:Date.now(),tags,likes:[],comments:[],downloads:0};
        posts.push(p); localStorage.setItem(K_POSTS, JSON.stringify(posts));
        // reset
        document.getElementById('title').value=''; document.getElementById('desc').value=''; document.getElementById('fileInput').value=''; document.getElementById('version').value=''; document.getElementById('tagsInput').value='';
        renderPosts(); alert('Post publicado!');
      }; reader.readAsDataURL(f);
    }

    // Render posts
    function renderPosts(){ const q = document.getElementById('searchInput').value.trim().toLowerCase(); const filter = document.getElementById('filterCat').value; const container = document.getElementById('postsList'); container.innerHTML='';
      posts.slice().reverse().forEach(p=>{
        if(bans.nicks.includes(p.author)) return; if(bans.posts.includes(p.id)) return; if(filter!=='all'&&p.cat!==filter) return;
        if(q && !(p.title.toLowerCase().includes(q)||p.desc.toLowerCase().includes(q)||p.author.toLowerCase().includes(q)||p.tags.some(t=>t.toLowerCase().includes(q)))) return;
        const el = document.createElement('div'); el.className='post';
        const left = document.createElement('div'); left.className='p-left';
        let tagsHtml = p.tags && p.tags.length ? `<div class="tags">${p.tags.map(t=>`<span class="tag">${escapeHtml(t)}</span>`).join('')}</div>` : '';
        left.innerHTML = `<div class="title">${escapeHtml(p.title)}</div><div class="meta">${escapeHtml(p.desc.substring(0,200))}</div>${tagsHtml}<div class="small">${escapeHtml(p.author)} • ${escapeHtml(p.time)} • ${escapeHtml(p.fileName)}</div>`;
        const right = document.createElement('div'); right.className='p-right';
        // preview button
        const viewBtn = document.createElement('button'); viewBtn.className='btn'; viewBtn.innerText='Visualizar'; viewBtn.onclick = ()=>previewFile(p.fileId);
        const dlBtn = document.createElement('button'); dlBtn.className='btn'; dlBtn.innerText='Download'; dlBtn.onclick = ()=>{ downloadFile(p.fileId,p.fileName); // inc counter
          const post = posts.find(x=>x.id===p.id); if(post){ post.downloads = (post.downloads||0)+1; localStorage.setItem(K_POSTS, JSON.stringify(posts)); renderPosts(); }
        };
        const likeBtn = document.createElement('button'); likeBtn.className='btn'; likeBtn.innerText = `❤ ${p.likes?p.likes.length:0}`; likeBtn.onclick = ()=>toggleLike(p.id);
        const commentsBtn = document.createElement('button'); commentsBtn.className='btn'; commentsBtn.innerText = `💬 ${p.comments?p.comments.length:0}`; commentsBtn.onclick = ()=>openComments(p.id);
        right.appendChild(viewBtn); right.appendChild(dlBtn); right.appendChild(likeBtn); right.appendChild(commentsBtn);
        if(user.admin){ const banP = document.createElement('button'); banP.className='btn'; banP.innerText='Banir post'; banP.onclick = ()=>{ if(confirm('Banir este post?')){ bans.posts.push(p.id); localStorage.setItem(K_BANS, JSON.stringify(bans)); renderPosts(); }}; const banU = document.createElement('button'); banU.className='btn'; banU.innerText='Banir nick'; banU.onclick = ()=>{ if(confirm('Banir autor '+p.author+'?')){ bans.nicks.push(p.author); localStorage.setItem(K_BANS, JSON.stringify(bans)); renderPosts(); renderChat(); alert('Nick banido: '+p.author); }}; right.appendChild(banP); right.appendChild(banU); }
        el.appendChild(left); el.appendChild(right); container.appendChild(el);
      });
    }

    // Files: preview and download
    function previewFile(fileId){ const f = files[fileId]; if(!f){ alert('Arquivo não encontrado'); return }
      const name = f.name.toLowerCase(); const isText = name.endsWith('.txt')||name.endsWith('.lua')||f.type.startsWith('text');
      if(isText){ try{ const arr = f.data.split(','); const text = decodeURIComponent(escape(atob(arr[1]))); showModal(`<pre style='white-space:pre-wrap;max-height:60vh;overflow:auto'>${escapeHtml(text)}</pre>`); }catch(e){ window.open(f.data,'_blank') } } else { // images etcif(f.type.startsWith('image/')){ showModal(`<div style='text-align:center'><img src='${f.data}' style='max-width:100%;border-radius:8px' /><div style='margin-top:8px'><button class='btn primary' onclick="downloadFile('${fileId}','${f.name}')">Baixar</button></div></div>`); } else { showModal(`<div style='text-align:center'>Arquivo: <b>${escapeHtml(f.name)}</b><div style='margin-top:8px'><button class='btn primary' onclick="downloadFile('${fileId}','${f.name}')">Baixar</button></div></div>`); } }
}
function downloadFile(fileId,name){ const f = files[fileId]; if(!f){ alert('Arquivo não encontrado'); return } try{ const arr = f.data.split(','); const mime = arr[0].match(/:(.*?);/)[1]; const bstr = atob(arr[1]); let n=bstr.length; const u8=new Uint8Array(n); while(n--) u8[n]=bstr.charCodeAt(n); const blob=new Blob([u8],{type:mime}); const url=URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download = name||f.name; document.body.appendChild(a); a.click(); a.remove(); setTimeout(()=>URL.revokeObjectURL(url),60000); }catch(e){ window.open(f.data,'_blank') } }

// Likes
function toggleLike(postId){ const p = posts.find(x=>x.id===postId); if(!p) return; const idx = p.likes.indexOf(user.id); if(idx===-1) p.likes.push(user.id); else p.likes.splice(idx,1); localStorage.setItem(K_POSTS, JSON.stringify(posts)); renderPosts(); }

// Comments modal
function openComments(postId){ const p = posts.find(x=>x.id===postId); if(!p) return; let html = `<h3>Comentários — ${escapeHtml(p.title)}</h3>`; html += `<div style='max-height:50vh;overflow:auto;padding:6px'>`;
  (p.comments||[]).forEach(c=>{ html += `<div style='border-bottom:1px solid rgba(255,255,255,0.03);padding:8px 0'><b>${escapeHtml(c.name)}</b> <span class='small'>• ${escapeHtml(c.time)}</span><div>${escapeHtml(c.text)}</div></div>`; });
  html += `</div><div style='margin-top:8px'><textarea id='newComment' placeholder='Escreva um comentário'></textarea><div style='display:flex;gap:8px;margin-top:8px'><button class='btn primary' onclick='addComment("${postId}")'>Comentar</button><button class='btn' onclick='closeModal()'>Fechar</button></div></div>`;
  showModal(html);
}
function addComment(postId){ const text = document.getElementById('newComment').value.trim(); if(!text) return; const p = posts.find(x=>x.id===postId); if(!p) return; const c = {id:Date.now().toString(36),name:user.name,text, time:new Date().toLocaleString()}; p.comments.push(c); localStorage.setItem(K_POSTS, JSON.stringify(posts)); closeModal(); openComments(postId); }

// Chat
function sendChat(){ const t = document.getElementById('chatInput').value.trim(); if(!t) return; if(bans.nicks.includes(user.name)){ alert('Você foi banido'); return } chat.push({id:Date.now().toString(36),name:user.name,text:t,time:new Date().toLocaleTimeString()}); localStorage.setItem(K_CHAT, JSON.stringify(chat)); document.getElementById('chatInput').value=''; renderChat(); }
function renderChat(){ const box = document.getElementById('chatBox'); box.innerHTML=''; chat.slice(-200).forEach(m=>{ if(bans.nicks.includes(m.name)) return; const el = document.createElement('div'); el.className='msg '+(m.name===user.name?'me':'other'); el.innerHTML = `<b>${escapeHtml(m.name)}</b> <span class='small'>• ${escapeHtml(m.time)}</span><div>${escapeHtml(m.text)}</div>`; box.appendChild(el); }); box.scrollTop = box.scrollHeight; }

// Servers
let curServersTab = 'rp'; function switchServers(tab,btn){ curServersTab = tab; document.getElementById('tab_rp').classList.toggle('btn primary', tab==='rp'); document.getElementById('tab_pvp').classList.toggle('btn primary', tab==='pvp'); renderServers(); }
function renderServers(){ const list = servers[curServersTab]||[]; const container = document.getElementById('serversList'); container.innerHTML=''; if(list.length===0) container.innerHTML = '<div class="muted">Nenhum servidor listado.</div>';
  list.slice().reverse().forEach(s=>{ if(bans.nicks.includes(s.author)) return; const el = document.createElement('div'); el.className='server-card'; el.innerHTML = `<strong>${escapeHtml(s.name)}</strong><div class='small'>${escapeHtml(s.desc)}</div><div style='margin-top:6px'><a href='${escapeHtml(s.link)}' target='_blank'>Abrir link</a> • <span class='muted'>por ${escapeHtml(s.author)}</span></div>`; if(user.admin){ const btn = document.createElement('button'); btn.className='btn'; btn.innerText='Remover'; btn.onclick = ()=>{ if(confirm('Remover servidor?')){ servers[curServersTab]=servers[curServersTab].filter(x=>x.id!==s.id); localStorage.setItem(K_SERVERS, JSON.stringify(servers)); renderServers(); } }; el.appendChild(btn);} container.appendChild(el); });
  if(user.admin){ const f = document.createElement('div'); f.style.marginTop='12px'; f.innerHTML = `<div style='font-weight:800;margin-bottom:6px'>Adicionar servidor (${curServersTab.toUpperCase()})</div><input id='srv_name' placeholder='Nome do servidor' /><input id='srv_link' placeholder='Link (Discord / IP / Site)' /><textarea id='srv_desc' placeholder='Descrição'></textarea><div style='display:flex;gap:8px;margin-top:8px'><button class='btn primary' onclick='addServer()'>Adicionar servidor</button></div>`; container.appendChild(f); }
}
function addServer(){ const name = document.getElementById('srv_name').value.trim(); const link = document.getElementById('srv_link').value.trim(); const desc = document.getElementById('srv_desc').value.trim(); if(!name||!link){ alert('Nome e link são necessários'); return } const s = {id:Date.now().toString(36),name,link,desc,author:user.name,time:new Date().toLocaleString()}; servers[curServersTab].push(s); localStorage.setItem(K_SERVERS, JSON.stringify(servers)); renderServers(); alert('Servidor adicionado'); }

// Admin
function promptAdmin(){ const code = prompt('Código admin:'); if(code===ADMIN_CODE){ user.admin = true; localStorage.setItem(K_USER, JSON.stringify(user)); document.getElementById('adminStatus').innerText = 'Admin (neste navegador)'; renderServers(); renderPosts(); alert('Admin ativado'); } else alert('Código incorreto'); }
function banByNickPrompt(){ if(!user.admin){ alert('Apenas admins'); return } const nick = prompt('Nick a banir:'); if(!nick) return; bans.nicks.push(nick); localStorage.setItem(K_BANS, JSON.stringify(bans)); renderPosts(); renderChat(); alert('Nick banido: '+nick); }
function unbanPrompt(){ if(!user.admin){ alert('Apenas admins'); return } const nick = prompt('Nick para remover ban:'); if(!nick) return; bans.nicks = bans.nicks.filter(n=>n!==nick); localStorage.setItem(K_BANS, JSON.stringify(bans)); renderPosts(); renderChat(); alert('Ban removido: '+nick); }
function viewBans(){ alert('Nicks banidos:

'+(bans.nicks.length?bans.nicks.join(' '):'Nenhum')+'

Posts banidos: '+(bans.posts.length?bans.posts.join(', '):'Nenhum')); }

function clearAll(){ if(!confirm('Apagar dados locais (posts, arquivos, chat, servidores, bans)?')) return; localStorage.removeItem(K_POSTS); localStorage.removeItem(K_FILES); localStorage.removeItem(K_CHAT); localStorage.removeItem(K_SERVERS); localStorage.removeItem(K_BANS); posts=[]; files={}; chat=[]; servers={rp:[],pvp:[]}; bans={nicks:[],posts:[]}; renderAll(); alert('Dados limpos'); }

// Comments modal close/open
function showModal(html){ document.getElementById('modalContent').innerHTML = html; document.getElementById('modal').style.display='flex'; }
function closeModal(){ document.getElementById('modal').style.display='none'; document.getElementById('modalContent').innerHTML=''; }

// Utility
function escapeHtml(s){ if(!s) return ''; return String(s).replace(/[&<>\"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]||c)); }
function changeName(){ const n = prompt('Novo nick', user.name); if(!n) return; user.name = n; localStorage.setItem(K_USER, JSON.stringify(user)); document.getElementById('displayName').innerText = user.name; renderPosts(); renderChat(); }

// initial render
function renderAll(){ renderPosts(); renderChat(); renderServers(); }
renderAll();

// Storage sync across tabs
window.addEventListener('storage', (e)=>{ if(e.key===K_POSTS) posts = JSON.parse(localStorage.getItem(K_POSTS)||'[]'), renderPosts(); if(e.key===K_CHAT) chat = JSON.parse(localStorage.getItem(K_CHAT)||'[]'), renderChat(); if(e.key===K_SERVERS) servers = JSON.parse(localStorage.getItem(K_SERVERS)||'{"rp":[],"pvp":[]}'), renderServers(); if(e.key===K_FILES) files = JSON.parse(localStorage.getItem(K_FILES)||'{}'); if(e.key===K_BANS) bans = JSON.parse(localStorage.getItem(K_BANS)||'{"nicks":[],"posts":[]}'), renderPosts(), renderChat(); if(e.key===K_USER) user = JSON.parse(localStorage.getItem(K_USER)||'{}'), document.getElementById('displayName').innerText = user.name; });

// keyboard shortcuts
document.getElementById('chatInput').addEventListener('keydown',(e)=>{ if(e.key==='Enter'){ e.preventDefault(); sendChat(); } });
document.getElementById('searchInput').addEventListener('keydown',(e)=>{ if(e.key==='Enter'){ e.preventDefault(); renderPosts(); } });

  </script>
</body>
</html>
