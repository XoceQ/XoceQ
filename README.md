---
// src/pages/panel-3d.astro
---
<!doctype html>
<html lang="es">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Panel 3D Senior · José</title>
    <style>
      :root{
        --bg:#0b0b0f;
        --card:#0f1724;
        --glass: rgba(255,255,255,0.04);
        --accent: linear-gradient(135deg,#7c3aed 0%,#06b6d4 100%);
        --glass-border: rgba(255,255,255,0.06);
        --text:#e6eef8;
      }
      html,body{height:100%;margin:0;font-family:Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; background:var(--bg); color:var(--text);}
      .wrap{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:40px}

      /* 3D stage */
      .stage{perspective:1400px;width:100%;max-width:1100px}
      .panel{
        width:100%;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
        border-radius:18px;padding:28px;border:1px solid var(--glass-border);
        transform-style:preserve-3d;transition:transform 300ms cubic-bezier(.2,.8,.2,1);
        box-shadow: 0 10px 30px rgba(2,6,23,0.6), inset 0 1px 0 rgba(255,255,255,0.02);
        background-clip:padding-box;backdrop-filter: blur(6px);
      }

      .panel::before{
        content:'';position:absolute;inset:0;border-radius:18px;pointer-events:none;z-index:-1;
        background: linear-gradient(180deg, rgba(255,255,255,0.02), transparent 40%);
      }

      .panel-header{display:flex;align-items:center;gap:16px}
      .avatar{width:72px;height:72px;border-radius:12px;background:var(--glass);display:flex;align-items:center;justify-content:center;border:1px solid var(--glass-border);box-shadow:0 6px 18px rgba(2,6,23,0.6);}
      .title{font-size:20px;font-weight:700}
      .subtitle{color:rgba(230,238,248,0.7);font-size:13px}

      .content{display:grid;grid-template-columns:1fr 360px;gap:24px;margin-top:18px}

      /* left: skills */
      .skills{background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);padding:18px;border-radius:12px;border:1px solid var(--glass-border)}
      .skill-row{display:flex;flex-wrap:wrap;gap:12px;align-items:center}
      .skill{display:flex;align-items:center;gap:10px;padding:8px 10px;border-radius:10px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.02);min-width:120px}
      .skill img{width:28px;height:28px}

      /* right: 3D mini dashboard */
      .mini{position:relative;height:100%;display:flex;align-items:center;justify-content:center}
      .card-3d{width:320px;height:220px;border-radius:16px;padding:18px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid var(--glass-border);transform-style:preserve-3d;transition:box-shadow 200ms}
      .glow{position:absolute;inset:-40px;border-radius:24px;filter:blur(60px);opacity:0.35;pointer-events:none;background:var(--accent);mix-blend-mode:screen}

      .tile{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}
      .metric{font-weight:700;font-size:18px}
      .sub{font-size:12px;color:rgba(230,238,248,0.65)}

      /* floating layers (3d effect) */
      .layer{position:absolute;inset:0;border-radius:16px;pointer-events:none}
      .layer.one{transform:translateZ(40px) scale(0.99);background:linear-gradient(90deg, rgba(255,255,255,0.015), transparent)}
      .layer.two{transform:translateZ(20px) scale(0.995);background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent)}

      /* interactivity hints */
      .hint{font-size:12px;color:rgba(230,238,248,0.6);margin-top:12px}

      @media (max-width:900px){.content{grid-template-columns:1fr;}.mini{margin-top:12px}.card-3d{margin:0 auto}}
    </style>
  </head>
  <body>
    <div class="wrap">
      <div class="stage">
        <div id="panel" class="panel">
          <div class="panel-header">
            <div class="avatar">
              <svg width="40" height="40" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="url(#g)"/><path d="M12 13a3 3 0 100-6 3 3 0 000 6zM5 20a7 7 0 0114 0" stroke="#fff" stroke-opacity="0.85" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/></svg>
            </div>
            <div>
              <div class="title">What's good? 👋, I'm José</div>
              <div class="subtitle">Senior Fullstack · Telecom Engineer · DevOps-friendly</div>
            </div>
            <div style="margin-left:auto;text-align:right">
              <div style="font-size:12px;color:rgba(230,238,248,0.6)">Bogotá · Colombia</div>
              <div style="font-size:11px;color:rgba(230,238,248,0.45)">Open to freelance & SaaS projects</div>
            </div>
          </div>

          <div class="content">
            <div class="skills">
              <h4 style="margin:0 0 12px 0">Languages & Tools</h4>
              <div class="skill-row">
                <!-- Reuse user's icons for visual consistency -->
                <div class="skill"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg" alt="js"/><div>JavaScript</div></div>
                <div class="skill"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="py"/><div>Python</div></div>
                <div class="skill"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/php/php-original.svg" alt="php"/><div>PHP</div></div>
                <div class="skill"><img src="https://www.vectorlogo.zone/logos/laravel/laravel-icon.svg" alt="laravel"/><div>Laravel</div></div>
                <div class="skill"><img src="https://cdn.worldvectorlogo.com/logos/nextjs-2.svg" alt="next"/><div>Next.js</div></div>
                <div class="skill"><img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" alt="aws"/><div>AWS</div></div>
                <div class="skill"><img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg" alt="linux"/><div>Linux</div></div>
                <div class="skill"><img src="https://www.vectorlogo.zone/logos/getpostman/getpostman-icon.svg" alt="postman"/><div>Postman</div></div>
              </div>

              <p class="hint">Tip: mueve el cursor sobre la tarjeta para ver el efecto 3D.</p>
            </div>

            <div class="mini">
              <div id="card" class="card-3d">
                <div class="glow"></div>

                <div class="layer one"></div>
                <div class="layer two"></div>

                <div style="position:relative;z-index:2">
                  <div class="tile"><div>
                    <div class="metric">+120</div>
                    <div class="sub">Proyectos</div>
                  </div><div style="font-size:22px;font-weight:700">●</div></div>

                  <div class="tile"><div>
                    <div class="metric">SLA 99.9%</div>
                    <div class="sub">Uptime</div>
                  </div><div style="font-size:14px;color:rgba(230,238,248,0.6)">⭐︎︎︎︎︎</div></div>

                  <div class="tile"><div>
                    <div class="metric">$150k</div>
                    <div class="sub">SaaS / mes (ejemplo)</div>
                  </div><div style="font-size:12px;color:rgba(230,238,248,0.6)">⇧</div></div>
                </div>
              </div>
            </div>
          </div>

        </div>
      </div>
    </div>

    <script>
      // Simple mouse-driven tilt + parallax for the panel and mini card
      (function(){
        const panel = document.getElementById('panel');
        const card = document.getElementById('card');
        const stage = document.querySelector('.stage');

        function handleMove(e){
          const rect = stage.getBoundingClientRect();
          const x = (e.clientX - rect.left) / rect.width - 0.5;
          const y = (e.clientY - rect.top) / rect.height - 0.5;

          const rx = -y * 10; // rotateX
          const ry = x * 12;  // rotateY
          panel.style.transform = `rotateX(${rx}deg) rotateY(${ry}deg)`;

          // card micro-parallax
          card.style.transform = `translateZ(30px) rotateX(${rx*0.7}deg) rotateY(${ry*0.7}deg)`;
        }
        function reset(){panel.style.transform='rotateX(0) rotateY(0)'; card.style.transform='translateZ(0)';}

        stage.addEventListener('mousemove', handleMove);
        stage.addEventListener('mouseleave', reset);

        // touch support
        let touchActive = false;
        stage.addEventListener('touchmove', (ev)=>{
          touchActive=true; handleMove(ev.touches[0]);
        }, {passive:true});
        stage.addEventListener('touchend', ()=>{touchActive=false; reset();});
      })();
    </script>
  </body>
</html>
