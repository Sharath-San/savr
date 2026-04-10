<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>SAVR | Food Creative Studio</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700&family=DM+Sans:wght@300;400;500&family=Bebas+Neue&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #0a0804;
    --bg2: #110f09;
    --surface: #1a1710;
    --gold: #c8973a;
    --gold-light: #e8b86d;
    --cream: #f5ead8;
    --red: #c43a2a;
    --text: #f0e8d8;
    --muted: #7a7060;
    --border: rgba(200,151,58,0.15);
    --ease: cubic-bezier(0.23, 1, 0.32, 1);
  }
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
  html{scroll-behavior:smooth;}
  body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;font-weight:300;overflow-x:hidden;cursor:none;}

  /* GRAIN */
  body::before{content:'';position:fixed;inset:0;background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");pointer-events:none;z-index:100;opacity:.4;}

  /* ─ CURSOR ─ */
  .cursor{width:10px;height:10px;background:var(--gold);border-radius:50%;position:fixed;top:0;left:0;pointer-events:none;z-index:9999;transform:translate(-50%,-50%);transition:width .3s,height .3s,background .3s;mix-blend-mode:difference;}
  .cursor-ring{width:38px;height:38px;border:1px solid var(--gold);border-radius:50%;position:fixed;top:0;left:0;pointer-events:none;z-index:9998;transform:translate(-50%,-50%);opacity:.6;transition:width .3s,height .3s,border-color .3s;}
  body.hovering .cursor{width:22px;height:22px;}
  body.hovering .cursor-ring{width:64px;height:64px;border-color:var(--gold-light);}

  /* ─ SIZZLE TRAIL ─ */
  .sizzle-dot{position:fixed;pointer-events:none;z-index:4999;font-size:1rem;opacity:0;transform:translate(-50%,-50%) scale(0);animation:sizzlePop .7s var(--ease) forwards;}
  @keyframes sizzlePop{0%{opacity:.9;transform:translate(-50%,-50%) scale(1.3);}60%{opacity:.35;transform:translate(-50%,calc(-50% - 32px)) scale(.85);}100%{opacity:0;transform:translate(-50%,calc(-50% - 60px)) scale(.2);}}

  /* ─ CHASE LAYER ─ */
  .chase-layer{position:fixed;bottom:22px;left:0;pointer-events:none;z-index:5000;display:flex;align-items:center;gap:6px;opacity:0;animation:chaseRun 20s linear infinite;}
  .chase-mouse{font-size:1.5rem;transform:scaleX(-1);}
  .chase-food{font-size:1.2rem;animation:foodBounce .35s ease-in-out infinite alternate;}
  .chase-cat{font-size:2.1rem;transform:scaleX(-1);animation:catBob .4s ease-in-out infinite alternate;}
  @keyframes chaseRun{0%{transform:translateX(-150px);opacity:0;}5%{opacity:1;}47%{opacity:1;}52%{transform:translateX(110vw);opacity:0;}100%{transform:translateX(110vw);opacity:0;}}
  @keyframes foodBounce{from{transform:translateY(0) rotate(-10deg);}to{transform:translateY(-10px) rotate(10deg);}}
  @keyframes catBob{from{transform:scaleX(-1) translateY(0);}to{transform:scaleX(-1) translateY(-9px);}}

  /* ─ HEIST LAYER ─ */
  .heist-layer{position:fixed;pointer-events:none;z-index:4000;display:flex;align-items:center;gap:6px;opacity:0;transform:scale(.5) translateY(10px);transition:opacity .25s var(--ease),transform .3s var(--ease);}
  .heist-layer.active{opacity:1;transform:scale(1) translateY(0);}
  .heist-mouse{font-size:1.5rem;animation:hWiggle .45s infinite;}
  .heist-food{font-size:1.8rem;}
  .heist-cat{font-size:2rem;transform:scaleX(-1);animation:hSwipe .55s infinite alternate;}
  @keyframes hWiggle{0%,100%{transform:rotate(0);}50%{transform:rotate(18deg);}}
  @keyframes hSwipe{from{transform:scaleX(-1) translateX(0);}to{transform:scaleX(-1) translateX(-14px);}}

  /* ─ FOOD PEEK on cards ─ */
  .food-peek{position:absolute;bottom:-36px;right:22px;font-size:2.8rem;opacity:.12;transition:transform .5s var(--ease),opacity .4s;user-select:none;pointer-events:none;}
  .service-card:hover .food-peek,.industry-card:hover .food-peek{transform:translateY(-46px) rotate(-12deg);opacity:1;}

  /* ─ FLOATING CRUMBS ─ */
  .crumb-layer{position:absolute;inset:0;overflow:hidden;pointer-events:none;z-index:1;}
  .crumb{position:absolute;bottom:-60px;opacity:0;animation:floatCrumb linear infinite;}
  @keyframes floatCrumb{0%{transform:translateY(0) rotate(0deg) scale(.8);opacity:0;}10%{opacity:.22;}90%{opacity:.12;}100%{transform:translateY(-110vh) rotate(540deg) scale(1.1);opacity:0;}}

  /* ─ MARQUEE ─ */
  .marquee-wrap{border-top:1px solid var(--border);border-bottom:1px solid var(--border);overflow:hidden;padding:16px 0;background:var(--bg2);}
  .marquee-track{display:flex;width:max-content;animation:marquee 22s linear infinite;}
  .marquee-track:hover{animation-play-state:paused;}
  .marquee-item{font-family:'Bebas Neue',sans-serif;font-size:1.2rem;letter-spacing:.15em;color:var(--muted);padding:0 36px;white-space:nowrap;transition:color .3s;}
  .marquee-item:hover{color:var(--gold);}
  .marquee-item span{color:var(--gold);margin-right:36px;}
  @keyframes marquee{from{transform:translateX(0);}to{transform:translateX(-50%);}}

  /* ─ NAV ─ */
  nav{position:fixed;top:0;left:0;right:0;z-index:50;display:flex;align-items:center;justify-content:space-between;padding:28px 48px;background:linear-gradient(to bottom,rgba(10,8,4,.95),transparent);transition:background .4s,border-bottom .4s;}
  nav.scrolled{background:rgba(10,8,4,.97);border-bottom:1px solid rgba(200,151,58,.1);}
  .nav-logo{font-family:'Bebas Neue',sans-serif;font-size:2rem;letter-spacing:.2em;color:var(--gold);text-decoration:none;}
  .nav-logo span{color:var(--cream);}
  .nav-links{display:flex;gap:36px;list-style:none;}
  .nav-links a{color:var(--muted);text-decoration:none;font-size:.8rem;letter-spacing:.15em;text-transform:uppercase;transition:color .3s;}
  .nav-links a:hover{color:var(--gold);}
  .nav-cta{border:1px solid var(--gold)!important;color:var(--gold)!important;padding:10px 24px;border-radius:2px;transition:background .3s,color .3s!important;}
  .nav-cta:hover{background:var(--gold)!important;color:var(--bg)!important;}

  /* ─ HERO ─ */
  .hero{min-height:100vh;display:flex;flex-direction:column;justify-content:flex-end;padding:0 48px 80px;position:relative;overflow:hidden;}
  .hero-bg{position:absolute;inset:0;background:radial-gradient(ellipse 60% 80% at 70% 40%,rgba(200,151,58,.08),transparent 60%),radial-gradient(ellipse 40% 60% at 20% 80%,rgba(196,58,42,.06),transparent 50%),var(--bg);}
  .hero-vline{position:absolute;top:0;bottom:0;left:48px;width:1px;background:linear-gradient(to bottom,transparent,var(--border),transparent);}
  .hero-float-word{position:absolute;right:5%;top:10%;font-family:'Bebas Neue',sans-serif;font-size:clamp(8rem,18vw,22rem);color:rgba(200,151,58,.04);letter-spacing:-.05em;pointer-events:none;user-select:none;animation:heroWordDrift 12s ease-in-out infinite alternate;}
  @keyframes heroWordDrift{from{transform:translateY(0) skewX(0);}to{transform:translateY(-28px) skewX(-1deg);}}
  .hero-label{font-size:.7rem;letter-spacing:.3em;text-transform:uppercase;color:var(--gold);margin-bottom:24px;position:relative;z-index:2;opacity:0;animation:fadeUp .8s ease forwards .3s;}
  .hero-title{font-family:'Playfair Display',serif;font-size:clamp(4rem,10vw,10rem);font-weight:900;line-height:.9;letter-spacing:-.02em;color:var(--cream);position:relative;z-index:2;}
  .hero-title .word{display:inline-block;opacity:0;transform:translateY(50px);animation:fadeUp .9s var(--ease) forwards;}
  .hero-title .word:nth-child(1){animation-delay:.5s;}
  .hero-title .word:nth-child(2){animation-delay:.7s;color:var(--gold);font-style:italic;}
  .hero-title .word:nth-child(3){animation-delay:.9s;}
  .hero-sub{margin-top:32px;font-size:1.1rem;color:var(--muted);max-width:480px;line-height:1.7;position:relative;z-index:2;opacity:0;animation:fadeUp .9s ease forwards 1.1s;}
  .hero-scroll{position:absolute;right:48px;bottom:80px;display:flex;flex-direction:column;align-items:center;gap:12px;opacity:0;animation:fadeUp 1s ease forwards 1.3s;}
  .hero-scroll span{font-size:.65rem;letter-spacing:.25em;text-transform:uppercase;color:var(--muted);writing-mode:vertical-rl;}
  .scroll-line{width:1px;height:60px;background:linear-gradient(to bottom,var(--gold),transparent);animation:scrollPulse 2s ease infinite;}
  @keyframes scrollPulse{0%,100%{opacity:.4;transform:scaleY(1);}50%{opacity:1;transform:scaleY(.8);}}
  @keyframes fadeUp{from{opacity:0;transform:translateY(28px);}to{opacity:1;transform:translateY(0);}}

  /* ─ SHARED ─ */
  section{position:relative;}
  .section-label{font-size:.65rem;letter-spacing:.35em;text-transform:uppercase;color:var(--gold);margin-bottom:16px;}
  .section-title{font-family:'Playfair Display',serif;font-size:clamp(2.2rem,5vw,4.5rem);font-weight:700;line-height:1.1;color:var(--cream);}
  .section-title em{font-style:italic;color:var(--gold);}

  /* ─ BLUEPRINT ─ */
  .blueprint{padding:120px 48px;display:grid;grid-template-columns:1fr 1fr;gap:80px;align-items:center;}
  .blueprint-visual{position:relative;aspect-ratio:4/3;background:var(--surface);border:1px solid var(--border);overflow:hidden;border-radius:2px;}
  .blueprint-visual::before{content:'';position:absolute;inset:0;background:linear-gradient(rgba(200,151,58,.05) 1px,transparent 1px),linear-gradient(90deg,rgba(200,151,58,.05) 1px,transparent 1px);background-size:40px 40px;}
  .bp-diagram{position:absolute;inset:20px;display:flex;flex-direction:column;justify-content:space-around;padding:20px;}
  .bp-row{display:flex;align-items:center;gap:16px;}
  .bp-node{width:48px;height:48px;border:1px solid var(--gold);display:flex;align-items:center;justify-content:center;font-family:'Bebas Neue',sans-serif;font-size:1.1rem;color:var(--gold);flex-shrink:0;position:relative;}
  .bp-node::before{content:'';position:absolute;inset:4px;background:rgba(200,151,58,.05);}
  .bp-line{flex:1;height:1px;background:linear-gradient(to right,var(--gold),transparent);opacity:.4;}
  .bp-text{font-size:.75rem;letter-spacing:.1em;color:var(--muted);text-transform:uppercase;}
  .blueprint-content p{color:var(--muted);line-height:1.8;font-size:1rem;margin-top:24px;max-width:480px;}

  /* ─ SERVICES ─ */
  .services{padding:80px 48px 120px;}
  .services-header{display:flex;justify-content:space-between;align-items:flex-end;margin-bottom:56px;}
  .services-scroll{display:flex;gap:2px;overflow-x:auto;scrollbar-width:none;}
  .services-scroll::-webkit-scrollbar{display:none;}
  .service-card{flex-shrink:0;width:320px;background:var(--surface);border:1px solid var(--border);overflow:hidden;position:relative;cursor:none;transition:border-color .35s,transform .45s var(--ease),box-shadow .45s;opacity:0;transform:translateY(32px);}
  .service-card.show{opacity:1;transform:translateY(0);}
  .service-card:hover{border-color:var(--gold);transform:translateY(-14px);box-shadow:0 28px 56px rgba(0,0,0,.7);}
  .svc-thumb{width:100%;aspect-ratio:3/2;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden;}
  .svc-thumb .glow{position:absolute;inset:0;}
  .svc-thumb .icon{position:relative;z-index:1;font-size:4.5rem;transition:transform .5s var(--ease);}
  .service-card:hover .svc-thumb .icon{transform:scale(1.12) rotate(-5deg);}
  .service-badge{position:absolute;top:16px;right:16px;font-size:.6rem;letter-spacing:.2em;text-transform:uppercase;color:var(--gold);border:1px solid var(--gold);padding:4px 10px;background:rgba(10,8,4,.7);backdrop-filter:blur(4px);}
  .service-body{padding:24px;}
  .service-num{font-family:'Bebas Neue',sans-serif;font-size:.8rem;color:var(--muted);letter-spacing:.2em;}
  .service-name{font-family:'Playfair Display',serif;font-size:1.4rem;font-weight:700;color:var(--cream);margin:6px 0 10px;line-height:1.2;}
  .service-desc{font-size:.82rem;color:var(--muted);line-height:1.6;}

  /* ─ STATS ─ */
  .stats{background:var(--surface);border-top:1px solid var(--border);border-bottom:1px solid var(--border);padding:60px 48px;display:grid;grid-template-columns:repeat(4,1fr);}
  .stat-item{padding:0 40px;border-right:1px solid var(--border);}
  .stat-item:first-child{padding-left:0;}
  .stat-item:last-child{border-right:none;}
  .stat-num{font-family:'Bebas Neue',sans-serif;font-size:3.5rem;color:var(--gold);line-height:1;letter-spacing:-.02em;}
  .stat-label{font-size:.75rem;color:var(--muted);letter-spacing:.1em;text-transform:uppercase;margin-top:8px;line-height:1.5;}

  /* ─ FRAMEWORK ─ */
  .framework{padding:120px 48px;display:grid;grid-template-columns:1fr 2fr;gap:80px;align-items:start;}
  .step{padding:32px 24px 32px 0;border-bottom:1px solid var(--border);display:grid;grid-template-columns:48px 1fr;gap:24px;align-items:start;position:relative;}
  .step::before{content:'';position:absolute;left:0;top:0;bottom:0;width:0;background:rgba(200,151,58,.04);transition:width .45s var(--ease);}
  .step:hover::before{width:100%;}
  .step-num{font-family:'Bebas Neue',sans-serif;font-size:.9rem;color:var(--gold);letter-spacing:.1em;padding-top:3px;}
  .step-title{font-family:'Playfair Display',serif;font-size:1.15rem;font-weight:700;color:var(--cream);margin-bottom:8px;}
  .step-desc{font-size:.82rem;color:var(--muted);line-height:1.7;}

  /* ─ INDUSTRIES ─ */
  .industries{padding:80px 48px 120px;}
  .industries-header{margin-bottom:56px;}
  .industry-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:2px;}
  .industry-card{position:relative;aspect-ratio:4/3;overflow:hidden;cursor:none;background:var(--surface);display:flex;flex-direction:column;justify-content:flex-end;padding:24px;border:1px solid var(--border);transition:border-color .3s;}
  .industry-card:hover{border-color:var(--gold);}
  .industry-card::before{content:'';position:absolute;inset:0;background:linear-gradient(to top,rgba(10,8,4,.92) 0%,rgba(10,8,4,.2) 60%,transparent 100%);z-index:1;}
  .industry-icon{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);font-size:5rem;opacity:.08;z-index:0;transition:opacity .4s,transform .5s var(--ease);}
  .industry-card:hover .industry-icon{opacity:.2;transform:translate(-50%,-56%) scale(1.18) rotate(8deg);}
  .industry-body{position:relative;z-index:2;}
  .industry-code{font-size:.6rem;letter-spacing:.3em;color:var(--gold);text-transform:uppercase;margin-bottom:6px;}
  .industry-name{font-family:'Playfair Display',serif;font-size:1.3rem;font-weight:700;color:var(--cream);margin-bottom:6px;}
  .industry-desc{font-size:.78rem;color:var(--muted);line-height:1.6;}
  .industry-arrow{position:absolute;top:20px;right:20px;width:32px;height:32px;border:1px solid var(--border);display:flex;align-items:center;justify-content:center;color:var(--gold);font-size:.9rem;z-index:2;opacity:0;transform:translateY(4px);transition:opacity .3s,transform .3s;}
  .industry-card:hover .industry-arrow{opacity:1;transform:translateY(0);}

  /* ─ AI ─ */
  .ai-section{padding:120px 48px;background:var(--bg2);border-top:1px solid var(--border);border-bottom:1px solid var(--border);display:grid;grid-template-columns:1fr 1fr;gap:80px;align-items:center;}
  .ai-tags{display:flex;flex-wrap:wrap;gap:8px;margin-top:32px;}
  .ai-tag{font-size:.7rem;letter-spacing:.15em;text-transform:uppercase;color:var(--muted);border:1px solid var(--border);padding:8px 16px;border-radius:2px;transition:border-color .3s,color .3s,transform .3s var(--ease);cursor:none;}
  .ai-tag:hover{border-color:var(--gold);color:var(--gold);transform:translateY(-3px);}
  .ai-visual{position:relative;aspect-ratio:1;background:var(--surface);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;overflow:hidden;}
  .ai-ring{position:absolute;border:1px solid var(--border);border-radius:50%;animation:spin 20s linear infinite;}
  .ai-ring:nth-child(1){width:80%;height:80%;border-color:rgba(200,151,58,.15);}
  .ai-ring:nth-child(2){width:60%;height:60%;border-color:rgba(200,151,58,.1);animation-duration:14s;animation-direction:reverse;}
  .ai-ring:nth-child(3){width:40%;height:40%;border-color:rgba(200,151,58,.08);animation-duration:9s;}
  .ai-center{position:relative;z-index:2;text-align:center;}
  .ai-center-title{font-family:'Bebas Neue',sans-serif;font-size:3rem;color:var(--gold);letter-spacing:.1em;line-height:1;}
  .ai-center-sub{font-size:.7rem;letter-spacing:.3em;color:var(--muted);text-transform:uppercase;margin-top:8px;}
  .ai-dot{position:absolute;width:8px;height:8px;border-radius:50%;background:var(--gold);}
  @keyframes spin{from{transform:rotate(0);}to{transform:rotate(360deg);}}

  /* ─ CTA ─ */
  .cta-section{padding:140px 48px;text-align:center;position:relative;overflow:hidden;}
  .cta-bg{position:absolute;inset:0;background:radial-gradient(ellipse 50% 70% at 50% 50%,rgba(200,151,58,.06),transparent 70%);}
  .cta-title{font-family:'Playfair Display',serif;font-size:clamp(3rem,7vw,7rem);font-weight:900;line-height:1;color:var(--cream);position:relative;z-index:2;}
  .cta-title em{font-style:italic;color:var(--gold);}
  .cta-sub{margin:28px auto 0;max-width:500px;color:var(--muted);line-height:1.7;position:relative;z-index:2;}
  .cta-btn{display:inline-flex;align-items:center;gap:12px;margin-top:48px;background:var(--gold);color:var(--bg);font-family:'DM Sans',sans-serif;font-size:.8rem;letter-spacing:.2em;text-transform:uppercase;padding:18px 40px;border:none;cursor:none;text-decoration:none;position:relative;z-index:2;transition:background .3s,transform .3s var(--ease);font-weight:500;}
  .cta-btn:hover{background:var(--gold-light);transform:translateY(-3px);}
  .cta-btn-arrow{font-size:1rem;transition:transform .3s;}
  .cta-btn:hover .cta-btn-arrow{transform:translateX(5px);}

  /* ─ FOOTER ─ */
  footer{background:var(--bg2);border-top:1px solid var(--border);padding:60px 48px 40px;display:grid;grid-template-columns:1fr auto 1fr;align-items:start;gap:40px;}
  .footer-brand .nav-logo{display:block;margin-bottom:12px;}
  .footer-tagline{font-size:.8rem;color:var(--muted);max-width:260px;line-height:1.6;}
  .footer-links{display:flex;flex-direction:column;gap:12px;align-items:center;}
  .footer-links a{font-size:.75rem;letter-spacing:.15em;text-transform:uppercase;color:var(--muted);text-decoration:none;transition:color .3s;}
  .footer-links a:hover{color:var(--gold);}
  .footer-right{text-align:right;}
  .footer-copy{font-size:.7rem;color:var(--muted);margin-top:40px;padding-top:24px;border-top:1px solid var(--border);letter-spacing:.1em;grid-column:1 / -1;text-align:center;}
  .social-links{display:flex;gap:16px;justify-content:flex-end;margin-bottom:12px;}
  .social-links a{font-size:.7rem;letter-spacing:.2em;text-transform:uppercase;color:var(--muted);text-decoration:none;border:1px solid var(--border);padding:8px 16px;transition:border-color .3s,color .3s;}
  .social-links a:hover{border-color:var(--gold);color:var(--gold);}

  /* ─ SCROLL REVEAL ─ */
  .reveal{opacity:0;transform:translateY(36px);transition:opacity .7s ease,transform .7s var(--ease);}
  .reveal.visible{opacity:1;transform:translateY(0);}

  @media(max-width:900px){
    nav{padding:20px 24px;}
    .nav-links{display:none;}
    .hero,.blueprint,.services,.framework,.industries,.ai-section,.cta-section{padding-left:24px;padding-right:24px;}
    .blueprint,.ai-section{grid-template-columns:1fr;gap:40px;}
    .stats{grid-template-columns:repeat(2,1fr);padding:40px 24px;}
    .stat-item{padding:20px;border-right:none;border-bottom:1px solid var(--border);}
    .framework{grid-template-columns:1fr;gap:40px;}
    .industry-grid{grid-template-columns:1fr 1fr;}
    footer{grid-template-columns:1fr;padding:40px 24px 24px;}
    .footer-right{text-align:left;}
    .social-links{justify-content:flex-start;}
  }
</style>
</head>
<body>

<!-- CURSOR -->
<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<!-- ANIM 2: CHASE STRIP -->
<div class="chase-layer" id="chaseLayer">
  <div class="chase-mouse">🐭</div>
  <div class="chase-food" id="chaseFood">🧀</div>
  <div class="chase-cat">🐱</div>
</div>

<!-- ANIM 3: HEIST LAYER -->
<div class="heist-layer" id="heistLayer">
  <div class="heist-mouse">🐭</div>
  <div class="heist-food" id="heistFood">🧀</div>
  <div class="heist-cat">🐱</div>
</div>

<!-- NAV -->
<nav id="nav">
  <a class="nav-logo" href="#">SAVR<span>.</span></a>
  <ul class="nav-links">
    <li><a href="#services">Services</a></li>
    <li><a href="#framework">Process</a></li>
    <li><a href="#industries">Industries</a></li>
    <li><a href="#" class="nav-cta">Start a Project</a></li>
  </ul>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-bg"></div>
  <div class="hero-vline"></div>
  <div class="hero-float-word">FOOD</div>
  <!-- ANIM 1: FLOATING CRUMBS -->
  <div class="crumb-layer" id="crumbLayer"></div>

  <div class="hero-label">Food Creative Studio · Est. 2020 · Bengaluru</div>
  <h1 class="hero-title">
    <span class="word">Where</span><br/>
    <span class="word">Flavour</span><br/>
    <span class="word">Meets Vision.</span>
  </h1>
  <p class="hero-sub">One studio. Every creative and digital need your food brand has — from farm to feed.</p>
  <div class="hero-scroll"><span>Scroll</span><div class="scroll-line"></div></div>
</section>

<!-- MARQUEE -->
<div class="marquee-wrap">
  <div class="marquee-track">
    <div class="marquee-item"><span>✦</span> Food Photography</div>
    <div class="marquee-item"><span>✦</span> Recipe Content</div>
    <div class="marquee-item"><span>✦</span> Brand Films</div>
    <div class="marquee-item"><span>✦</span> Restaurant Branding</div>
    <div class="marquee-item"><span>✦</span> Social Campaigns</div>
    <div class="marquee-item"><span>✦</span> Menu Design</div>
    <div class="marquee-item"><span>✦</span> Packaging Visuals</div>
    <div class="marquee-item"><span>✦</span> Digital Marketing</div>
    <div class="marquee-item"><span>✦</span> Food Photography</div>
    <div class="marquee-item"><span>✦</span> Recipe Content</div>
    <div class="marquee-item"><span>✦</span> Brand Films</div>
    <div class="marquee-item"><span>✦</span> Restaurant Branding</div>
    <div class="marquee-item"><span>✦</span> Social Campaigns</div>
    <div class="marquee-item"><span>✦</span> Menu Design</div>
    <div class="marquee-item"><span>✦</span> Packaging Visuals</div>
    <div class="marquee-item"><span>✦</span> Digital Marketing</div>
  </div>
</div>

<!-- BLUEPRINT -->
<section class="blueprint reveal">
  <div class="blueprint-visual">
    <div class="bp-diagram">
      <div class="bp-row"><div class="bp-node">01</div><div class="bp-line"></div><div class="bp-text">Brand Audit</div></div>
      <div class="bp-row"><div class="bp-node">02</div><div class="bp-line"></div><div class="bp-text">Content Architecture</div></div>
      <div class="bp-row"><div class="bp-node">03</div><div class="bp-line"></div><div class="bp-text">Shoot Production</div></div>
      <div class="bp-row"><div class="bp-node">04</div><div class="bp-line"></div><div class="bp-text">Deploy & Amplify</div></div>
      <div class="bp-row"><div class="bp-node">05</div><div class="bp-line"></div><div class="bp-text">Growth Analytics</div></div>
    </div>
  </div>
  <div class="blueprint-content">
    <div class="section-label">Our Blueprint</div>
    <h2 class="section-title">Architecting the future of <em>Food Storytelling.</em></h2>
    <p>We don't just shoot beautiful plates. We build complete content ecosystems — from brand strategy through production, distribution, and performance — that make people stop scrolling and start craving.</p>
    <p style="margin-top:16px;">Every frame, every caption, every campaign is engineered to convert appetite into action.</p>
  </div>
</section>

<!-- SERVICES -->
<section class="services reveal" id="services">
  <div class="services-header">
    <div>
      <div class="section-label">SAVR Originals</div>
      <h2 class="section-title">What We <em>Create.</em></h2>
    </div>
    <a href="#" style="color:var(--gold);font-size:.8rem;letter-spacing:.15em;text-transform:uppercase;text-decoration:none;border-bottom:1px solid var(--gold);padding-bottom:2px;">Explore Portfolio →</a>
  </div>
  <div class="services-scroll">
    <div class="service-card" data-food="🍽️" data-delay="0">
      <div class="svc-thumb" style="background:linear-gradient(135deg,#1a1200,#3a2a10,#1a1200);">
        <div class="glow" style="background:radial-gradient(ellipse at 40% 60%,rgba(200,151,58,.3),transparent 70%);"></div>
        <span class="icon">🍽️</span>
      </div>
      <div class="service-badge">Studio Shoot</div>
      <div class="service-body">
        <div class="service-num">SVC_01</div>
        <div class="service-name">Food Photography</div>
        <div class="service-desc">Editorial-grade stills that make every dish look like a cover story — shot in our dedicated food studio.</div>
      </div>
      <div class="food-peek">🍽️</div>
    </div>
    <div class="service-card" data-food="🎬" data-delay="1">
      <div class="svc-thumb" style="background:linear-gradient(135deg,#120a0a,#3a1010,#120a0a);">
        <div class="glow" style="background:radial-gradient(ellipse at 60% 40%,rgba(196,58,42,.25),transparent 70%);"></div>
        <span class="icon">🎬</span>
      </div>
      <div class="service-badge">Cinematic</div>
      <div class="service-body">
        <div class="service-num">SVC_02</div>
        <div class="service-name">Brand Films & Reels</div>
        <div class="service-desc">Short-form and long-form video content that captures the soul of your food brand — fast, rich, scroll-stopping.</div>
      </div>
      <div class="food-peek">🎬</div>
    </div>
    <div class="service-card" data-food="📲" data-delay="2">
      <div class="svc-thumb" style="background:linear-gradient(135deg,#0a1200,#1a3010,#0a1200);">
        <div class="glow" style="background:radial-gradient(ellipse at 50% 50%,rgba(80,140,50,.2),transparent 70%);"></div>
        <span class="icon">📲</span>
      </div>
      <div class="service-badge">Performance</div>
      <div class="service-body">
        <div class="service-num">SVC_03</div>
        <div class="service-name">Social Media Management</div>
        <div class="service-desc">Strategy, content calendars, and community management — we handle your channels while you run your kitchen.</div>
      </div>
      <div class="food-peek">📲</div>
    </div>
    <div class="service-card" data-food="📦" data-delay="3">
      <div class="svc-thumb" style="background:linear-gradient(135deg,#0a080a,#2a1a30,#0a080a);">
        <div class="glow" style="background:radial-gradient(ellipse at 40% 50%,rgba(150,80,200,.15),transparent 70%);"></div>
        <span class="icon">📦</span>
      </div>
      <div class="service-badge">Design</div>
      <div class="service-body">
        <div class="service-num">SVC_04</div>
        <div class="service-name">Packaging & Menu Design</div>
        <div class="service-desc">From takeaway boxes to tasting menus — tactile, beautiful design that tells your story before the first bite.</div>
      </div>
      <div class="food-peek">📦</div>
    </div>
    <div class="service-card" data-food="🌐" data-delay="4">
      <div class="svc-thumb" style="background:linear-gradient(135deg,#0a0a12,#101040,#0a0a12);">
        <div class="glow" style="background:radial-gradient(ellipse at 60% 40%,rgba(58,80,200,.2),transparent 70%);"></div>
        <span class="icon">🌐</span>
      </div>
      <div class="service-badge">Digital</div>
      <div class="service-body">
        <div class="service-num">SVC_05</div>
        <div class="service-name">Website & Digital Presence</div>
        <div class="service-desc">Immersive brand websites built to convert hungry browsers into loyal, repeat customers.</div>
      </div>
      <div class="food-peek">🌐</div>
    </div>
    <div class="service-card" data-food="📣" data-delay="5">
      <div class="svc-thumb" style="background:linear-gradient(135deg,#120a00,#3a2010,#120a00);">
        <div class="glow" style="background:radial-gradient(ellipse at 50% 60%,rgba(200,120,40,.2),transparent 70%);"></div>
        <span class="icon">📣</span>
      </div>
      <div class="service-badge">Growth</div>
      <div class="service-body">
        <div class="service-num">SVC_06</div>
        <div class="service-name">Influencer & PR Campaigns</div>
        <div class="service-desc">Curated creator partnerships and media outreach that put your brand in front of the right food-loving audiences.</div>
      </div>
      <div class="food-peek">📣</div>
    </div>
  </div>
</section>

<!-- STATS -->
<div class="stats reveal">
  <div class="stat-item"><div class="stat-num" data-target="50" data-suffix="M+">0</div><div class="stat-label">Million Content<br/>Impressions</div></div>
  <div class="stat-item"><div class="stat-num" data-target="400" data-suffix="+">0</div><div class="stat-label">Food Brands<br/>Elevated</div></div>
  <div class="stat-item"><div class="stat-num" data-target="12" data-suffix="K+">0</div><div class="stat-label">Thousand Dishes<br/>Photographed</div></div>
  <div class="stat-item"><div class="stat-num" data-target="100" data-suffix="%">0</div><div class="stat-label">Percent End-to-End<br/>Ownership</div></div>
</div>

<!-- FRAMEWORK -->
<section class="framework reveal" id="framework">
  <div>
    <div class="section-label">Our Process</div>
    <h2 class="section-title">Industry-first <em>Thinking.</em></h2>
    <p style="color:var(--muted);line-height:1.8;font-size:.9rem;margin-top:24px;">We don't apply a generic template. We build bespoke visual systems tailored to the psychology, market, and audience of each food category.</p>
  </div>
  <div>
    <div class="step"><div class="step-num">01</div><div><div class="step-title">Brand & Market Research</div><div class="step-desc">Deep-diving into your brand's story, competitive landscape, audience appetite, and positioning before a single shot is set up.</div></div></div>
    <div class="step"><div class="step-num">02</div><div><div class="step-title">Content Architecture</div><div class="step-desc">Designing a full content system — visual language, platform formats, messaging hierarchy — built for your specific food audience's psychology.</div></div></div>
    <div class="step"><div class="step-num">03</div><div><div class="step-title">Production & Styling</div><div class="step-desc">In-studio and on-location shoots with professional food stylists, cinematographers, and creative directors who live and breathe culinary content.</div></div></div>
    <div class="step"><div class="step-num">04</div><div><div class="step-title">Launch & Distribution</div><div class="step-desc">We handle publishing, scheduling, paid amplification, and platform management across all your digital channels simultaneously.</div></div></div>
    <div class="step"><div class="step-num">05</div><div><div class="step-title">Performance & Iteration</div><div class="step-desc">Monthly reporting on reach, engagement, conversions, and brand sentiment — with data-driven optimisation built into every cycle.</div></div></div>
  </div>
</section>

<!-- INDUSTRIES -->
<section class="industries reveal" id="industries">
  <div class="industries-header">
    <div class="section-label">Built for your segment</div>
    <h2 class="section-title">Every Food <em>Category.</em></h2>
  </div>
  <div class="industry-grid">
    <div class="industry-card" data-food="🍕"><div class="industry-icon">🍕</div><div class="industry-arrow">↗</div><div class="industry-body"><div class="industry-code">SEG_01 · Active</div><div class="industry-name">Restaurants & QSR</div><div class="industry-desc">Foot traffic, delivery orders, and loyalty — through content that makes your food irresistible on every platform.</div></div><div class="food-peek">🍕</div></div>
    <div class="industry-card" data-food="☕"><div class="industry-icon">☕</div><div class="industry-arrow">↗</div><div class="industry-body"><div class="industry-code">SEG_02 · Active</div><div class="industry-name">Cafés & Beverages</div><div class="industry-desc">Aesthetic-led content systems for specialty coffee, bubble tea, and artisan beverage brands with a community to build.</div></div><div class="food-peek">☕</div></div>
    <div class="industry-card" data-food="🛒"><div class="industry-icon">🛒</div><div class="industry-arrow">↗</div><div class="industry-body"><div class="industry-code">SEG_03 · Active</div><div class="industry-name">D2C & FMCG Brands</div><div class="industry-desc">Performance-driven content for packaged food brands selling online — designed to reduce friction and spike conversions.</div></div><div class="food-peek">🛒</div></div>
    <div class="industry-card" data-food="🌱"><div class="industry-icon">🌱</div><div class="industry-arrow">↗</div><div class="industry-body"><div class="industry-code">SEG_04 · Active</div><div class="industry-name">Health & Wellness Foods</div><div class="industry-desc">Trust-building, science-backed storytelling for nutraceuticals, superfoods, and clean-label brands that want credible reach.</div></div><div class="food-peek">🌱</div></div>
    <div class="industry-card" data-food="🎂"><div class="industry-icon">🎂</div><div class="industry-arrow">↗</div><div class="industry-body"><div class="industry-code">SEG_05 · Active</div><div class="industry-name">Bakeries & Desserts</div><div class="industry-desc">Sensory-rich, tactile visuals that communicate craft, indulgence, and artistry — for the brands that bake memories.</div></div><div class="food-peek">🎂</div></div>
    <div class="industry-card" data-food="🍷"><div class="industry-icon">🍷</div><div class="industry-arrow">↗</div><div class="industry-body"><div class="industry-code">SEG_06 · Active</div><div class="industry-name">Fine Dining & Luxury</div><div class="industry-desc">Premium cinematic content for Michelin-level experiences — understated, elegant, and impeccably styled.</div></div><div class="food-peek">🍷</div></div>
  </div>
</section>

<!-- AI SECTION -->
<section class="ai-section reveal">
  <div>
    <div class="section-label">AI-Powered Creation</div>
    <h2 class="section-title">We don't follow trends. <em>We cook them.</em></h2>
    <p style="color:var(--muted);line-height:1.8;margin-top:20px;font-size:.95rem;">As Bengaluru's leading food content studio, we integrate generative AI, text-to-video engines, and synthetic food styling directly into our production pipeline.</p>
    <div class="ai-tags">
      <div class="ai-tag">AI-Generated Concepts</div>
      <div class="ai-tag">Text-to-Video Reels</div>
      <div class="ai-tag">AI Voiceover</div>
      <div class="ai-tag">Automated Scheduling</div>
      <div class="ai-tag">Prompt-to-Production</div>
      <div class="ai-tag">Generative Backgrounds</div>
    </div>
  </div>
  <div class="ai-visual">
    <div class="ai-ring"></div><div class="ai-ring"></div><div class="ai-ring"></div>
    <div class="ai-dot" style="top:10%;left:50%;transform:translate(-50%,0);"></div>
    <div class="ai-dot" style="top:50%;right:10%;transform:translate(0,-50%);background:var(--red);"></div>
    <div class="ai-dot" style="bottom:10%;left:30%;background:var(--cream);width:6px;height:6px;"></div>
    <div class="ai-center"><div class="ai-center-title">SAVR<br/>AI</div><div class="ai-center-sub">Food Intelligence</div></div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section">
  <div class="cta-bg"></div>
  <div class="section-label reveal">Initiate Contact</div>
  <h2 class="cta-title reveal">Let's <em>plate</em><br/>your brand's<br/>next chapter.</h2>
  <p class="cta-sub reveal">Ready to transform how the world sees your food? Let's talk strategy, shoot dates, and the story only your brand can tell.</p>
  <a href="#" class="cta-btn reveal">Enter The Studio <span class="cta-btn-arrow">→</span></a>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-brand">
    <a class="nav-logo" href="#">SAVR<span>.</span></a>
    <div class="footer-tagline">Food Creative Studio & Digital Marketing Agency. Bengaluru, India.</div>
  </div>
  <div class="footer-links">
    <a href="#">Services</a><a href="#">Portfolio</a><a href="#">Industries</a><a href="#">Process</a><a href="#">Contact</a>
  </div>
  <div class="footer-right">
    <div class="social-links"><a href="#">Instagram</a><a href="#">LinkedIn</a><a href="#">Behance</a></div>
    <div style="font-size:.75rem;color:var(--muted);">hello@savrstudio.in</div>
  </div>
  <div class="footer-copy">© 2025 SAVR Creative Studio. All rights reserved.</div>
</footer>

<script>
/* ─── 1. CURSOR + SIZZLE TRAIL ─── */
const cursor = document.getElementById('cursor');
const ring   = document.getElementById('cursorRing');
let mx=0,my=0,rx=0,ry=0,lastTrail=0;
const sparks = ['✨','🔥','⭐','💫','🌟','•'];

document.addEventListener('mousemove', e => {
  mx = e.clientX; my = e.clientY;
  cursor.style.left = mx+'px'; cursor.style.top = my+'px';
  // Sizzle trail
  const now = Date.now();
  if (now - lastTrail > 110) {
    lastTrail = now;
    const d = document.createElement('div');
    d.className = 'sizzle-dot';
    d.textContent = sparks[Math.floor(Math.random()*sparks.length)];
    d.style.left = mx+'px'; d.style.top = my+'px';
    d.style.fontSize = (.5+Math.random()*.6)+'rem';
    document.body.appendChild(d);
    setTimeout(()=>d.remove(), 720);
  }
});

(function animRing(){
  rx += (mx-rx)*.12; ry += (my-ry)*.12;
  ring.style.left = rx+'px'; ring.style.top = ry+'px';
  requestAnimationFrame(animRing);
})();

document.querySelectorAll('a,button,.service-card,.industry-card,.ai-tag,.step').forEach(el=>{
  el.addEventListener('mouseenter',()=>document.body.classList.add('hovering'));
  el.addEventListener('mouseleave',()=>document.body.classList.remove('hovering'));
});


/* ─── 2. FLOATING CRUMBS (hero) ─── */
const foods = ['🍕','🍔','🌮','🍜','🍣','🍩','🧆','🥗','🍱','🧁','🫕','🥘','🍛','🍤'];
const crumbLayer = document.getElementById('crumbLayer');
foods.forEach(f => {
  const el = document.createElement('div');
  el.className = 'crumb';
  el.textContent = f;
  el.style.left = (4+Math.random()*92)+'%';
  el.style.fontSize = (1.1+Math.random()*1.3)+'rem';
  el.style.animationDuration = (11+Math.random()*13)+'s';
  el.style.animationDelay = (Math.random()*-22)+'s';
  crumbLayer.appendChild(el);
});


/* ─── 3. CHASE STRIP — cycling foods ─── */
const chaseFoods = ['🧀','🍗','🥩','🍕','🌮','🍩','🍣','🧁'];
const chaseFoodEl = document.getElementById('chaseFood');
let ci = 0;
setInterval(()=>{ ci=(ci+1)%chaseFoods.length; chaseFoodEl.textContent=chaseFoods[ci]; }, 20000);


/* ─── 4. HEIST LAYER on card hover ─── */
const heistLayer = document.getElementById('heistLayer');
const heistFoodEl = document.getElementById('heistFood');
let heistTimer;

document.querySelectorAll('.service-card,.industry-card').forEach(card=>{
  card.addEventListener('mouseenter', ()=>{
    clearTimeout(heistTimer);
    heistFoodEl.textContent = card.dataset.food || '🍽️';
    const r = card.getBoundingClientRect();
    heistLayer.style.top  = (r.top  + window.scrollY - 56)+'px';
    heistLayer.style.left = (r.right + window.scrollX - 140)+'px';
    heistLayer.classList.add('active');
  });
  card.addEventListener('mouseleave', ()=>{
    heistTimer = setTimeout(()=>heistLayer.classList.remove('active'), 260);
  });
});


/* ─── 5. SCROLL REVEAL ─── */
const revObs = new IntersectionObserver(entries=>{
  entries.forEach(e=>{ if(e.isIntersecting) e.target.classList.add('visible'); });
},{threshold:.1});
document.querySelectorAll('.reveal').forEach(el=>revObs.observe(el));


/* ─── 6. SERVICE CARD STAGGER ─── */
const cardObs = new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(e.isIntersecting){
      const delay = (parseInt(e.target.dataset.delay)||0)*120;
      setTimeout(()=>e.target.classList.add('show'), delay);
    }
  });
},{threshold:.1});
document.querySelectorAll('.service-card').forEach(c=>cardObs.observe(c));


/* ─── 7. COUNT-UP STATS ─── */
const statObs = new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(!e.isIntersecting) return;
    const el = e.target;
    const target = parseInt(el.dataset.target);
    const suffix = el.dataset.suffix || '';
    let start=0;
    const step = ts=>{
      if(!start) start=ts;
      const p = Math.min((ts-start)/1600,1);
      const eased = 1-Math.pow(1-p,3);
      el.textContent = Math.floor(eased*target)+suffix;
      if(p<1) requestAnimationFrame(step);
    };
    requestAnimationFrame(step);
    statObs.unobserve(el);
  });
},{threshold:.5});
document.querySelectorAll('.stat-num[data-target]').forEach(el=>statObs.observe(el));


/* ─── 8. NAV SCROLL ─── */
window.addEventListener('scroll',()=>{
  document.getElementById('nav').classList.toggle('scrolled', window.scrollY>80);
});
</script>
</body>
</html>
