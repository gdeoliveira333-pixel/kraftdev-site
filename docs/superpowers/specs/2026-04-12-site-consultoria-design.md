# Design Doc — Site Institucional: Consultoria de Produto + AI

**Data:** 2026-04-12
**Autor:** Gui
**Status:** Aprovado

---

## Objetivo

Landing page one-pager institucional para consultoria premium de produto digital com AI. Público-alvo: founders, diretores e gerentes de PMEs. Objetivo principal: gerar credibilidade e converter visitante em lead via WhatsApp ou email.

---

## Stack

**HTML + CSS + JS puro.** Arquivo único `index.html` com CSS embutido e JS vanilla. Zero dependências, build instantâneo, Lighthouse > 90 no mobile.

Deploy: qualquer static host (Netlify, Vercel, GitHub Pages).

---

## Design System

| Token | Valor |
|---|---|
| Background principal | `#0A0A0B` |
| Texto principal | `#E8E6E3` |
| Texto secundário | `#8A8A8A` |
| Accent verde | `#00E5A0` |
| Accent roxo | `#6C63FF` |
| Accent coral | `#FF6B6B` |

**Tipografia (Google Fonts):**
- Headings display: Instrument Serif
- Body/UI: DM Sans
- Detalhes técnicos: Space Mono

**Efeitos globais:**
- Grid sutil no background: `background-image` com linhas a 0.02 opacity, 40px × 40px
- Glassmorphism nos cards: `backdrop-blur`, borda `rgba(255,255,255,0.07)`
- Separadores entre seções: linha de glow horizontal (gradiente verde)
- Max-width do conteúdo: 1200px centralizado

---

## Estrutura de Seções

### 1. Navbar (fixa)
- Logo à esquerda, links de navegação, botão "Fale Conosco" à direita
- `backdrop-blur` ativado após scroll > 50px via JS
- Menu hamburguer no mobile (toggle class)

### 2. Hero
- **Visual:** Grid background + orbe gradiente verde/roxo no canto direito superior
- Tag pill acima do headline: "Consultoria de Produto + AI"
- Headline serifada grande: "Transformamos problemas de negócio em produtos digitais com inteligência artificial."
- Subtítulo em DM Sans, cor secundária
- Dois botões: primário verde ("Iniciar um Projeto →"), secundário outline ("Conheça nosso processo")

### 3. Barra de Capacidades
- Faixa com 8 tags/pills em wrap
- Hover: borda e texto mudam para accent verde
- Scroll horizontal suave no mobile

### 4. Serviços — As 3 Fases
- Título: "Do problema ao produto em produção"
- 3 cards lado a lado (stack no mobile)
- **Estilo dos cards:** top border 3px colorido (verde / roxo / coral), fundo glassmorphism, número grande decorativo com opacity 0.08, lista de entregáveis

### 5. Como Funciona — Timeline
- Título: "Como funciona na prática"
- **Desktop:** Horizontal com 5 dots conectados por linha, título e descrição curta abaixo de cada dot
- **Mobile:** Vertical com linha lateral
- Reveal por scroll via Intersection Observer

### 6. Para Quem
- 3 cards com tag/badge, título e descrição
- Hover: border muda para accent verde

### 7. Diferenciais
- Grid 2×2 com 4 itens
- Cada item com título e parágrafo
- Layout simples, sem cards pesados

### 8. CTA Final / Contato
- Headline + subtítulo
- Botão primário grande: WhatsApp (`https://wa.me/5515999999999`)
- Botão secundário: email (`mailto:contato@empresa.com`)
- Texto: "Respondemos em até 24h."

### 9. Footer
- Logo, links, ícones sociais (LinkedIn, Instagram, WhatsApp), copyright

---

## Comportamentos & Interações

| Interação | Implementação |
|---|---|
| Scroll animations | `IntersectionObserver` — fade-in + translateY(20px) → 0 |
| Navbar blur | `scroll` event listener — adiciona classe após 50px |
| Cards hover | CSS `transition: transform 0.2s` + `translateY(-4px)` |
| Botões CTA hover | CSS `scale(1.03)` + `box-shadow` glow |
| Smooth scroll | `scroll-behavior: smooth` + `scrollIntoView` nos links da navbar |
| Menu hamburguer | Toggle de classe CSS, fecha ao clicar em link |

---

## Responsividade

| Breakpoint | Comportamento |
|---|---|
| Desktop > 1024px | Cards 3 colunas, navbar horizontal, timeline horizontal |
| Tablet 768–1024px | Cards 2 colunas, ajuste de espaçamentos |
| Mobile < 768px | Tudo coluna única, hamburguer, tags com scroll horizontal, timeline vertical |

---

## Conteúdo Placeholder

- Nome da empresa: `[NOME DA EMPRESA]`
- WhatsApp: `https://wa.me/5515999999999`
- Email: `contato@empresa.com`
- Redes sociais: links `#` placeholder

---

## Fora de Escopo (v1)

Blog, login, CRM, multi-idioma, formulário com backend, cases/portfólio, precificação explícita.

---

## Critérios de Aceite

- [ ] Lighthouse mobile > 90
- [ ] Responsivo em mobile, tablet e desktop
- [ ] Navbar fixa com blur no scroll
- [ ] Scroll animations sem jank
- [ ] CTAs levam aos links corretos
- [ ] Smooth scroll nos links da navbar
- [ ] Hover states em todos os cards e botões
- [ ] Design system de cores e tipografia respeitado
- [ ] Sem erros no console
- [ ] Acessibilidade básica: contraste, alt-texts, navegação por teclado
