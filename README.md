<script>
// ... Your constants and initial localStorage setup here

function showSection(sectionId) {
  // Hide all main sections
  document.getElementById('repoSection').style.display = 'none';
  document.getElementById('postSection').style.display = 'none';
  document.getElementById('serversSection').style.display = 'none';

  // Show selected section
  if (sectionId === 'repo') document.getElementById('repoSection').style.display = 'block';
  if (sectionId === 'post') document.getElementById('postSection').style.display = 'block';
  if (sectionId === 'servers') document.getElementById('serversSection').style.display = 'block';

  // Special case for chat: scroll to chat box
  if (sectionId === 'chat') {
    document.getElementById('chatBox').scrollIntoView({behavior: 'smooth'});
  }
}

// Fix: Only the selected section is visible
function navTo(section) {
  document.querySelectorAll('.bn').forEach(b => b.classList.remove('active'));
  document.querySelector('.bn[data-section="' + section + '"]').classList.add('active');
  showSection(section);
}

// On initial load, show repo section
showSection('repo');

// Fix: update admin panel visibility
function updateAdminPanel() {
  if (user.admin) {
    document.getElementById('adminPanel').style.display = 'block';
    document.getElementById('adminStatus').innerText = 'Admin (neste navegador)';
  } else {
    document.getElementById('adminPanel').style.display = 'none';
    document.getElementById('adminStatus').innerText = 'Não admin';
  }
}

// Call on load
updateAdminPanel();

// Fix: When user enters admin code, update admin panel
function promptAdmin() {
  const code = prompt('Digite o código de admin:');
  if (code === ADMIN_CODE) {
    user.admin = true;
    localStorage.setItem(K_USER, JSON.stringify(user));
    updateAdminPanel();
    alert('Admin ativado!');
  } else {
    alert('Código incorreto!');
  }
}

// When user changes name, also update admin panel
function changeName() {
  const newName = prompt('Escolha um nick:', user.name || '');
  if (newName) {
    user.name = newName;
    localStorage.setItem(K_USER, JSON.stringify(user));
    document.getElementById('displayName').innerText = user.name;
    updateAdminPanel();
  }
}

// ... rest of your code ...
// Replace all code that directly sets admin panel display with updateAdminPanel()

// Storage sync across tabs (update admin panel too)
window.addEventListener('storage', (e) => {
  if (e.key === K_USER) {
    user = JSON.parse(localStorage.getItem(K_USER) || '{}');
    document.getElementById('displayName').innerText = user.name;
    updateAdminPanel();
  }
  // ... rest of your event code ...
});

// On load, always update admin panel and section visibility
document.addEventListener('DOMContentLoaded', () => {
  updateAdminPanel();
  showSection('repo');
});
</script>
