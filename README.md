<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>LensCraft Studios — Manila</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet" />
  <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>

  <style>
    :root {
      --gold: #b8965a;
      --gold-light: #d4b07a;
      --dark: #0a0a0a;
      --surface: #111111;
      --surface2: #1a1a1a;
    }

    * { box-sizing: border-box; }

    html { scroll-behavior: smooth; }

    body {
      background: var(--dark);
      color: #e8e4dc;
      font-family: 'DM Sans', sans-serif;
      font-weight: 300;
      cursor: none;
    }

    /* ─── Custom Cursor ─────────────────────────────── */
    #cursor {
      position: fixed; width: 10px; height: 10px;
      background: var(--gold); border-radius: 50%;
      pointer-events: none; z-index: 9999;
      transform: translate(-50%, -50%);
      transition: transform 0.1s ease, width 0.3s ease, height 0.3s ease, background 0.3s;
    }
    #cursor-ring {
      position: fixed; width: 36px; height: 36px;
      border: 1px solid rgba(184,150,90,0.5); border-radius: 50%;
      pointer-events: none; z-index: 9998;
      transform: translate(-50%, -50%);
      transition: transform 0.18s ease, width 0.3s ease, height 0.3s ease, opacity 0.3s;
    }
    body:hover #cursor-ring { opacity: 1; }

    /* ─── Typography ────────────────────────────────── */
    .font-display { font-family: 'Cormorant Garamond', serif; }

    /* ─── Grain Overlay ─────────────────────────────── */
    body::before {
      content: '';
      position: fixed; inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none; z-index: 9990; opacity: 0.35;
    }

    /* ─── Animations ────────────────────────────────── */
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(32px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to   { opacity: 1; }
    }
    @keyframes kenBurns {
      0%   { transform: scale(1)   translate(0, 0); }
      100% { transform: scale(1.1) translate(-2%, -2%); }
    }
    @keyframes kenBurns2 {
      0%   { transform: scale(1.1) translate(-2%, -2%); }
      100% { transform: scale(1)   translate(2%, 1%); }
    }
    .animate-fade-up { animation: fadeUp 0.9s cubic-bezier(0.22, 1, 0.36, 1) forwards; }
    .animate-fade-in { animation: fadeIn 1.2s ease forwards; }

    /* ─── Scroll Reveal ─────────────────────────────── */
    .reveal { opacity: 0; transform: translateY(40px); transition: opacity 0.8s cubic-bezier(0.22,1,0.36,1), transform 0.8s cubic-bezier(0.22,1,0.36,1); }
    .reveal.visible { opacity: 1; transform: translateY(0); }
    .reveal-left { opacity: 0; transform: translateX(-40px); transition: opacity 0.8s ease, transform 0.8s ease; }
    .reveal-left.visible { opacity: 1; transform: translateX(0); }

    /* ─── Hero ──────────────────────────────────────── */
    #hero { min-height: 100svh; position: relative; overflow: hidden; }

    /* ─── Carousel ──────────────────────────────────── */
    .carousel-slide { position: absolute; inset: 0; opacity: 0; transition: opacity 1.2s ease; }
    .carousel-slide.active { opacity: 1; }
    .carousel-img {
      width: 100%; height: 100%; object-fit: cover;
      animation: kenBurns 8s ease-in-out forwards;
    }
    .carousel-slide:nth-child(even) .carousel-img { animation-name: kenBurns2; }

    nav { backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px); }

    .service-card {
      background: var(--surface);
      border: 1px solid rgba(184,150,90,0.1);
      transition: transform 0.4s cubic-bezier(0.22,1,0.36,1), border-color 0.4s ease, background 0.4s ease;
    }
    .service-card:hover {
      transform: translateY(-6px);
      border-color: rgba(184,150,90,0.4);
      background: var(--surface2);
    }

    .gold-line::before {
      content: '';
      display: inline-block;
      width: 3rem; height: 1px;
      background: var(--gold);
      vertical-align: middle;
      margin-right: 1rem;
    }

    .btn-primary {
      position: relative; overflow: hidden;
      border: 1px solid var(--gold);
      color: var(--gold);
      transition: color 0.4s ease;
    }
    .btn-primary::before {
      content: '';
      position: absolute; inset: 0;
      background: var(--gold);
      transform: scaleX(0); transform-origin: left;
      transition: transform 0.4s cubic-bezier(0.22,1,0.36,1);
    }
    .btn-primary:hover::before { transform: scaleX(1); }
    .btn-primary:hover { color: #0a0a0a; }
    .btn-primary span { position: relative; z-index: 1; }

    .form-input {
      background: var(--surface);
      border: 1px solid rgba(255,255,255,0.08);
      color: #e8e4dc;
      transition: border-color 0.3s ease, background 0.3s ease;
      outline: none;
    }
    .form-input:focus { border-color: var(--gold); background: var(--surface2); }

    .dot { background: rgba(255,255,255,0.3); transition: background 0.3s, width 0.3s; }
    .dot.active { background: var(--gold); width: 1.5rem; border-radius: 4px; }

    .section-divider {
      width: 100%; height: 1px;
      background: linear-gradient(90deg, transparent, rgba(184,150,90,0.3), transparent);
    }

    ::-webkit-scrollbar { width: 4px; }
    ::-webkit-scrollbar-track { background: var(--dark); }
    ::-webkit-scrollbar-thumb { background: var(--gold); border-radius: 2px; }

    @media (hover: none) { body { cursor: auto; } #cursor, #cursor-ring { display: none; } }
  </style>
</head>

<body class="antialiased">

  <div id="cursor"></div>
  <div id="cursor-ring"></div>

  <nav id="navbar" class="fixed top-0 left-0 right-0 z-50 transition-all duration-500">
    <div class="max-w-6xl mx-auto px-6 py-5 flex items-center justify-between">
      <a href="#hero" class="font-display text-xl tracking-widest text-white" style="letter-spacing:0.2em">
        LENS<span style="color:var(--gold)">CRAFT</span>
      </a>
      <div class="hidden md:flex items-center gap-8 text-xs tracking-widest uppercase" style="color:rgba(232,228,220,0.6)">
        <a href="#carousel" class="hover:text-[#b8965a] transition-colors">Portfolio</a>
        <a href="#services" class="hover:text-[#b8965a] transition-colors">Services</a>
        <a href="#about" class="hover:text-[#b8965a] transition-colors">About</a>
        <a href="#booking" class="btn-primary px-5 py-2 text-xs tracking-widest"><span>Book Session</span></a>
      </div>
    </div>
  </nav>

  <section id="hero" class="relative flex items-center justify-center">
    <div class="absolute inset-0 bg-black/60 z-10"></div>
    <div class="absolute inset-0 overflow-hidden">
      <img src="https://images.unsplash.com/photo-1606216794074-735e91aa2c92?w=1800&auto=format&fit=crop&q=80"
           alt="" class="w-full h-full object-cover"
           style="animation: kenBurns 16s ease-in-out infinite alternate; filter: brightness(0.4) saturate(0.8);" />
    </div>

    <div class="relative z-20 text-center px-6 max-w-4xl mx-auto">
      <p class="gold-line font-display italic text-sm tracking-widest mb-8 opacity-0 animate-fade-up"
         style="color:var(--gold-light); animation-delay:0.3s; animation-fill-mode:forwards">Manila, Philippines</p>
      <h1 class="font-display text-5xl md:text-7xl lg:text-8xl leading-none mb-6 opacity-0 animate-fade-up"
          style="font-weight:300; animation-delay:0.5s; animation-fill-mode:forwards">
        Crafting Stories<br/><em style="font-style:italic; color:var(--gold-light)">Through Visuals</em>
      </h1>
      <div class="opacity-0 animate-fade-up flex flex-col sm:flex-row gap-4 justify-center items-center"
           style="animation-delay:1s; animation-fill-mode:forwards">
        <a href="#booking" class="btn-primary px-10 py-4 text-xs tracking-widest uppercase"><span>Book a Session</span></a>
      </div>
    </div>
  </section>

  <section id="carousel" class="relative" style="height: 90vh; overflow: hidden;">
    <div id="carousel-wrapper" class="relative w-full h-full">
      <div class="carousel-slide active"><img class="carousel-img" src="https://images.unsplash.com/photo-1519741497674-611481863552?w=1800&auto=format&fit=crop&q=80" alt="Wedding" /></div>
      <div class="carousel-slide"><img class="carousel-img" src="https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?w=1800&auto=format&fit=crop&q=80" alt="Portrait" /></div>
    </div>
    <button id="prev-btn" class="absolute left-6 top-1/2 -translate-y-1/2 z-20 w-12 h-12 border border-white/20 text-white"> < </button>
    <button id="next-btn" class="absolute right-6 top-1/2 -translate-y-1/2 z-20 w-12 h-12 border border-white/20 text-white"> > </button>
    <div id="dots" class="absolute bottom-8 left-1/2 -translate-x-1/2 z-20 flex gap-2"></div>
  </section>

  <section id="services" class="py-24 px-6">
    <div class="max-w-6xl mx-auto">
      <div class="mb-16 reveal">
        <h2 class="font-display text-4xl md:text-5xl">Our Services</h2>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="service-card p-8 reveal">
          <h3 class="font-display text-xl mb-3">Weddings</h3>
          <p class="text-sm opacity-50">Timeless documentation of your most cherished moments.</p>
        </div>
        <div class="service-card p-8 reveal">
          <h3 class="font-display text-xl mb-3">Corporate</h3>
          <p class="text-sm opacity-50">Captured with the professionalism your brand deserves.</p>
        </div>
        <div class="service-card p-8 reveal">
          <h3 class="font-display text-xl mb-3">Drone</h3>
          <p class="text-sm opacity-50">Breathtaking aerial perspectives of Metro Manila.</p>
        </div>
      </div>
    </div>
  </section>

  <script>
    // Cursor Logic
    const cursor = document.getElementById('cursor');
    const cursorRing = document.getElementById('cursor-ring');
    document.addEventListener('mousemove', e => {
      cursor.style.left = e.clientX + 'px'; cursor.style.top = e.clientY + 'px';
      cursorRing.style.left = e.clientX + 'px'; cursorRing.style.top = e.clientY + 'px';
    });

    // Carousel Logic
    const slides = document.querySelectorAll('.carousel-slide');
    let current = 0;
    function goTo(idx) {
      slides[current].classList.remove('active');
      current = (idx + slides.length) % slides.length;
      slides[current].classList.add('active');
    }
    document.getElementById('next-btn').addEventListener('click', () => goTo(current + 1));
    document.getElementById('prev-btn').addEventListener('click', () => goTo(current - 1));
    setInterval(() => goTo(current + 1), 5000);

    // Scroll Reveal
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => { if (entry.isIntersecting) entry.target.classList.add('visible'); });
    }, { threshold: 0.1 });
    document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
  </script>
</body>
</html>
