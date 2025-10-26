<script>
  // Simple tab switcher
  const tabs = document.querySelectorAll('.tab');
  const forms = {login: document.getElementById('login'), register: document.getElementById('register')};
  const message = document.getElementById('message');

  tabs.forEach(t=> t.addEventListener('click', ()=>{
    tabs.forEach(x=>x.classList.remove('active'));
    t.classList.add('active');
    const target = t.dataset.target;
    for(const k in forms){
      forms[k].style.display = (k===target)?'grid':'none';
      forms[k].setAttribute('aria-hidden', k===target ? 'false' : 'true');
    }
    message.textContent = '';
  }));

  // Demo credentials fill
  document.getElementById('demoCreds').addEventListener('click', ()=>{
    document.getElementById('loginEmail').value = 'demo@example.com';
    document.getElementById('loginPass').value = 'password123';
    message.textContent = 'Demo credentials filled. (Client-side only)';
  });

  // Login handler (client-side demo)
  document.getElementById('login').addEventListener('submit', (e)=>{
    e.preventDefault();
    const email = document.getElementById('loginEmail').value.trim();
    const pass = document.getElementById('loginPass').value;
    if(!email || pass.length < 6){
      message.innerHTML = '<span class="error">Please enter valid email and password (min 6 characters).</span>';
      return;
    }
    // Demo success
    message.innerHTML = '<span class="success">Signed in as '+escapeHtml(email)+'. (Demo — no backend)</span>';
  });

  // Register handler
  document.getElementById('register').addEventListener('submit', (e)=>{
    e.preventDefault();
    const name = document.getElementById('regName').value.trim();
    const email = document.getElementById('regEmail').value.trim();
    const pass = document.getElementById('regPass').value;
    const pass2 = document.getElementById('regPass2').value;
    const terms = document.getElementById('terms').checked;

    if(!name || !email || pass.length < 6){
      message.innerHTML = '<span class="error">Please complete all required fields and use a 6+ character password.</span>';
      return;
    }
    if(pass !== pass2){
      message.innerHTML = '<span class="error">Passwords do not match.</span>';
      return;
    }
    if(!terms){
      message.innerHTML = '<span class="error">You must agree to the terms to register.</span>';
      return;
    }

    // Simulate success
    message.innerHTML = '<span class="success">Account created for '+escapeHtml(name)+' — (client-side demo).</span>';
    // Optionally switch to login
    setTimeout(()=>{
      document.querySelector('.tab[data-target=\"login\"]').click();
    }, 900);
  });

  document.getElementById('clearReg').addEventListener('click', ()=>{
    document.getElementById('regName').value='';
    document.getElementById('regEmail').value='';
    document.getElementById('regPhone').value='';
    document.getElementById('regPass').value='';
    document.getElementById('regPass2').value='';
    document.getElementById('terms').checked=false;
    message.textContent='Form cleared.';
  });

  document.getElementById('forgotLink').addEventListener('click',(e)=>{
    e.preventDefault();
    message.innerHTML='<span class=\"small\">This is a demo. For password reset implement backend email flow.</span>';
  });

  // small html-escape helper
  function escapeHtml(s){
    return s.replace(/[&<>"']/g,function(c){
      return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',\"'\":\"&#39;\"}[c]
    });
  }
</script>
