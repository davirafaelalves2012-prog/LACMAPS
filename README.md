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
body{margin:0;font-family:Inter,system-ui,Arial;background:linear-gradient(180deg,#001207,#03110a);color:#eaffea;scroll-behavior:smooth}
.appbar{position:sticky;top:0;z-index:60;display:flex;align-items:center;gap:12px;padding:12px;background:linear-gradient(90deg,var(--accent2),var(--accent));}
.logo{width:52px;height:52px;border-radius:10px;background:linear-gradient(180deg,#d9ffd9,#aaffcc);display:flex;align-items:center;justify-content:center;font-weight:900;color:#022}
.brand{display:flex;flex-direction:column}
.brand h1{margin:0;font-size:18px;color:#022}
.brand p{margin:0;font-size:12px;color:#022;opacity:0.9}
.actions{margin-left:auto;display:flex;gap:8px;flex-wrap:wrap}
.btn{padding:10px 14px;border-radius:12px;border:none;background:transparent;color:var(--accent);cursor:pointer;font-weight:700;transition:0.2s}
.btn:hover{opacity:0.8}
.btn.primary{background:linear-gradient(90deg,var(--accent2),var(--accent));color:#022}
.wrap{max-width:1100px;margin:12px auto;padding:12px}
.grid{display:grid;grid-template-columns:1fr 360px;gap:16px}
@media(max-width:920px){.grid{grid-template-columns:1fr}}
.card{background:var(--panel);padding:16px;border-radius:12px;border:1px solid rgba(255,255,255,0.02);box-shadow:0 6px 18px rgba(0,0,0,0.6)}
h2{margin:0 0 8px 0}
input,textarea,select{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:var(--glass);color:#eaffea}
textarea{min-height:90px}
.row{display:flex;gap:8px;flex-wrap:wrap}
.muted{color:var(--muted);font-size:13px}
.posts{display:flex;flex-direction:column;gap:12px}
.post{display:flex;gap:12px;padding:12px;border-radius:12px;background:linear-gradient(180deg,rgba(0,0,0,0.25),rgba(255,255,255,0.01));flex-wrap:wrap}
.p-left{flex:1}
.p-right{width:160px;display:flex;flex-direction:column;gap:6px;align-items:flex-end;flex-wrap:wrap}
.title{font-weight:800;color:var(--accent)}
.meta{font-size:13px;color:var(--muted)}
.tags{display:flex;gap:6px;flex-wrap:wrap;margin-top:6px}
.tag{background:var(--accent2);color:#022;padding:4px 8px;border-radius:8px;font-weight:700;font-size:12px}
.small{font-size:12px;color:var(--muted)}
.chat-box{height:300px;overflow:auto;padding:8px;display:flex;flex-direction:column;gap:8px;background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent);border-radius:8px}
.msg{padding:8px;border-radius:10px;word-wrap:break-word}
.msg.me{align-self:flex-end;background:rgba(0,255,140,0.12)}
.msg.other{align-self:flex-start;background:rgba(255,255,255,0.02)}
.server-card{padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.03);background:rgba(0,0,0,0.2);margin-bottom:8px}
.bottomnav{position:fixed;left:0;right:0;bottom:0;padding:8px;background:linear-gradient(180deg,rgba(0,0,0,0.4),rgba(0,0,0,0.8));display:flex;gap:8px;justify-content:space-around;z-index:70}
.bn{padding:10px 14px;border-radius:12px;background:rgba(255,255,255,0.02);color:var(--muted);font-weight:700;flex:1;text-align:center}
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
    <button class="btn" onclick="changeName()">Nick</button>
    <button class="btn" onclick="promptAdmin()">Admin</button>
  </div>
</div>

<div class="wrap">
<div class="grid">
<main>
  <!-- Post Section -->
  <section class="card" id="postSection">
    <h2>Publicar (Mods / Mapas / Comandos)</h2>
    <div class="row" style="margin-top:8px">
      <select id="category">
        <option value="mod">Mod</option>
        <option value="map">Mapa (.txt)</option>
        <option value="cmd">Comando</option>
      </select>
      <input id="version" placeholder="Versão (opcional)" />
    </div>
    <input id="title" placeholder="Título" style="margin-top:8px" />
    <textarea id="desc" placeholder="Descrição / instruções"></textarea>
    <input id="tagsInput" placeholder="Tags (separadas por vírgula)" />
    <input id="linkInput" placeholder="Link (opcional)" style="margin-top:8px" />
    <input id="fileInput" type="file" accept=".txt,.zip,.json,.lua,.png,.jpg,.jpeg" style="margin-top:8px" />
    <div style="margin-top:8px">
      <button class="btn primary" onclick="publishPost()">Publicar</button>
    </div>
    <div class="muted" style="margin-top:8px">Você pode enviar um arquivo ou um link. Mapas em .txt serão visualizáveis.</div>
  </section>

  <!-- Repositório -->
  <section class="card" id="repoSection" style="margin-top:12px">
    <h2>Repositório</h2>
    <div class="row" style="margin-bottom:8px">
      <input id="searchInput" placeholder="Pesquisar..." />
      <select id="filterCat" style="max-width:140px">
        <option value="all">Todas</option>
        <option value="mod">Mods</option>
        <option value="map">Mapas</option>
        <option value="cmd">Comandos</option>
      </select>
    </div>
    <div class="posts" id="postsList"></div>
    <div class="ad" style="margin-top:12px">Espaço para anúncios</div>
  </section>

  <!-- Servidores -->
  <section class="card" id="serversSection" style="margin-top:12px">
    <h2>Servidores</h2>
    <div style="display:flex;gap:8px;margin-bottom:8px;flex-wrap:wrap">
      <button class="btn" onclick="switchServers('rp',this)" id="tab_rp">SERVIDORES ROLEPLAY</button>
      <button class="btn" onclick="switchServers('pvp',this)" id="tab_pvp">SERVIDORES PVP</button>
    </div>
    <div id="serversList"></div>
  </section>
</main>

<aside>
  <!-- Chat -->
  <div class="card">
    <h2>Fórum / Chat</h2>
    <div class="chat-box" id="chatBox"></div>
    <div style="display:flex;gap:8px;margin-top:8px"><input id="chatInput" placeholder="Mensagem..." /><button class="btn primary" onclick="sendChat()">Enviar</button></div>
  </div>

  <!-- Painel Admin -->
  <div class="card" id="adminPanel" style="margin-top:12px;display:none">
    <h2>Painel Admin</h2>
    <div class="muted">Status: <span id="adminStatus">—</span></div>
    <div style="display:flex;gap:8px;margin-top:8px;flex-wrap:wrap">
      <button class="btn" onclick="banByNickPrompt()">Banir por Nick</button>
      <button class="btn" onclick="unbanPrompt()">Remover ban</button>
      <button class="btn" onclick="viewBans()">Ver bans</button>
      <button class="btn" onclick="clearChat()">Apagar mensagens</button>
      <button class="btn" onclick="clearAll()">Limpar dados locais</button>
    </div>
  </div>

  <!-- Minha conta -->
  <div class="card" style="margin-top:12px">
    <h2>Minha conta</h2>
    <div class="muted">Nick: <span id="displayName"></span></div>
    <div class="muted">ID local: <span id="displayId"></span></div>
    <div style="margin-top:8px"><button class="btn" onclick="changeName()">Trocar nome</button></div>
  </div>
</aside>
</div>

<footer>Protótipo NEXOS LAC — protótipo mobile-first • Admin code definido</footer>
</div>

<div class="bottomnav">
  <div class="bn active" data-section="repo" onclick="navTo('repo')">Repositório</div>
  <div class="bn" data-section="post" onclick="navTo('post')">Postar</div>
  <div class="bn" data-section="servers" onclick="navTo('servers')">Servidores</div>
  <div class="bn" data-section="chat" onclick="navTo('chat')">Chat</div>
</div>

<div id="modal" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,0.6);align-items:center;justify-content:center;z-index:80">
  <div style="background:var(--panel);padding:12px;border-radius:10px;max-width:90%;width:720px;max-height:80vh;overflow:auto" id="modalContent"></div>
</div>

<script>
const K_POSTS='nexos_posts_v3',K_FILES='nexos_files_v3',K_CHAT='nexos_chat_v3',K_SERVERS='nexos_servers_v3',K_USER='nexos_user_v3',K_BANS='nexos_bans_v3';
let posts=JSON.parse(localStorage.getItem(K_POSTS)||'[]');
let files=JSON.parse(localStorage.getItem(K_FILES)||'{}');
let chat=JSON.parse(localStorage.getItem(K_CHAT)||'[]');
let servers=JSON.parse(localStorage.getItem(K_SERVERS)||'{"rp":[],"pvp":[]}');
let bans=JSON.parse(localStorage.getItem(K_BANS)||'{"nicks":[],"posts":[]}');
let user=JSON.parse(localStorage.getItem(K_USER)||'{}');
if(!user.id){ user.id=Date.now().toString(36)+Math.random().toString(36).slice(2,6); user.name=prompt('Escolha um nick:')||'Visitante'; user.admin=false; localStorage.setItem(K_USER,JSON.stringify(user)); }

const ADMIN_CODE='1525';

document.getElementById('displayName').innerText=user.name;
document.getElementById('displayId').innerText=user.id;
document.getElementById('adminStatus').innerText=user.admin?'Admin (neste navegador)':'Não admin';
if(user.admin) document.getElementById('adminPanel').style.display='block';
document.getElementById('searchInput').addEventListener('input',renderPosts);

function navTo(section){
  document.querySelectorAll('.bn').forEach(b=>b.classList.remove('active'));
  document.querySelector('.bn[data-section="'+section+'"]').classList.add('active');
  if(section==='repo') document.getElementById('repoSection').scrollIntoView({behavior:'smooth'});
  if(section==='post') document.getElementById('postSection').scrollIntoView({behavior:'smooth'});
  if(section==='servers') document.getElementById('serversSection').scrollIntoView({behavior:'smooth'});
  if(section==='chat') document.getElementById('chatBox').scrollIntoView({behavior:'smooth'});
}

function publishPost(){
  const title=document.getElementById('title').value.trim();
  const desc=document.getElementById('desc').value.trim();
  const cat=document.getElementById('category').value;
  const version=document.getElementById('version').value.trim();
  const tagStr=document.getElementById('tagsInput').value.trim();
  const tags=tagStr?tagStr.split(',').map(t=>t.trim()).filter(t=>t):[];
  const link=document.getElementById('linkInput').value.trim();
  const f=document.getElementById('fileInput').files[0];

  if(!title||( !f && !link)){ alert('Título e arquivo ou link são necessários'); return; }

  const id=Date.now().toString(36)+Math.random().toString(36).slice(2,6);

  if(f){
    const reader=new FileReader();
    reader.onload=(e)=>{
      files[id]={data:e.target.result,name:f.name,type:f.type,size:f.size};
      localStorage.setItem(K_FILES,JSON.stringify(files));
      addPost(id,null);
    };
    reader.readAsDataURL(f);
  } else addPost(null,link);

  function addPost(fileId,link){
    const p={id,title,desc,cat,version,fileId,fileName:fileId?f.name:null,link,author:user.name,authorId:user.id,time:new Date().toLocaleString(),ts:Date.now(),tags,likes:[],comments:[],downloads:0};
    posts.push(p); localStorage.setItem(K_POSTS,JSON.stringify(posts));
    document.getElementById('title').value='';
    document.getElementById('desc').value='';
    document.getElementById('tagsInput').value='';
    document.getElementById('fileInput').value='';
    document.getElementById('linkInput').value='';
    document.getElementById('version').value='';
    renderPosts(); alert('Post publicado!');
  }
}

// ... Aqui você pode manter as funções de renderPosts(), previewFile(), downloadFile(), toggleLike(), openComments(), addComment(), sendChat(), renderChat(), switchServers(), renderServers(), addServer(), promptAdmin(), banByNickPrompt(), unbanPrompt(), viewBans(), clearAll() ...

// Nova função para limpar mensagens do chat
function clearChat(){
  if(!user.admin){ alert('Apenas admins'); return; }
  if(!confirm('Apagar todas as mensagens do chat?')) return;
  chat=[]; localStorage.setItem(K_CHAT,JSON.stringify(chat)); renderChat();
}

// Inicial render
function renderAll(){ renderPosts(); renderChat(); renderServers(); }
renderAll();

// Storage sync across tabs
window.addEventListener('storage',(e)=>{
  if(e.key===K_POSTS) posts=JSON.parse(localStorage.getItem(K_POSTS)||'[]'), renderPosts();
  if(e.key===K_CHAT) chat=JSON.parse(localStorage.getItem(K_CHAT)||'[]'), renderChat();
  if(e.key===K_SERVERS) servers=JSON.parse(localStorage.getItem(K_SERVERS)||'{"rp":[],"pvp":[]}'), renderServers();
  if(e.key===K_FILES) files=JSON.parse(localStorage.getItem(K_FILES)||'{}');
  if(e.key===K_BANS) bans=JSON.parse(localStorage.getItem(K_BANS)||'{"nicks":[],"posts":[]}'), renderPosts(), renderChat();
  if(e.key===K_USER) user=JSON.parse(localStorage.getItem(K_USER)||'{}'), document.getElementById('displayName').innerText=user.name;
});

// Atalhos teclado
document.getElementById('chatInput').addEventListener('keydown',(e)=>{ if(e.key==='Enter'){ e.preventDefault
