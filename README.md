<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>José — Dynamic Dashboard</title>
  <!-- Bootstrap 5 -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" />
  <!-- Bootstrap Icons -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css" rel="stylesheet" />
  <style>
    :root{
      --bg:#0b0f19;          /* base dark */
      --fg:#e8ecf1;          /* text */
      --muted:#98a2b3;       /* secondary text */
      --brand:#7c3aed;       /* accent */
      --brand-2:#22d3ee;     /* accent 2 */
      --card:#111729aa;      /* glass */
      --ring: 0 0 0 .2rem rgba(124,58,237,.25);
      --shadow: 0 20px 40px rgba(0,0,0,.35);
    }

    html, body{height:100%}
    body{
      background: radial-gradient(1200px 1200px at 10% -10%, #1e1b4b 0%, transparent 50%),
                  radial-gradient(900px 900px at 110% 10%, #0ea5e9 0%, transparent 40%),
                  linear-gradient(180deg, #0b0f19 0%, #0b0f19 100%);
      color:var(--fg);
      overflow-x: hidden;
    }

    /* Floating blob accents */
    .blob{position:fixed; filter: blur(60px); opacity:.35; z-index:-1;}
    .blob.one{width:420px;height:420px; background:#7c3aed; top:-120px; left:-80px; animation: float 14s ease-in-out infinite;}
    .blob.two{width:380px;height:380px; background:#22d3ee; bottom:-140px; right:-80px; animation: float 18s ease-in-out infinite reverse;}
    @keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(24px)}}

    /* Glass cards */
    .glass{
      background: var(--card);
      backdrop-filter: blur(10px);
      border: 1px solid rgba(255,255,255,.08);
      box-shadow: var(--shadow);
    }

    /* Hover lift + tilt */
    .lift{
      transition: transform .35s ease, box-shadow .35s ease;
      will-change: transform;
    }
    .lift:hover{ transform: translateY(-6px) scale(1.01); box-shadow:0 30px 60px rgba(0,0,0,.45)}

    /* Reveal on scroll */
    .reveal{opacity:0; transform: translateY(18px); transition: opacity .7s ease, transform .7s ease}
    .reveal.visible{opacity:1; transform:none}

    /* Section titles */
    .section-title{
      letter-spacing:.3px;
      font-weight:700;
      background: linear-gradient(90deg, var(--brand), var(--brand-2));
      -webkit-background-clip:text; background-clip:text; color:transparent;
    }

    /* Typing effect */
    .typing{ border-right:2px solid var(--brand-2); white-space:nowrap; overflow:hidden }

    /* Badges shimmer */
    .shimmer{ position:relative; overflow:hidden }
    .shimmer::after{
      content:""; position:absolute; inset:0; transform: translateX(-100%);
      background: linear-gradient(110deg, transparent 0%, rgba(255,255,255,.06) 40%, rgba(255,255,255,.16) 60%, transparent 100%);
      animation: shimmer 2.4s ease-in-out infinite;
    }
    @keyframes shimmer{ 0%{transform:translateX(-100%)} 100%{transform:translateX(100%)} }

    /* Icon grid */
    .tool-icon{
      width:42px; height:42px; object-fit:contain; filter: drop-shadow(0 2px 8px rgba(0,0,0,.3));
      transition: transform .25s ease;
    }
    .tool-icon:hover{ transform: translateY(-4px) scale(1.06) }

    /* Quick actions */
    .quick-action{ border:1px dashed rgba(255,255,255,.1)}
    .quick-action:hover{ border-color: rgba(255,255,255,.25)}

    /* Timeline dots */
    .tl-dot{ width:10px; height:10px; background:var(--brand); border-radius:50% }

    /* Respect reduced motion */
    @media (prefers-reduced-motion: reduce){
      *{animation:none!important; transition:none!important}
    }
  </style>
</head>
<body>
  <!-- Background blobs -->
  <div class="blob one"></div>
  <div class="blob two"></div>

  <!-- NAVBAR -->
  <nav class="navbar navbar-expand-lg sticky-top glass rounded-4 mx-3 mt-3">
    <div class="container-fluid">
      <a class="navbar-brand fw-bold" href="#"><i class="bi bi-brilliance me-2 text-info"></i>José<span class="text-info">.dev</span></a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#nav" aria-controls="nav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div id="nav" class="collapse navbar-collapse">
        <ul class="navbar-nav ms-auto align-items-lg-center gap-lg-3">
          <li class="nav-item"><a class="nav-link active" href="#top">Inicio</a></li>
          <li class="nav-item"><a class="nav-link" href="#stats">Estadísticas</a></li>
          <li class="nav-item"><a class="nav-link" href="#tools">Stack</a></li>
          <li class="nav-item"><a class="nav-link" href="#activity">Actividad</a></li>
          <li class="nav-item"><a class="btn btn-sm btn-primary rounded-pill px-3" href="#contact"><i class="bi bi-lightning-charge me-1"></i>Contactar</a></li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- HERO -->
  <header id="top" class="container py-5">
    <div class="row align-items-center g-4">
      <div class="col-lg-7">
        <div class="p-4 p-md-5 glass rounded-4 lift reveal">
          <span class="badge bg-dark border shimmer mb-3"><i class="bi bi-activity me-2"></i>Dashboard • UI dinámico</span>
          <h1 class="display-5 fw-bold mb-3">What’s good? 👋, I'm José</h1>
          <p class="lead text-secondary mb-4">
            <span class="typing" id="typing"></span>
          </p>
          <div class="d-flex flex-wrap gap-2">
            <a class="btn btn-primary rounded-pill" href="#projects"><i class="bi bi-kanban me-2"></i>Ver Proyectos</a>
            <a class="btn btn-outline-light rounded-pill" href="#tools"><i class="bi bi-tools me-2"></i>Lenguajes & Tools</a>
          </div>
        </div>
      </div>
      <div class="col-lg-5">
        <div class="glass rounded-4 p-4 lift reveal">
          <div class="row g-3">
            <div class="col-6">
              <div class="p-3 rounded-4 border h-100 quick-action">
                <div class="fs-4 fw-bold">💼</div>
                <div class="mt-2">Clientes activos</div>
                <div class="h2 fw-bold" data-counter="128">0</div>
              </div>
            </div>
            <div class="col-6">
              <div class="p-3 rounded-4 border h-100 quick-action">
                <div class="fs-4 fw-bold">💸</div>
                <div class="mt-2">Ingresos este mes</div>
                <div class="h2 fw-bold" data-counter="24500000" data-format="money">0</div>
              </div>
            </div>
            <div class="col-12">
              <div class="p-3 rounded-4 border d-flex justify-content-between align-items-center quick-action">
                <div class="d-flex align-items-center gap-2">
                  <i class="bi bi-stars fs-4 text-info"></i>
                  <div>
                    <div class="fw-semibold">Lanzar nueva versión del dashboard</div>
                    <small class="text-secondary">Animaciones + mejoras de accesibilidad</small>
                  </div>
                </div>
                <button class="btn btn-sm btn-primary rounded-pill">Deploy</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </header>

  <!-- STATS CARDS -->
  <section id="stats" class="container pb-4">
    <h2 class="section-title mb-3">Indicadores rápidos</h2>
    <div class="row g-4">
      <div class="col-md-4">
        <div class="glass rounded-4 p-4 lift reveal">
          <div class="d-flex align-items-center justify-content-between">
            <div>
              <div class="text-secondary">Tickets resueltos</div>
              <div class="display-6 fw-bold" data-counter="342">0</div>
            </div>
            <i class="bi bi-check2-circle fs-1 text-success"></i>
          </div>
          <div class="progress mt-3" role="progressbar" aria-label="Progreso" aria-valuenow="78" aria-valuemin="0" aria-valuemax="100">
            <div class="progress-bar bg-success" style="width: 78%">78%</div>
          </div>
        </div>
      </div>
      <div class="col-md-4">
        <div class="glass rounded-4 p-4 lift reveal">
          <div class="d-flex align-items-center justify-content-between">
            <div>
              <div class="text-secondary">Disponibilidad</div>
              <div class="display-6 fw-bold">99.94%</div>
            </div>
            <i class="bi bi-cloud-check fs-1 text-info"></i>
          </div>
          <div class="progress mt-3" role="progressbar" aria-valuenow="99" aria-valuemin="0" aria-valuemax="100">
            <div class="progress-bar" style="width: 99%"></div>
          </div>
        </div>
      </div>
      <div class="col-md-4">
        <div class="glass rounded-4 p-4 lift reveal">
          <div class="d-flex align-items-center justify-content-between">
            <div>
              <div class="text-secondary">Nuevos suscriptores</div>
              <div class="display-6 fw-bold" data-counter="57">0</div>
            </div>
            <i class="bi bi-person-plus fs-1 text-warning"></i>
          </div>
          <div class="progress mt-3" role="progressbar" aria-valuenow="57" aria-valuemin="0" aria-valuemax="100">
            <div class="progress-bar bg-warning" style="width: 57%"></div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- TOOLS GRID -->
  <section id="tools" class="container py-4">
    <h2 class="section-title mb-3">Languages & Tools</h2>
    <div class="glass rounded-4 p-4 lift reveal">
      <div class="row g-3 row-cols-3 row-cols-sm-4 row-cols-md-6 row-cols-lg-8">
        <!-- Repeatable icon cell; keep URLs from your snippet -->
        <div class="col text-center">
          <a href="https://www.arduino.cc/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://cdn.worldvectorlogo.com/logos/arduino-1.svg" alt="arduino"/></a>
          <div class="small text-secondary mt-1">Arduino</div>
        </div>
        <div class="col text-center">
          <a href="https://www.w3schools.com/css/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/css3/css3-original-wordmark.svg" alt="css3"/></a>
          <div class="small text-secondary mt-1">CSS3</div>
        </div>
        <div class="col text-center">
          <a href="https://www.cypress.io" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/simple-icons/simple-icons/6e46ec1fc23b60c8fd0d2f2ff46db82e16dbd75f/icons/cypress.svg" alt="cypress"/></a>
          <div class="small text-secondary mt-1">Cypress</div>
        </div>
        <div class="col text-center">
          <a href="https://www.figma.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://www.vectorlogo.zone/logos/figma/figma-icon.svg" alt="figma"/></a>
          <div class="small text-secondary mt-1">Figma</div>
        </div>
        <div class="col text-center">
          <a href="https://git-scm.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://www.vectorlogo.zone/logos/git-scm/git-scm-icon.svg" alt="git"/></a>
          <div class="small text-secondary mt-1">Git</div>
        </div>
        <div class="col text-center">
          <a href="https://www.w3.org/html/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original-wordmark.svg" alt="html5"/></a>
          <div class="small text-secondary mt-1">HTML5</div>
        </div>
        <div class="col text-center">
          <a href="https://www.java.com" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/java/java-original.svg" alt="java"/></a>
          <div class="small text-secondary mt-1">Java</div>
        </div>
        <div class="col text-center">
          <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg" alt="javascript"/></a>
          <div class="small text-secondary mt-1">JavaScript</div>
        </div>
        <div class="col text-center">
          <a href="https://www.jenkins.io" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://www.vectorlogo.zone/logos/jenkins/jenkins-icon.svg" alt="jenkins"/></a>
          <div class="small text-secondary mt-1">Jenkins</div>
        </div>
        <div class="col text-center">
          <a href="https://laravel.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://www.vectorlogo.zone/logos/laravel/laravel-icon.svg" alt="Laravel"/></a>
          <div class="small text-secondary mt-1">Laravel</div>
        </div>
        <div class="col text-center">
          <a href="https://www.linux.org/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg" alt="linux"/></a>
          <div class="small text-secondary mt-1">Linux</div>
        </div>
        <div class="col text-center">
          <a href="https://mariadb.org/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://www.vectorlogo.zone/logos/mariadb/mariadb-icon.svg" alt="mariadb"/></a>
          <div class="small text-secondary mt-1">MariaDB</div>
        </div>
        <div class="col text-center">
          <a href="https://www.mathworks.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://upload.wikimedia.org/wikipedia/commons/2/21/Matlab_Logo.png" alt="matlab"/></a>
          <div class="small text-secondary mt-1">MATLAB</div>
        </div>
        <div class="col text-center">
          <a href="https://www.mysql.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/mysql/mysql-original-wordmark.svg" alt="mysql"/></a>
          <div class="small text-secondary mt-1">MySQL</div>
        </div>
        <div class="col text-center">
          <a href="https://nextjs.org/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://cdn.worldvectorlogo.com/logos/nextjs-2.svg" alt="nextjs"/></a>
          <div class="small text-secondary mt-1">Next.js</div>
        </div>
        <div class="col text-center">
          <a href="https://nodejs.org" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nodejs/nodejs-original-wordmark.svg" alt="nodejs"/></a>
          <div class="small text-secondary mt-1">Node.js</div>
        </div>
        <div class="col text-center">
          <a href="https://www.oracle.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/oracle/oracle-original.svg" alt="oracle"/></a>
          <div class="small text-secondary mt-1">Oracle</div>
        </div>
        <div class="col text-center">
          <a href="https://www.php.net" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/php/php-original.svg" alt="php"/></a>
          <div class="small text-secondary mt-1">PHP</div>
        </div>
        <div class="col text-center">
          <a href="https://postman.com" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://www.vectorlogo.zone/logos/getpostman/getpostman-icon.svg" alt="postman"/></a>
          <div class="small text-secondary mt-1">Postman</div>
        </div>
        <div class="col text-center">
          <a href="https://www.python.org" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="python"/></a>
          <div class="small text-secondary mt-1">Python</div>
        </div>
        <div class="col text-center">
          <a href="https://www.selenium.dev" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/detain/svg-logos/780f25886640cef088af994181646db2f6b1a3f8/svg/selenium-logo.svg" alt="selenium"/></a>
          <div class="small text-secondary mt-1">Selenium</div>
        </div>
        <div class="col text-center">
          <a href="https://www.typescriptlang.org/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-original.svg" alt="typescript"/></a>
          <div class="small text-secondary mt-1">TypeScript</div>
        </div>
        <div class="col text-center">
          <a href="https://spring.io/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/spring/spring-original.svg" alt="spring"/></a>
          <div class="small text-secondary mt-1">Spring</div>
        </div>
        <div class="col text-center">
          <a href="https://aws.amazon.com/" target="_blank" rel="noreferrer"><img class="tool-icon" src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" alt="aws"/></a>
          <div class="small text-secondary mt-1">AWS</div>
        </div>
      </div>
    </div>
  </section>

  <!-- ACTIVITY / TIMELINE -->
  <section id="activity" class="container py-4">
    <h2 class="section-title mb-3">Actividad reciente</h2>
    <div class="row g-4">
      <div class="col-lg-8">
        <div class="glass rounded-4 p-4 lift reveal">
          <div class="d-flex flex-column gap-4">
            <div class="d-flex gap-3 align-items-start">
              <div class="tl-dot mt-2"></div>
              <div>
                <div class="fw-semibold">Deploy de <span class="text-info">Graciela’s Hotel Manager</span> v1.4.2</div>
                <small class="text-secondary">Hace 2 horas · CI/CD GitHub Actions</small>
              </div>
            </div>
            <div class="d-flex gap-3 align-items-start">
              <div class="tl-dot mt-2"></div>
              <div>
                <div class="fw-semibold">Integración de Power BI con SQLite para AutoFactura</div>
                <small class="text-secondary">Ayer · Conectores y actualización incremental</small>
              </div>
            </div>
            <div class="d-flex gap-3 align-items-start">
              <div class="tl-dot mt-2"></div>
              <div>
                <div class="fw-semibold">Mejoras de rendimiento en API Next.js</div>
                <small class="text-secondary">Hace 3 días · Cache + ISR</small>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-4">
        <div class="glass rounded-4 p-4 lift reveal" id="contact">
          <h5 class="fw-bold mb-2">¿Trabajamos juntos?</h5>
          <p class="text-secondary mb-3">Hablemos de tu idea — creo dashboards con animaciones sutiles y accesibles.</p>
          <div class="d-grid gap-2">
            <a class="btn btn-primary rounded-pill" href="mailto:jose@example.com"><i class="bi bi-envelope-open me-2"></i>Escríbeme</a>
            <a class="btn btn-outline-light rounded-pill" href="#"><i class="bi bi-linkedin me-2"></i>LinkedIn</a>
          </div>
        </div>
      </div>
    </div>
  </section>

  <footer class="container pb-5 pt-2 text-center text-secondary">
    <small>Hecho con Bootstrap 5 · Animaciones CSS + JS minimal</small>
  </footer>

  <!-- Bootstrap JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
  <script>
    // Typing effect
    const phrases = [
      "Telecom engineer • Full‑stack dev • Cybersecurity.",
      "Laravel · Next.js · AWS · Power BI.",
      "Creo dashboards oscuros con micro‑interacciones ✨"
    ];
    let pi = 0, ci = 0, deleting = false;
    const el = document.getElementById('typing');
    function type(){
      const current = phrases[pi];
      el.textContent = current.slice(0, ci);
      if(!deleting){
        if(ci < current.length){ ci++; }
        else { deleting = true; setTimeout(type, 1200); return; }
      } else {
        if(ci > 0){ ci--; }
        else { deleting = false; pi = (pi+1)%phrases.length; }
      }
      setTimeout(type, deleting ? 32 : 48);
    }
    type();

    // IntersectionObserver reveal
    const io = new IntersectionObserver(entries => {
      entries.forEach(e => { if(e.isIntersecting){ e.target.classList.add('visible'); io.unobserve(e.target); } })
    }, { threshold:.2 });
    document.querySelectorAll('.reveal').forEach(el => io.observe(el));

    // Counter animation (supports money formatting)
    function animateCounter(el){
      const target = +el.dataset.counter || 0;
      const isMoney = el.dataset.format === 'money';
      const duration = 1200; const start = performance.now();
      function step(t){
        const p = Math.min((t - start)/duration, 1);
        const val = Math.floor(target * (0.2 + 0.8 * p));
        el.textContent = isMoney ? val.toLocaleString('es-CO',{style:'currency', currency:'COP', maximumFractionDigits:0}) : val.toLocaleString('es-CO');
        if(p < 1) requestAnimationFrame(step);
      }
      requestAnimationFrame(step);
    }
    document.querySelectorAll('[data-counter]').forEach(animateCounter);

    // Subtle tilt on mouse move for .lift cards
    document.querySelectorAll('.lift').forEach(card =>{
      card.addEventListener('mousemove', (e)=>{
        const r = card.getBoundingClientRect();
        const x = e.clientX - r.left, y = e.clientY - r.top;
        const rx = ((y / r.height) - .5) * -6; // rotateX
        const ry = ((x / r.width) - .5) * 6;  // rotateY
        card.style.transform = `perspective(900px) rotateX(${rx}deg) rotateY(${ry}deg)`;
      });
      card.addEventListener('mouseleave', ()=>{
        card.style.transform = '';
      });
    });
  </script>
</body>
</html>


