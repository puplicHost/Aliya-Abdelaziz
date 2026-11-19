# Portfolio — How It Works & Project Layout

Reference documentation for the Aliaa Abdelaziz personal portfolio. This file is
documentation only; it does not affect the site.

## 1. Overview

This is a **static, single-page portfolio**. There is no build step, framework, or
backend. The entire site lives in one file — `index.html` — with all styles,
scripts, and content embedded. Everything you see is rendered by the browser
directly from that file.

- **Type:** static site (plain HTML + CSS + JavaScript)
- **Pages:** one (`index.html`)
- **Languages:** English (default) and Arabic, with a runtime language switcher
- **Hosting:** any static host or local folder — just open `index.html` in a browser

## 2. How the site works

### 2.1 Styling (Tailwind CDN)

`index.html` loads the [Tailwind CSS browser CDN](https://cdn.tailwindcss.com) in
`<head>`:

```html
<script src="https://cdn.tailwindcss.com?plugins=forms,typography,container-queries"></script>
```

- Utility classes (`flex`, `grid`, `text-white`, etc.) are generated **in the
  browser at runtime**, so there is no compiled CSS file.
- A custom `tailwind.config` block defines the design tokens:
  - `primary` = `#2D1ED7` (indigo/purple accent)
  - `background-dark` = `#050505` (main dark background)
  - `background-light` = `#f8fafc`
  - Display font = `Space Grotesk`, body font = `Inter`
- A `<style type="text/tailwindcss">` block adds custom classes
  (`.glass-card`, `.reveal`, `.stagger`, `.hero-fade`, `.glow-button`, etc.) and
  the Arabic/RTL overrides.
- Fonts (Inter, Space Grotesk, Cairo, Material Symbols) are loaded from Google
  Fonts. An internet connection is therefore required for correct styling.

### 2.2 Page sections

The body is a vertical stack of `<section>` blocks and a fixed `<nav>`:

| # | Section (anchor) | Purpose |
|---|---|---|
| 1 | `<nav>` (fixed) | Logo, desktop links, language switcher, "Download CV", mobile menu |
| 2 | hero (top, no id) | Name, headline "Crafting Digital Success Stories", profile image |
| 3 | `#about` | Bio, three highlight stats (4+ years, Gulf, 40+ projects) |
| 4 | `#skills` | Animated skill bars (Brand Strategy, Account Management, etc.) |
| 5 | expertise (no id) | Three expertise cards (Strategy, Account & Team Mgmt, Problem Solving) |
| 6 | `#portfolio` | Grid of 20 project cards linking to Canva or local PDFs |
| 7 | proficiency (no id) | Languages and tools (Arabic, English, Adobe/Canva, MS Office) |
| 8 | `#education` | Education timeline (Faculty of Applied Arts) |
| 9 | `#experience` | Professional journey at Mulhamah with five responsibility items |
| 10 | CTA + `<footer>` | Call-to-action, contact details (email, WhatsApp), quick links |

Navigation links are plain hash anchors (`href="#about"`, `#skills`, etc.).

### 2.3 Bilingual support (English / Arabic)

All switching logic is in the final `<script>` block at the bottom of the file.

- **Static text:** any element with a `data-i18n="key"` attribute is translated.
  The `translations` object holds `en` and `ar` dictionaries of `key → HTML`.
- **Dynamic portfolio cards:** the `portfolioData` object holds parallel arrays of
  20 `{ title, category }` entries per language. The switcher matches cards by
  their position in the grid (`#portfolio .grid > a` → index). The "View Work"
  label is also swapped (`عرض العمل` in Arabic).
- **Direction:** `setLanguage()` sets `<html lang>` and `<html dir>` (`rtl` for
  Arabic, `ltr` for English). Tailwind CDN does not auto-flip spacing utilities
  for RTL, so the `<style>` block contains manual `html[dir="rtl"] ...` overrides.
- **Persistence:** the chosen language is saved in `localStorage` under
  `portfolio-lang` and restored on load. Buttons show the *target* language
  (English page shows "العربية"; Arabic page shows "English").

To add or edit a translated string, update **both** the `en` and `ar` objects in
`translations`, or both arrays in `portfolioData`.

### 2.4 Interactions (JavaScript)

The same bottom `<script>` block implements:

- **Mobile menu:** toggles `-translate-y-4`, `opacity-0`, `pointer-events-none`
  classes on `#mobile-menu`; closes when a `.mobile-link` is clicked.
- **Scroll-reveal animations:** an `IntersectionObserver` adds `.visible` to
  `.reveal`, `.stagger`, `.skill-fill`, and `.hero-fade` elements as they enter
  the viewport. For `.skill-fill` it sets `width` to the element's `data-width`.
- **Counters:** a second `IntersectionObserver` animates `.counter` elements
  from 0 to their `data-target` value (e.g. `92%`) when 50% visible.
- **Navbar behavior:** on scroll past 80px the nav gets `.nav-scrolled`; scrolling
  down past 200px hides it (`translateY(-100%)`), scrolling up shows it again.
- **Language toggle:** `handleLangToggle()` flips between `en`/`ar` on either
  language button (desktop and mobile).

### 2.5 Links and assets

- **CV download:** the "Download CV" buttons link to `cv.pdf` with the `download`
  attribute.
- **Portfolio cards:** the first five cards link to external Canva presentations
  (`https://canva.link/...`, opened in a new tab). The remaining 15 link to local
  PDF files inside `pdfs/`.
- **Contact:** email via `mailto:aliaa99albdelaziz@gmail.com`; WhatsApp via
  `https://wa.me/201003347128`.
- All image/icon paths are **relative**, so the folder structure must be kept
  intact when deploying.

## 3. Project layout

```
Aliya-Abdelaziz/
├── index.html      # The entire website: markup + Tailwind config + custom CSS
                    #   + all content + all JavaScript (single source of truth)
├── cv.pdf          # Résumé served by the "Download CV" buttons
├── fav.png         # Browser favicon (linked in <head>)
├── README.md       # Minimal project stub (currently just the project title)
├── HOW_IT_WORKS.md # This documentation file
├── icons/          # Small PNG illustrations used by the "expertise" cards
│   ├── ads.png
│   ├── business-plan.png
│   ├── persuasion.png
│   └── strategy-development.png
├── images/
│   └── profile.png # Profile photo shown in the hero section
└── pdfs/           # 15 downloadable work samples opened by portfolio cards
    ├── build plus - marketing strategy.pdf
    ├── Colomby Coffee Shop opening.pdf
    ├── Copy of marketing Presentation.pdf
    ├── mood board (4).pdf
    ├── Nebras Code.pdf
    ├── SUADI TRACKING PLAN.pdf
    ├── استراتيجية تسويقية لارنج.pdf
    ├── استراتيجية تسويقية لشركة سند.pdf
    ├── الخطة التسويقية   لقوه.pdf
    ├── المحتوى النصي للمنشورات.pdf+ خطة النشر.pdf
    ├── رزنامة شهرية.pdf
    ├── روح بيروت.pdf
    ├── مطعم كبسة .pdf
    ├── مقترحات ليوم التاسيس.pdf
    └── هيكس استراتيجي.pdf
```

### File roles at a glance

| Path | Used by | Notes |
|---|---|---|
| `index.html` | everything | Edit here for all content, styling, and behavior |
| `cv.pdf` | nav + CTA "Download CV" | Replace file to update the résumé |
| `fav.png` | `<head>` favicon | Browser tab icon |
| `images/profile.png` | hero section | Profile picture |
| `icons/*.png` | expertise cards | Four card illustrations |
| `pdfs/*.pdf` | portfolio cards | Work samples; keep filenames in sync with card `href`s |
| `README.md` | — | Placeholder only |

## 4. Common maintenance tasks

- **Change text:** edit the element's default text **and** its `data-i18n` values
  in both `translations.en` and `translations.ar`.
- **Add a portfolio item:** add an `<a>` card in the `#portfolio` grid and a
  matching entry at the same index in both `portfolioData.en` and
  `portfolioData.ar`.
- **Replace a work sample:** drop the new PDF into `pdfs/` and keep the card's
  `href` pointing to the exact filename (including spaces).
- **Change colors/fonts:** edit the `tailwind.config` block and the CSS variables
  / custom classes in the `<style>` block.
- **Deploy:** upload the whole folder to any static host (e.g. GitHub Pages,
  Netlify, Vercel) or zip it. No build command is needed. Because Tailwind and
  fonts come from CDNs, the site needs internet access to render correctly.

## 5. Known quirks

- Tailwind runs from a CDN — the page relies on third-party scripts/fonts at
  load time and emits a console warning about CDN usage in production.
- Several portfolio comments are misnumbered (e.g. two cards labeled "2"); the
  ordering is driven by the `portfolioData` array indices, not the comments.
- `SUADI TRACKING PLAN.pdf` exists in `pdfs/` but is not referenced by any card;
  card 6 ("SA Tracking Report") links to a Canva URL instead.
- The `href` for the content-plan card contains a literal `+`
  (`المحتوى النصي للمنشورات.pdf+ خطة النشر.pdf`); keep it exactly as-is.
