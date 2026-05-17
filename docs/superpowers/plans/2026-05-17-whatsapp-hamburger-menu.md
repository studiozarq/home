# WhatsApp Pre-filled Message & Hamburger Menu Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add pre-filled WhatsApp messages to both CTA buttons and add a mobile hamburger menu with full-screen overlay navigation.

**Architecture:** All changes are in a single HTML file (`studio-zarq-novo.html`). Task 1 updates two `href` attributes. Tasks 2–4 add CSS, HTML, and JS for the hamburger overlay — each isolated to a clear section of the file. No external dependencies are introduced.

**Tech Stack:** Plain HTML, CSS, vanilla JavaScript (no frameworks, no libraries)

---

## File Map

| File | What changes |
|---|---|
| `studio-zarq-novo.html` | Update 2 `href` attrs; add hamburger CSS inside existing `@media (max-width: 900px)` block; add hamburger `<button>` and overlay `<div>` to `<body>`; add `<script>` block before `</body>` |

---

### Task 1: Update WhatsApp URLs with pre-filled message

**Files:**
- Modify: `studio-zarq-novo.html` lines ~504 and ~531

- [ ] **Step 1: Update the nav "Orçamento" button href (line ~504)**

Find:
```html
<a href="https://wa.me/5511984493780" class="nav-cta">Orçamento</a>
```

Replace with:
```html
<a href="https://wa.me/5511984493780?text=Olá!%20Gostaria%20de%20solicitar%20um%20orçamento%20😊" class="nav-cta">Orçamento</a>
```

- [ ] **Step 2: Update the hero "Solicitar orçamento" button href (line ~531)**

Find:
```html
<a href="https://wa.me/5511984493780" class="btn-primary">
```

Replace with:
```html
<a href="https://wa.me/5511984493780?text=Olá!%20Gostaria%20de%20solicitar%20um%20orçamento%20😊" class="btn-primary">
```

- [ ] **Step 3: Verify in browser**

Open `studio-zarq-novo.html` in a browser. On desktop, click "Orçamento" in the nav — WhatsApp should open with "Olá! Gostaria de solicitar um orçamento 😊" pre-filled. Repeat for the hero "Solicitar orçamento" button.

---

### Task 2: Add hamburger + overlay CSS

**Files:**
- Modify: `studio-zarq-novo.html` — inside `<style>`, after the existing `@media (max-width: 900px)` block (line ~488, just before `</style>`)

- [ ] **Step 1: Add hamburger button and overlay CSS**

Find the closing `</style>` tag (line ~489) and insert the following block immediately before it:

```css
    /* HAMBURGER MENU */
    .hamburger {
      display: none;
      flex-direction: column;
      justify-content: space-between;
      width: 28px; height: 20px;
      background: none; border: none;
      cursor: pointer; padding: 0;
      z-index: 1100;
    }
    .hamburger span {
      display: block;
      width: 100%; height: 2px;
      background: #C9B96A;
      border-radius: 2px;
      transition: opacity 0.2s;
    }

    .nav-overlay {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(26,37,53,0.97);
      z-index: 1050;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 0;
    }
    .nav-overlay.is-open {
      display: flex;
    }
    .nav-overlay a {
      color: rgba(201,185,154,0.85);
      font-size: 22px;
      letter-spacing: 3px;
      text-transform: uppercase;
      text-decoration: none;
      padding: 18px 0;
      transition: color 0.2s;
    }
    .nav-overlay a:hover { color: #C9B96A; }
    .nav-overlay .nav-cta {
      margin-top: 24px;
      background: #C9B96A;
      color: #1a2535;
      padding: 14px 36px;
      border-radius: 2px;
      font-size: 12px;
      letter-spacing: 2px;
      font-weight: 600;
    }
    .nav-overlay .nav-cta:hover {
      background: #e5d5a0;
      color: #1a2535;
    }
    .overlay-close {
      position: absolute;
      top: 22px; right: 28px;
      background: none; border: none;
      color: #C9B96A;
      font-size: 32px;
      cursor: pointer;
      line-height: 1;
      padding: 0;
    }

    @media (max-width: 900px) {
      .hamburger { display: flex; }
    }
```

- [ ] **Step 2: Update the existing mobile rule to hide nav links**

Find (in the existing `@media (max-width: 900px)` block, line ~473):
```css
      nav { gap: 16px; }
      .nav-cta { display: none; }
```

Replace with:
```css
      nav { display: none; }
```

- [ ] **Step 3: Verify CSS loads without errors**

Open `studio-zarq-novo.html` in a browser. Open DevTools Console — no CSS errors should appear. At desktop width the layout should be unchanged.

---

### Task 3: Add hamburger button and overlay HTML

**Files:**
- Modify: `studio-zarq-novo.html` — inside `<body>`, in the `<header>` and just after it

- [ ] **Step 1: Add hamburger button to the header**

Find (line ~504–505):
```html
    <a href="https://wa.me/5511984493780?text=Olá!%20Gostaria%20de%20solicitar%20um%20orçamento%20😊" class="nav-cta">Orçamento</a>
  </nav>
</header>
```

Replace with:
```html
    <a href="https://wa.me/5511984493780?text=Olá!%20Gostaria%20de%20solicitar%20um%20orçamento%20😊" class="nav-cta">Orçamento</a>
  </nav>
  <button class="hamburger" id="hamburgerBtn" aria-label="Abrir menu">
    <span></span>
    <span></span>
    <span></span>
  </button>
</header>
```

- [ ] **Step 2: Add full-screen overlay after the closing `</header>` tag**

Find:
```html
</header>

<!-- HERO -->
```

Replace with:
```html
</header>

<!-- MOBILE NAV OVERLAY -->
<div class="nav-overlay" id="navOverlay">
  <button class="overlay-close" id="overlayClose" aria-label="Fechar menu">&times;</button>
  <a href="#servicos">Serviços</a>
  <a href="#portfolio">Projetos</a>
  <a href="#antes-depois">Antes &amp; Depois</a>
  <a href="#processo">Processo</a>
  <a href="#contato">Contato</a>
  <a href="https://wa.me/5511984493780?text=Olá!%20Gostaria%20de%20solicitar%20um%20orçamento%20😊" class="nav-cta">Orçamento</a>
</div>

<!-- HERO -->
```

- [ ] **Step 3: Verify HTML in browser**

Resize browser to < 900px wide. The hamburger icon (3 gold lines) should appear top-right. The nav links should be hidden. Desktop width should be completely unchanged.

---

### Task 4: Add JavaScript toggle

**Files:**
- Modify: `studio-zarq-novo.html` — add `<script>` block just before `</body>`

- [ ] **Step 1: Find the closing `</body>` tag**

Search for `</body>` near the end of the file.

- [ ] **Step 2: Insert the script block before `</body>`**

Find:
```html
</body>
```

Replace with:
```html
<script>
  (function () {
    var overlay = document.getElementById('navOverlay');
    var hamburger = document.getElementById('hamburgerBtn');
    var closeBtn = document.getElementById('overlayClose');

    function openMenu() {
      overlay.classList.add('is-open');
      document.body.style.overflow = 'hidden';
    }

    function closeMenu() {
      overlay.classList.remove('is-open');
      document.body.style.overflow = '';
    }

    hamburger.addEventListener('click', openMenu);
    closeBtn.addEventListener('click', closeMenu);

    overlay.querySelectorAll('a').forEach(function (link) {
      link.addEventListener('click', closeMenu);
    });
  })();
</script>
</body>
```

- [ ] **Step 3: Full verification**

Open `studio-zarq-novo.html` in a browser and verify all of the following:

**Mobile (resize to < 900px):**
- [ ] Hamburger icon (3 gold lines) is visible top-right
- [ ] Nav links in the header are hidden
- [ ] Tapping hamburger → full-screen dark overlay appears with all links + Orçamento CTA
- [ ] Tapping any nav link → overlay closes and page scrolls to that section
- [ ] Tapping X button → overlay closes
- [ ] While overlay is open, body scroll is locked (page behind doesn't scroll)

**Desktop (> 900px):**
- [ ] Hamburger icon is not visible
- [ ] Nav links and Orçamento CTA are visible in header as before
- [ ] No layout changes from original

**WhatsApp buttons:**
- [ ] Nav "Orçamento" → WhatsApp opens with "Olá! Gostaria de solicitar um orçamento 😊" pre-filled
- [ ] Hero "Solicitar orçamento" → same pre-filled message
- [ ] Overlay "Orçamento" → same pre-filled message
