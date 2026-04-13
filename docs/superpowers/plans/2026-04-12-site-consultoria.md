# Site Consultoria — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Construir um one-pager institucional dark-mode premium para consultoria de produto + AI em HTML/CSS/JS puro.

**Architecture:** Três arquivos separados — `index.html` (estrutura semântica), `styles.css` (design system + layout + responsividade), `main.js` (Intersection Observer para scroll animations, navbar blur, smooth scroll, menu hamburguer). Sem dependências externas além de Google Fonts via CDN.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, flexbox, backdrop-filter), Vanilla JS (ES6+), Google Fonts (Instrument Serif, DM Sans, Space Mono).

---

## File Structure

| Arquivo | Responsabilidade |
|---|---|
| `index.html` | Marcação semântica de todas as 9 seções |
| `styles.css` | Design system (CSS vars), layout, componentes, animações, responsividade |
| `main.js` | Scroll animations (IntersectionObserver), navbar blur, hamburguer, smooth scroll |

---

## Task 1: Setup — Design System e Shell

**Files:**
- Create: `index.html`
- Create: `styles.css`
- Create: `main.js`

- [ ] **Step 1: Criar `index.html` com shell semântico e links para assets**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[NOME DA EMPRESA] — Consultoria de Produto + AI</title>
  <meta name="description" content="Construímos produtos digitais com AI embutida para empresas que precisam de resultado, não de código." />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=DM+Sans:wght@300;400;500;600;700&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <!-- Seções serão adicionadas nas tasks seguintes -->
  <script src="main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Criar `styles.css` com design system completo (CSS custom properties, reset, base)**

```css
/* =====================
   DESIGN SYSTEM
   ===================== */
:root {
  --bg-primary: #0A0A0B;
  --bg-card: rgba(255, 255, 255, 0.03);
  --text-primary: #E8E6E3;
  --text-secondary: #8A8A8A;
  --accent-green: #00E5A0;
  --accent-purple: #6C63FF;
  --accent-coral: #FF6B6B;
  --border-subtle: rgba(255, 255, 255, 0.07);
  --border-card: rgba(255, 255, 255, 0.06);
  --max-width: 1200px;
  --font-display: 'Instrument Serif', Georgia, serif;
  --font-body: 'DM Sans', system-ui, sans-serif;
  --font-mono: 'Space Mono', monospace;
}

/* =====================
   RESET & BASE
   ===================== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 1.6;
  overflow-x: hidden;
}

/* Background grid global */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(255,255,255,0.02) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.02) 1px, transparent 1px);
  background-size: 40px 40px;
  pointer-events: none;
  z-index: 0;
}

/* =====================
   LAYOUT UTILITIES
   ===================== */
.container {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 24px;
  position: relative;
  z-index: 1;
}
section { position: relative; z-index: 1; }

/* =====================
   GLOW DIVIDER
   ===================== */
.glow-divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent-green), var(--accent-purple), transparent);
  opacity: 0.4;
  margin: 0;
}

/* =====================
   SCROLL ANIMATION
   ===================== */
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
.reveal-delay-1 { transition-delay: 0.1s; }
.reveal-delay-2 { transition-delay: 0.2s; }
.reveal-delay-3 { transition-delay: 0.3s; }
```

- [ ] **Step 3: Criar `main.js` com Intersection Observer base**

```js
// =====================
// SCROLL ANIMATIONS
// =====================
const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      revealObserver.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));
```

- [ ] **Step 4: Abrir `index.html` no browser e verificar que a página carrega sem erros no console**

- [ ] **Step 5: Commit**

```bash
git init
git add index.html styles.css main.js
git commit -m "feat: setup — design system, shell HTML e scroll observer base"
```

---

## Task 2: Navbar

**Files:**
- Modify: `index.html` — adicionar `<nav>` dentro de `<body>`
- Modify: `styles.css` — estilos da navbar
- Modify: `main.js` — blur no scroll + hamburguer toggle

- [ ] **Step 1: Adicionar marcação da navbar em `index.html` (dentro de `<body>`, antes do `<script>`)**

```html
<nav class="navbar" id="navbar">
  <div class="container navbar__inner">
    <a href="#" class="navbar__logo">[NOME DA EMPRESA]</a>
    <ul class="navbar__links" id="navLinks">
      <li><a href="#servicos">Serviços</a></li>
      <li><a href="#como-funciona">Como Funciona</a></li>
      <li><a href="#para-quem">Para Quem</a></li>
      <li><a href="#contato">Contato</a></li>
    </ul>
    <a href="#contato" class="btn btn--primary navbar__cta">Fale Conosco</a>
    <button class="navbar__hamburger" id="hamburger" aria-label="Abrir menu" aria-expanded="false">
      <span></span><span></span><span></span>
    </button>
  </div>
</nav>
```

- [ ] **Step 2: Adicionar estilos da navbar em `styles.css`**

```css
/* =====================
   NAVBAR
   ===================== */
.navbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  padding: 16px 0;
  transition: background 0.3s ease, backdrop-filter 0.3s ease, border-color 0.3s ease;
  border-bottom: 1px solid transparent;
}
.navbar.scrolled {
  background: rgba(10, 10, 11, 0.8);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom-color: var(--border-subtle);
}
.navbar__inner {
  display: flex;
  align-items: center;
  gap: 32px;
}
.navbar__logo {
  font-family: var(--font-display);
  font-size: 18px;
  color: var(--text-primary);
  text-decoration: none;
  white-space: nowrap;
}
.navbar__links {
  display: flex;
  list-style: none;
  gap: 28px;
  margin: 0 auto;
}
.navbar__links a {
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  transition: color 0.2s;
}
.navbar__links a:hover { color: var(--text-primary); }
.navbar__cta { flex-shrink: 0; }
.navbar__hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  margin-left: auto;
}
.navbar__hamburger span {
  display: block;
  width: 22px; height: 2px;
  background: var(--text-primary);
  border-radius: 2px;
  transition: transform 0.3s, opacity 0.3s;
}
.navbar__hamburger.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
.navbar__hamburger.open span:nth-child(2) { opacity: 0; }
.navbar__hamburger.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

/* =====================
   BUTTONS (global)
   ===================== */
.btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 10px 20px;
  border-radius: 8px;
  font-family: var(--font-body);
  font-size: 14px;
  font-weight: 600;
  text-decoration: none;
  cursor: pointer;
  border: none;
  transition: transform 0.2s ease, box-shadow 0.2s ease, background 0.2s ease;
}
.btn:hover { transform: scale(1.03); }
.btn--primary {
  background: var(--accent-green);
  color: #0A0A0B;
}
.btn--primary:hover {
  box-shadow: 0 0 20px rgba(0, 229, 160, 0.4);
}
.btn--outline {
  background: transparent;
  color: var(--text-primary);
  border: 1.5px solid rgba(232, 230, 227, 0.2);
}
.btn--outline:hover {
  border-color: rgba(232, 230, 227, 0.5);
}

/* =====================
   MOBILE NAV
   ===================== */
@media (max-width: 768px) {
  .navbar__links {
    position: fixed;
    top: 0; left: 0; right: 0;
    flex-direction: column;
    background: rgba(10, 10, 11, 0.97);
    backdrop-filter: blur(16px);
    padding: 80px 24px 32px;
    gap: 20px;
    transform: translateY(-100%);
    transition: transform 0.3s ease;
    z-index: 99;
  }
  .navbar__links.open { transform: translateY(0); }
  .navbar__links a { font-size: 18px; }
  .navbar__cta { display: none; }
  .navbar__hamburger { display: flex; }
}
```

- [ ] **Step 3: Adicionar lógica de blur e hamburguer em `main.js`**

```js
// =====================
// NAVBAR — SCROLL BLUR
// =====================
const navbar = document.getElementById('navbar');
window.addEventListener('scroll', () => {
  navbar.classList.toggle('scrolled', window.scrollY > 50);
}, { passive: true });

// =====================
// NAVBAR — HAMBURGER
// =====================
const hamburger = document.getElementById('hamburger');
const navLinks = document.getElementById('navLinks');

hamburger.addEventListener('click', () => {
  const isOpen = navLinks.classList.toggle('open');
  hamburger.classList.toggle('open', isOpen);
  hamburger.setAttribute('aria-expanded', String(isOpen));
});

// Fecha ao clicar em link
navLinks.querySelectorAll('a').forEach(link => {
  link.addEventListener('click', () => {
    navLinks.classList.remove('open');
    hamburger.classList.remove('open');
    hamburger.setAttribute('aria-expanded', 'false');
  });
});
```

- [ ] **Step 4: Verificar no browser — navbar fixa, blur ao scroll, hamburguer funcional no mobile (DevTools 375px)**

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css main.js
git commit -m "feat: navbar fixa com blur no scroll e menu hamburguer"
```

---

## Task 3: Seção Hero

**Files:**
- Modify: `index.html` — adicionar `<section id="hero">`
- Modify: `styles.css` — estilos do hero

- [ ] **Step 1: Adicionar marcação do hero em `index.html` (após `</nav>`)**

```html
<section id="hero" class="hero">
  <div class="container hero__inner">
    <div class="hero__content reveal">
      <span class="hero__tag">Consultoria de Produto + AI</span>
      <h1 class="hero__title">Transformamos problemas de negócio em produtos digitais com inteligência artificial.</h1>
      <p class="hero__subtitle">Somos uma consultoria de produto que entende de negócio, projeta a solução e constrói com AI — do zero ao deploy e além.</p>
      <div class="hero__buttons">
        <a href="#contato" class="btn btn--primary">Iniciar um Projeto →</a>
        <a href="#como-funciona" class="btn btn--outline">Conheça nosso processo</a>
      </div>
    </div>
  </div>
  <div class="hero__orb" aria-hidden="true"></div>
</section>
<div class="glow-divider"></div>
```

- [ ] **Step 2: Adicionar estilos do hero em `styles.css`**

```css
/* =====================
   HERO
   ===================== */
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 120px 0 80px;
  position: relative;
  overflow: hidden;
}
.hero__inner {
  position: relative;
  z-index: 2;
}
.hero__content {
  max-width: 720px;
}
.hero__tag {
  display: inline-block;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent-green);
  background: rgba(0, 229, 160, 0.1);
  border: 1px solid rgba(0, 229, 160, 0.2);
  padding: 5px 12px;
  border-radius: 20px;
  margin-bottom: 24px;
}
.hero__title {
  font-family: var(--font-display);
  font-size: clamp(36px, 5vw, 64px);
  line-height: 1.15;
  color: var(--text-primary);
  margin-bottom: 20px;
}
.hero__subtitle {
  font-size: 18px;
  color: var(--text-secondary);
  max-width: 560px;
  margin-bottom: 36px;
  line-height: 1.7;
}
.hero__buttons {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}
.hero__buttons .btn { font-size: 15px; padding: 12px 24px; }

/* Orb decorativo */
.hero__orb {
  position: absolute;
  top: -80px;
  right: -40px;
  width: 600px;
  height: 600px;
  border-radius: 50%;
  background: radial-gradient(
    circle,
    rgba(0, 229, 160, 0.12) 0%,
    rgba(108, 99, 255, 0.07) 45%,
    transparent 70%
  );
  filter: blur(40px);
  pointer-events: none;
  z-index: 1;
}

@media (max-width: 768px) {
  .hero { padding: 100px 0 60px; }
  .hero__subtitle { font-size: 16px; }
  .hero__orb { width: 300px; height: 300px; top: -40px; right: -80px; }
}
```

- [ ] **Step 3: Verificar no browser — hero renderiza com orbe, tag pill, headline serifada, dois botões. Scroll animation ao entrar na viewport.**

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: seção hero com orbe gradiente e scroll animation"
```

---

## Task 4: Barra de Capacidades

**Files:**
- Modify: `index.html` — adicionar `<section class="capabilities">`
- Modify: `styles.css` — estilos das tags

- [ ] **Step 1: Adicionar marcação em `index.html` (após o glow-divider do hero)**

```html
<section class="capabilities">
  <div class="container">
    <div class="capabilities__track reveal">
      <span class="cap-tag">SaaS & Plataformas Web</span>
      <span class="cap-tag">Apps Mobile</span>
      <span class="cap-tag">Chatbots & Agentes AI</span>
      <span class="cap-tag">Copilots Inteligentes</span>
      <span class="cap-tag">Automações com LLM</span>
      <span class="cap-tag">Dashboards & BI</span>
      <span class="cap-tag">Integrações & APIs</span>
      <span class="cap-tag">MVPs em Semanas</span>
    </div>
  </div>
</section>
<div class="glow-divider"></div>
```

- [ ] **Step 2: Adicionar estilos em `styles.css`**

```css
/* =====================
   CAPABILITIES BAR
   ===================== */
.capabilities {
  padding: 40px 0;
}
.capabilities__track {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}
.cap-tag {
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
  border: 1px solid var(--border-subtle);
  padding: 7px 16px;
  border-radius: 20px;
  cursor: default;
  transition: color 0.2s, border-color 0.2s;
  white-space: nowrap;
}
.cap-tag:hover {
  color: var(--accent-green);
  border-color: rgba(0, 229, 160, 0.4);
}

@media (max-width: 768px) {
  .capabilities__track {
    flex-wrap: nowrap;
    overflow-x: auto;
    justify-content: flex-start;
    padding-bottom: 8px;
    scrollbar-width: none;
  }
  .capabilities__track::-webkit-scrollbar { display: none; }
}
```

- [ ] **Step 3: Verificar no browser — tags renderizam, hover funciona, scroll horizontal no mobile.**

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: barra de capacidades com tags pill"
```

---

## Task 5: Seção Serviços — As 3 Fases

**Files:**
- Modify: `index.html` — adicionar `<section id="servicos">`
- Modify: `styles.css` — estilos dos phase cards

- [ ] **Step 1: Adicionar marcação em `index.html`**

```html
<section id="servicos" class="section-services">
  <div class="container">
    <div class="section-header reveal">
      <h2 class="section-title">Do problema ao produto em produção</h2>
      <p class="section-subtitle">Nosso modelo de três fases garante que seu projeto saia do papel, rode no mundo real e evolua continuamente.</p>
    </div>
    <div class="phases-grid">

      <article class="phase-card reveal reveal-delay-1" data-accent="green">
        <div class="phase-card__num" aria-hidden="true">01</div>
        <div class="phase-card__icon">◆</div>
        <h3 class="phase-card__title">Discovery & Build</h3>
        <p class="phase-card__model">Valor fixo</p>
        <p class="phase-card__desc">Mergulhamos no problema de negócio, definimos a solução ideal e construímos o produto com AI embutida. Você recebe um produto funcionando, não só código.</p>
        <ul class="phase-card__list">
          <li>Diagnóstico de negócio</li>
          <li>Arquitetura da solução</li>
          <li>Desenvolvimento completo</li>
          <li>Produto funcional entregue</li>
        </ul>
      </article>

      <article class="phase-card reveal reveal-delay-2" data-accent="purple">
        <div class="phase-card__num" aria-hidden="true">02</div>
        <div class="phase-card__icon">◈</div>
        <h3 class="phase-card__title">Implantação</h3>
        <p class="phase-card__model">Valor fixo</p>
        <p class="phase-card__desc">Deploy, integrações, migração de dados e treinamento. Garantimos que o produto rode no seu ambiente real com sua equipe preparada.</p>
        <ul class="phase-card__list">
          <li>Deploy em produção</li>
          <li>Integração com sistemas</li>
          <li>Migração de dados</li>
          <li>Treinamento da equipe</li>
        </ul>
      </article>

      <article class="phase-card reveal reveal-delay-3" data-accent="coral">
        <div class="phase-card__num" aria-hidden="true">03</div>
        <div class="phase-card__icon">◇</div>
        <h3 class="phase-card__title">Sustentação</h3>
        <p class="phase-card__model">Mensal recorrente</p>
        <p class="phase-card__desc">Monitoramento, evoluções contínuas, suporte técnico e atualização dos modelos de AI. Seu produto nunca fica parado.</p>
        <ul class="phase-card__list">
          <li>Monitoramento contínuo</li>
          <li>Correções e melhorias</li>
          <li>Evolução do produto</li>
          <li>Suporte dedicado</li>
        </ul>
      </article>

    </div>
  </div>
</section>
<div class="glow-divider"></div>
```

- [ ] **Step 2: Adicionar estilos em `styles.css`**

```css
/* =====================
   SECTION SHARED
   ===================== */
.section-services,
.section-how,
.section-for-whom,
.section-differentials,
.section-cta {
  padding: 96px 0;
}
.section-header {
  text-align: center;
  margin-bottom: 56px;
}
.section-title {
  font-family: var(--font-display);
  font-size: clamp(28px, 3.5vw, 44px);
  color: var(--text-primary);
  margin-bottom: 12px;
  line-height: 1.2;
}
.section-subtitle {
  font-size: 16px;
  color: var(--text-secondary);
  max-width: 560px;
  margin: 0 auto;
  line-height: 1.7;
}

/* =====================
   PHASE CARDS
   ===================== */
.phases-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
.phase-card {
  background: var(--bg-card);
  border: 1px solid var(--border-card);
  border-radius: 14px;
  padding: 28px;
  position: relative;
  overflow: hidden;
  transition: transform 0.25s ease, border-top-color 0.25s ease;
}
/* Top border accent via data-accent */
.phase-card[data-accent="green"] { border-top: 3px solid var(--accent-green); }
.phase-card[data-accent="purple"] { border-top: 3px solid var(--accent-purple); }
.phase-card[data-accent="coral"]  { border-top: 3px solid var(--accent-coral); }

.phase-card:hover { transform: translateY(-4px); }

.phase-card__num {
  position: absolute;
  top: 12px; right: 20px;
  font-family: var(--font-body);
  font-size: 72px;
  font-weight: 800;
  line-height: 1;
  opacity: 0.06;
  color: var(--text-primary);
  pointer-events: none;
  user-select: none;
}
.phase-card[data-accent="green"] .phase-card__num { color: var(--accent-green); opacity: 0.15; }
.phase-card[data-accent="purple"] .phase-card__num { color: var(--accent-purple); opacity: 0.15; }
.phase-card[data-accent="coral"]  .phase-card__num { color: var(--accent-coral); opacity: 0.15; }

.phase-card__icon {
  font-size: 20px;
  margin-bottom: 12px;
}
.phase-card[data-accent="green"] .phase-card__icon { color: var(--accent-green); }
.phase-card[data-accent="purple"] .phase-card__icon { color: var(--accent-purple); }
.phase-card[data-accent="coral"]  .phase-card__icon { color: var(--accent-coral); }

.phase-card__title {
  font-size: 18px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 4px;
}
.phase-card__model {
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 14px;
}
.phase-card[data-accent="green"] .phase-card__model { color: var(--accent-green); }
.phase-card[data-accent="purple"] .phase-card__model { color: var(--accent-purple); }
.phase-card[data-accent="coral"]  .phase-card__model { color: var(--accent-coral); }

.phase-card__desc {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
  margin-bottom: 16px;
}
.phase-card__list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 6px;
}
.phase-card__list li {
  font-size: 13px;
  color: var(--text-secondary);
  padding-left: 16px;
  position: relative;
}
.phase-card__list li::before {
  content: '—';
  position: absolute;
  left: 0;
  opacity: 0.4;
}

@media (max-width: 1024px) {
  .phases-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 768px) {
  .phases-grid { grid-template-columns: 1fr; }
  .section-services, .section-how, .section-for-whom, .section-differentials, .section-cta { padding: 64px 0; }
}
```

- [ ] **Step 3: Verificar no browser — 3 cards com top border correto, número decorativo, hover com translateY, responsivo.**

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: seção serviços com os 3 phase cards"
```

---

## Task 6: Seção "Como Funciona" — Timeline

**Files:**
- Modify: `index.html` — adicionar `<section id="como-funciona">`
- Modify: `styles.css` — estilos da timeline horizontal/vertical

- [ ] **Step 1: Adicionar marcação em `index.html`**

```html
<section id="como-funciona" class="section-how">
  <div class="container">
    <div class="section-header reveal">
      <h2 class="section-title">Como funciona na prática</h2>
      <p class="section-subtitle">Um processo desenhado para gerar resultado, não reunião.</p>
    </div>
    <div class="timeline reveal">
      <div class="timeline__step">
        <div class="timeline__dot" aria-hidden="true"></div>
        <div class="timeline__connector" aria-hidden="true"></div>
        <div class="timeline__content">
          <span class="timeline__num">01</span>
          <h3 class="timeline__title">Conversa Inicial</h3>
          <p class="timeline__desc">Você nos conta o problema. A gente escuta, pergunta e entende o contexto real do negócio. Sem compromisso.</p>
        </div>
      </div>
      <div class="timeline__step">
        <div class="timeline__dot" aria-hidden="true"></div>
        <div class="timeline__connector" aria-hidden="true"></div>
        <div class="timeline__content">
          <span class="timeline__num">02</span>
          <h3 class="timeline__title">Proposta</h3>
          <p class="timeline__desc">Montamos uma proposta com escopo, prazo e investimento. Tudo transparente, sem surpresa.</p>
        </div>
      </div>
      <div class="timeline__step">
        <div class="timeline__dot" aria-hidden="true"></div>
        <div class="timeline__connector" aria-hidden="true"></div>
        <div class="timeline__content">
          <span class="timeline__num">03</span>
          <h3 class="timeline__title">Build</h3>
          <p class="timeline__desc">Nosso time projeta e constrói o produto em ciclos curtos com entregas visíveis. Você acompanha tudo.</p>
        </div>
      </div>
      <div class="timeline__step">
        <div class="timeline__dot" aria-hidden="true"></div>
        <div class="timeline__connector" aria-hidden="true"></div>
        <div class="timeline__content">
          <span class="timeline__num">04</span>
          <h3 class="timeline__title">Implantação</h3>
          <p class="timeline__desc">Colocamos o produto no ar no seu ambiente, integramos com seus sistemas e treinamos quem vai usar.</p>
        </div>
      </div>
      <div class="timeline__step">
        <div class="timeline__dot" aria-hidden="true"></div>
        <div class="timeline__content">
          <span class="timeline__num">05</span>
          <h3 class="timeline__title">Sustentação</h3>
          <p class="timeline__desc">O produto evolui com você. Monitoramos, corrigimos e melhoramos mês a mês.</p>
        </div>
      </div>
    </div>
  </div>
</section>
<div class="glow-divider"></div>
```

- [ ] **Step 2: Adicionar estilos em `styles.css`**

```css
/* =====================
   TIMELINE — HORIZONTAL (desktop) / VERTICAL (mobile)
   ===================== */
.timeline {
  display: flex;
  flex-direction: row;
  gap: 0;
  position: relative;
}
.timeline__step {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
}
.timeline__dot {
  width: 14px; height: 14px;
  border-radius: 50%;
  border: 2px solid var(--accent-green);
  background: var(--bg-primary);
  position: relative;
  z-index: 2;
  flex-shrink: 0;
}
.timeline__connector {
  position: absolute;
  top: 6px;
  left: 50%;
  width: 100%;
  height: 1px;
  background: linear-gradient(90deg, rgba(0,229,160,0.5), rgba(0,229,160,0.1));
  z-index: 1;
}
.timeline__step:last-child .timeline__connector { display: none; }

.timeline__content {
  margin-top: 16px;
  text-align: center;
  padding: 0 8px;
}
.timeline__num {
  font-family: var(--font-mono);
  font-size: 11px;
  color: var(--accent-green);
  font-weight: 700;
  letter-spacing: 0.08em;
  display: block;
  margin-bottom: 4px;
}
.timeline__title {
  font-size: 14px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 6px;
}
.timeline__desc {
  font-size: 12px;
  color: var(--text-secondary);
  line-height: 1.5;
}

/* Vertical no mobile */
@media (max-width: 768px) {
  .timeline {
    flex-direction: column;
    padding-left: 20px;
    gap: 0;
  }
  .timeline__step {
    flex-direction: row;
    align-items: flex-start;
    gap: 16px;
    padding-bottom: 28px;
  }
  .timeline__step:last-child { padding-bottom: 0; }
  .timeline__dot {
    margin-top: 3px;
    flex-shrink: 0;
  }
  .timeline__connector {
    top: 14px;
    left: 6px;
    width: 1px;
    height: 100%;
    background: linear-gradient(180deg, rgba(0,229,160,0.5), rgba(0,229,160,0.1));
  }
  .timeline__content {
    margin-top: 0;
    text-align: left;
    padding: 0;
  }
}
```

- [ ] **Step 3: Verificar no browser — timeline horizontal no desktop (5 passos conectados), vertical no mobile.**

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: seção como funciona com timeline horizontal/vertical"
```

---

## Task 7: Seção "Para Quem" + Diferenciais

**Files:**
- Modify: `index.html` — adicionar `<section id="para-quem">` e `<section class="section-differentials">`
- Modify: `styles.css` — estilos dos segment cards e grid de diferenciais

- [ ] **Step 1: Adicionar marcação das duas seções em `index.html`**

```html
<!-- PARA QUEM -->
<section id="para-quem" class="section-for-whom">
  <div class="container">
    <div class="section-header reveal">
      <h2 class="section-title">Para quem é</h2>
      <p class="section-subtitle">Trabalhamos com empresas que levam tecnologia a sério — de startups a indústrias.</p>
    </div>
    <div class="segment-grid">

      <article class="segment-card reveal reveal-delay-1">
        <span class="segment-tag">Digitalização inteligente</span>
        <h3 class="segment-title">PMEs & Indústrias</h3>
        <p class="segment-desc">Empresas tradicionais que precisam digitalizar operações com inteligência, sem depender de times internos de tecnologia.</p>
      </article>

      <article class="segment-card reveal reveal-delay-2">
        <span class="segment-tag">MVP rápido</span>
        <h3 class="segment-title">Startups Early-Stage</h3>
        <p class="segment-desc">Fundadores não-técnicos que precisam de um MVP funcional e robusto em semanas, não meses.</p>
      </article>

      <article class="segment-card reveal reveal-delay-3">
        <span class="segment-tag">AI layer</span>
        <h3 class="segment-title">Scale-ups & Mid-Market</h3>
        <p class="segment-desc">Empresas com produto existente que querem adicionar camadas de AI — copilots, automações, chatbots inteligentes.</p>
      </article>

    </div>
  </div>
</section>
<div class="glow-divider"></div>

<!-- DIFERENCIAIS -->
<section class="section-differentials">
  <div class="container">
    <div class="section-header reveal">
      <h2 class="section-title">Por que a gente, não os outros</h2>
    </div>
    <div class="diff-grid">

      <div class="diff-item reveal reveal-delay-1">
        <h3 class="diff-title">Pensamos como produto, não como código</h3>
        <p class="diff-desc">Não recebemos Figma e codificamos. Entramos no problema e definimos o que construir.</p>
      </div>

      <div class="diff-item reveal reveal-delay-2">
        <h3 class="diff-title">AI como padrão, não como feature</h3>
        <p class="diff-desc">Inteligência artificial está na base de tudo que entregamos, não é um add-on.</p>
      </div>

      <div class="diff-item reveal reveal-delay-1">
        <h3 class="diff-title">Velocidade real</h3>
        <p class="diff-desc">Nosso stack e processo permitem entregar em semanas o que outros demoram meses.</p>
      </div>

      <div class="diff-item reveal reveal-delay-2">
        <h3 class="diff-title">Parceria, não projeto</h3>
        <p class="diff-desc">Nosso modelo de sustentação garante que a gente cresce junto com o seu produto.</p>
      </div>

    </div>
  </div>
</section>
<div class="glow-divider"></div>
```

- [ ] **Step 2: Adicionar estilos em `styles.css`**

```css
/* =====================
   SEGMENT CARDS (Para Quem)
   ===================== */
.segment-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
.segment-card {
  background: var(--bg-card);
  border: 1px solid var(--border-card);
  border-radius: 14px;
  padding: 28px;
  transition: border-color 0.25s ease;
}
.segment-card:hover {
  border-color: rgba(0, 229, 160, 0.35);
}
.segment-tag {
  display: inline-block;
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent-green);
  background: rgba(0, 229, 160, 0.08);
  border: 1px solid rgba(0, 229, 160, 0.2);
  padding: 4px 10px;
  border-radius: 20px;
  margin-bottom: 14px;
}
.segment-title {
  font-size: 18px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 10px;
}
.segment-desc {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
}

@media (max-width: 1024px) {
  .segment-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 768px) {
  .segment-grid { grid-template-columns: 1fr; }
}

/* =====================
   DIFERENCIAIS (2×2 grid)
   ===================== */
.diff-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px 48px;
  max-width: 860px;
  margin: 0 auto;
}
.diff-item {}
.diff-title {
  font-size: 17px;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 8px;
  display: flex;
  align-items: flex-start;
  gap: 10px;
}
.diff-title::before {
  content: '◆';
  color: var(--accent-green);
  font-size: 10px;
  margin-top: 4px;
  flex-shrink: 0;
}
.diff-desc {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
  padding-left: 20px;
}

@media (max-width: 768px) {
  .diff-grid { grid-template-columns: 1fr; gap: 24px; }
}
```

- [ ] **Step 3: Verificar no browser — 3 segment cards com hover verde, grid 2×2 de diferenciais.**

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: seções para quem e diferenciais"
```

---

## Task 8: CTA Final + Footer

**Files:**
- Modify: `index.html` — adicionar `<section id="contato">` e `<footer>`
- Modify: `styles.css` — estilos do CTA e footer

- [ ] **Step 1: Adicionar marcação em `index.html`**

```html
<!-- CTA FINAL -->
<section id="contato" class="section-cta">
  <div class="container">
    <div class="cta-inner reveal">
      <h2 class="cta-title">Vamos conversar sobre o seu projeto?</h2>
      <p class="cta-subtitle">Conta pra gente o problema que você quer resolver. A primeira conversa é sem compromisso.</p>
      <div class="cta-buttons">
        <a href="https://wa.me/5515999999999" class="btn btn--primary btn--large" target="_blank" rel="noopener noreferrer">Fale pelo WhatsApp →</a>
        <a href="mailto:contato@empresa.com" class="btn btn--outline btn--large">Ou mande um email →</a>
      </div>
      <p class="cta-note">Respondemos em até 24h.</p>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer class="footer">
  <div class="glow-divider"></div>
  <div class="container footer__inner">
    <a href="#" class="footer__logo">[NOME DA EMPRESA]</a>
    <nav class="footer__links" aria-label="Links do rodapé">
      <a href="#servicos">Serviços</a>
      <a href="#como-funciona">Como Funciona</a>
      <a href="#para-quem">Para Quem</a>
      <a href="#contato">Contato</a>
    </nav>
    <div class="footer__social">
      <a href="#" aria-label="LinkedIn" target="_blank" rel="noopener noreferrer">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
      </a>
      <a href="#" aria-label="Instagram" target="_blank" rel="noopener noreferrer">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838a6.162 6.162 0 1 0 0 12.324 6.162 6.162 0 0 0 0-12.324zM12 16a4 4 0 1 1 0-8 4 4 0 0 1 0 8zm6.406-11.845a1.44 1.44 0 1 0 0 2.881 1.44 1.44 0 0 0 0-2.881z"/></svg>
      </a>
      <a href="https://wa.me/5515999999999" aria-label="WhatsApp" target="_blank" rel="noopener noreferrer">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 0 1-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 0 1-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 0 1 2.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0 0 12.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 0 0 5.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 0 0-3.48-8.413z"/></svg>
      </a>
    </div>
    <p class="footer__copy">© 2026 [NOME DA EMPRESA]. Todos os direitos reservados.</p>
  </div>
</footer>
```

- [ ] **Step 2: Adicionar estilos em `styles.css`**

```css
/* =====================
   CTA FINAL
   ===================== */
.section-cta { text-align: center; }
.cta-inner {
  max-width: 640px;
  margin: 0 auto;
}
.cta-title {
  font-family: var(--font-display);
  font-size: clamp(28px, 3.5vw, 44px);
  color: var(--text-primary);
  margin-bottom: 16px;
  line-height: 1.2;
}
.cta-subtitle {
  font-size: 16px;
  color: var(--text-secondary);
  margin-bottom: 36px;
  line-height: 1.7;
}
.cta-buttons {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 16px;
}
.btn--large { font-size: 15px; padding: 14px 28px; }
.cta-note {
  font-size: 13px;
  color: var(--text-secondary);
  opacity: 0.7;
}

/* =====================
   FOOTER
   ===================== */
.footer {
  padding-bottom: 40px;
}
.footer__inner {
  padding-top: 40px;
  display: grid;
  grid-template-columns: auto 1fr auto;
  grid-template-rows: auto auto;
  align-items: center;
  gap: 16px 32px;
}
.footer__logo {
  font-family: var(--font-display);
  font-size: 16px;
  color: var(--text-primary);
  text-decoration: none;
  grid-column: 1;
  grid-row: 1;
}
.footer__links {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  grid-column: 2;
  grid-row: 1;
}
.footer__links a {
  font-size: 13px;
  color: var(--text-secondary);
  text-decoration: none;
  transition: color 0.2s;
}
.footer__links a:hover { color: var(--text-primary); }
.footer__social {
  display: flex;
  gap: 12px;
  align-items: center;
  grid-column: 3;
  grid-row: 1;
}
.footer__social a {
  color: var(--text-secondary);
  transition: color 0.2s;
}
.footer__social a:hover { color: var(--accent-green); }
.footer__copy {
  font-size: 12px;
  color: var(--text-secondary);
  opacity: 0.5;
  grid-column: 1 / -1;
  grid-row: 2;
}

@media (max-width: 768px) {
  .footer__inner {
    grid-template-columns: 1fr;
    grid-template-rows: auto;
    justify-items: center;
    text-align: center;
    gap: 20px;
  }
  .footer__logo, .footer__links, .footer__social, .footer__copy {
    grid-column: 1; grid-row: auto;
  }
  .footer__links { justify-content: center; }
}
```

- [ ] **Step 3: Verificar no browser — CTA com dois botões, footer com logo, links, ícones SVG e copyright.**

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: seção CTA final e footer com ícones sociais"
```

---

## Task 9: Scroll Animations + Polish Final

**Files:**
- Modify: `main.js` — inicializar observer em todos os `.reveal` após DOM ready
- Modify: `styles.css` — ajustes finais de espaçamento e acessibilidade

- [ ] **Step 1: Atualizar `main.js` para inicializar observer após DOM carregado**

```js
document.addEventListener('DOMContentLoaded', () => {

  // =====================
  // SCROLL ANIMATIONS
  // =====================
  const revealObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        revealObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });

  document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));

  // =====================
  // NAVBAR — SCROLL BLUR
  // =====================
  const navbar = document.getElementById('navbar');
  window.addEventListener('scroll', () => {
    navbar.classList.toggle('scrolled', window.scrollY > 50);
  }, { passive: true });

  // =====================
  // NAVBAR — HAMBURGER
  // =====================
  const hamburger = document.getElementById('hamburger');
  const navLinks = document.getElementById('navLinks');

  hamburger.addEventListener('click', () => {
    const isOpen = navLinks.classList.toggle('open');
    hamburger.classList.toggle('open', isOpen);
    hamburger.setAttribute('aria-expanded', String(isOpen));
  });

  navLinks.querySelectorAll('a').forEach(link => {
    link.addEventListener('click', () => {
      navLinks.classList.remove('open');
      hamburger.classList.remove('open');
      hamburger.setAttribute('aria-expanded', 'false');
    });
  });

});
```

- [ ] **Step 2: Adicionar estilos de foco para acessibilidade em `styles.css`**

```css
/* =====================
   ACCESSIBILITY
   ===================== */
:focus-visible {
  outline: 2px solid var(--accent-green);
  outline-offset: 3px;
  border-radius: 4px;
}
.sr-only {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}

/* Padding top em body para compensar navbar fixa */
body { padding-top: 0; }
#hero { scroll-margin-top: 0; }
#servicos, #como-funciona, #para-quem, #contato {
  scroll-margin-top: 80px;
}
```

- [ ] **Step 3: Testar scroll animations — abrir no browser, rolar a página, verificar que cada `.reveal` faz fade-in + slide-up ao entrar na viewport. Verificar que não há jank.**

- [ ] **Step 4: Testar responsividade — abrir DevTools, testar 375px (iPhone SE), 768px (tablet), 1280px (desktop). Verificar todas as seções.**

- [ ] **Step 5: Verificar console — zero erros.**

- [ ] **Step 6: Commit final**

```bash
git add index.html styles.css main.js
git commit -m "feat: scroll animations, foco de acessibilidade e polish final"
```

---

## Self-Review — Spec Coverage

| Requisito da spec | Task |
|---|---|
| Navbar fixa com blur | Task 2 |
| Hero com orbe + grid | Task 3 |
| Barra de capacidades | Task 4 |
| 3 phase cards com top border | Task 5 |
| Timeline horizontal (desktop) / vertical (mobile) | Task 6 |
| Para Quem — 3 cards | Task 7 |
| Diferenciais — grid 2×2 | Task 7 |
| CTA Final WhatsApp + email | Task 8 |
| Footer com ícones sociais | Task 8 |
| Scroll animations (IntersectionObserver) | Task 9 |
| Menu hamburguer mobile | Task 2 |
| Smooth scroll | CSS `scroll-behavior: smooth` — Task 1 |
| Hover states cards e botões | Tasks 2, 5, 7 |
| Responsividade 3 breakpoints | Todas as tasks |
| Acessibilidade básica (foco, alt, aria) | Tasks 2, 8, 9 |
| Google Fonts (Instrument Serif, DM Sans, Space Mono) | Task 1 |
