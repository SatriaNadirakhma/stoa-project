# Stoa — Landing Page Design Document
> **Version:** 1.0  
> **Stack:** Astro + Tailwind CSS + Motion (Framer Motion) + Lenis  
> **Purpose:** Portfolio landing page for Apple Developer Academy application  
> **Platform:** Web (Desktop-first, responsive to mobile)  
> **Last Updated:** June 2025

---

## 0. Project Brief

**Product:** Stoa — A macOS productivity app for laptop-based workers  
**Audience:** Mahasiswa, freelancer, remote workers, developers, designers (age 18–32)  
**Page's Single Job:** Communicate Stoa's vision so clearly and beautifully that the viewer immediately *feels* what using the app would be like — and wants it.  
**Tone:** Calm. Philosophical. Confident. Not corporate. Not hype.

---

## 1. Design System

### 1.1 Color Palette

```css
/* === STOA COLOR TOKENS === */

--color-bg:           #080808;   /* Near-black canvas — void, focus */
--color-surface:      #111111;   /* Card/panel background */
--color-surface-2:    #1A1A1A;   /* Elevated surface, hover states */
--color-border:       #242424;   /* Subtle dividers */
--color-border-light: #2E2E2E;   /* Slightly visible borders */

--color-text-primary:    #F0EDE6; /* Warm white — not harsh pure white */
--color-text-secondary:  #7A7773; /* Muted gray for subtext */
--color-text-tertiary:   #3D3B38; /* Very muted, labels, decorative text */

--color-accent:       #A89268;   /* Warm gold-stone — aged marble, Stoic columns */
--color-accent-dim:   #6B5E42;   /* Accent at low opacity, borders */
--color-accent-glow:  rgba(168, 146, 104, 0.12); /* Accent glow for backgrounds */

--color-success:      #6B8F71;   /* Muted sage green — task completion */
--color-white:        #FFFFFF;
```

**Design rationale:**  
The near-black background creates a "sanctuary" — the world drops away. The warm gold accent (`#A89268`) evokes aged marble, Stoic architecture, and classical philosophy — deliberately distinct from the typical blue/green productivity app palette. The warm white (`#F0EDE6`) softens the darkness without feeling clinical.

---

### 1.2 Typography

```css
/* === FONT FAMILIES === */

/* Display — Cormorant Garamond (Google Fonts) */
/* Used for: Hero headline, section titles, pull quotes */
/* Weight: 300, 400, 500 */
/* Character: Elegant, literary, classical. Carries philosophical weight. */
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;1,300;1,400&display=swap');

/* Body — DM Sans (Google Fonts) */
/* Used for: Body copy, UI text, labels, CTAs */
/* Weight: 300, 400, 500, 600 */
/* Character: Clean, neutral, highly legible. Modern without being sterile. */
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&display=swap');

/* Mono — JetBrains Mono (Google Fonts) */
/* Used for: Keyboard shortcuts, code-like labels, version tags */
/* Weight: 400 */
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400&display=swap');
```

```css
/* === TYPE SCALE === */

/* Hero Display */
--text-display:    clamp(56px, 8vw, 96px);
--leading-display: 0.95;
--tracking-display: -0.04em;
--font-display: 'Cormorant Garamond', Georgia, serif;
--weight-display: 300;

/* Section Title */
--text-title:    clamp(36px, 5vw, 60px);
--leading-title: 1.05;
--tracking-title: -0.03em;
--font-title: 'Cormorant Garamond', Georgia, serif;
--weight-title: 400;

/* Subtitle / Lead */
--text-lead:    clamp(18px, 2vw, 22px);
--leading-lead: 1.6;
--tracking-lead: -0.01em;
--font-lead: 'DM Sans', sans-serif;
--weight-lead: 300;

/* Body */
--text-body:    16px;
--leading-body: 1.7;
--font-body: 'DM Sans', sans-serif;
--weight-body: 400;

/* Caption / Label */
--text-caption:    12px;
--leading-caption: 1.4;
--tracking-caption: 0.08em;
--font-caption: 'DM Sans', sans-serif;
--weight-caption: 500;
/* Always uppercase for labels */

/* Mono / Shortcut */
--text-mono:    13px;
--font-mono: 'JetBrains Mono', monospace;
--weight-mono: 400;
```

---

### 1.3 Spacing & Layout

```css
/* === SPACING TOKENS === */
--space-xs:   4px;
--space-sm:   8px;
--space-md:   16px;
--space-lg:   24px;
--space-xl:   40px;
--space-2xl:  64px;
--space-3xl:  96px;
--space-4xl:  128px;
--space-5xl:  192px;

/* === LAYOUT === */
--container-max:     1120px;
--container-padding: clamp(24px, 5vw, 80px);
--section-gap:       clamp(96px, 12vw, 160px);

/* === BORDER RADIUS === */
--radius-sm:   6px;
--radius-md:   12px;
--radius-lg:   20px;
--radius-xl:   32px;
--radius-full: 9999px;

/* === BORDER === */
--border-default: 1px solid var(--color-border);
--border-accent:  1px solid var(--color-accent-dim);
```

---

### 1.4 Motion & Animation

```css
/* === EASING === */
--ease-out-expo:   cubic-bezier(0.16, 1, 0.3, 1);
--ease-out-quart:  cubic-bezier(0.25, 1, 0.5, 1);
--ease-in-out:     cubic-bezier(0.4, 0, 0.2, 1);
--ease-spring:     cubic-bezier(0.34, 1.56, 0.64, 1);

/* === DURATION === */
--duration-fast:    150ms;
--duration-default: 300ms;
--duration-slow:    600ms;
--duration-slower:  1000ms;

/* === GLOBAL RULES === */
/* - All scroll-triggered: fade-up (translateY: 20px → 0, opacity: 0 → 1) */
/* - Stagger delay between sibling elements: 80ms */
/* - Hover states: 200ms transition */
/* - Page load sequence: elements enter staggered, 0ms → 600ms window */
/* - Respect prefers-reduced-motion: disable transforms, keep opacity fades */
```

**Lenis Config:**
```javascript
const lenis = new Lenis({
  duration: 1.4,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smooth: true,
});
```

---

### 1.5 Component Styles

#### Button — Primary CTA
```
Background: var(--color-accent)
Text: #080808 (dark on gold)
Font: DM Sans 500, 14px, tracking 0.02em
Padding: 12px 28px
Border-radius: var(--radius-full)
Hover: brightness(1.1), translateY(-1px)
Transition: 200ms ease-out
```

#### Button — Secondary / Ghost
```
Background: transparent
Border: 1px solid var(--color-border-light)
Text: var(--color-text-primary)
Font: DM Sans 400, 14px
Padding: 11px 24px
Border-radius: var(--radius-full)
Hover: border-color var(--color-accent-dim), text var(--color-accent)
Transition: 200ms ease-out
```

#### Keyboard Shortcut Badge
```html
<kbd>⌘ D</kbd>
```
```
Background: var(--color-surface-2)
Border: 1px solid var(--color-border-light)
Font: JetBrains Mono 400, 12px
Color: var(--color-text-secondary)
Padding: 3px 8px
Border-radius: var(--radius-sm)
```

#### Feature Card
```
Background: var(--color-surface)
Border: 1px solid var(--color-border)
Border-radius: var(--radius-lg)
Padding: 32px
Hover: border-color var(--color-border-light), background var(--color-surface-2)
Transition: 300ms ease-out
```

#### Eyebrow / Section Label
```
Font: DM Sans 500, 11px
Color: var(--color-accent)
Letter-spacing: 0.12em
Text-transform: uppercase
Display: inline-flex with a 16px hairline line before the text
```

---

### 1.6 Signature Element

**The Arch Line Art** — Stoa's single most memorable visual.

A large SVG illustration of a classical arch (the architectural stoa/portico), drawn as minimal linework in `var(--color-border-light)`. It sits behind the hero text as a decorative element at ~60% viewport height. On scroll, it slowly rotates or parallaxes. On mouse move, it subtly responds (±4px translate), creating a sense of depth.

The arch symbolizes: structure, clarity, a gateway into focused work — the Stoa as literal and metaphorical frame.

---

## 2. Page Structure

```
stoa-landing/
│
├── [00] Navbar
├── [01] Hero
├── [02] Problem Statement
├── [03] Philosophy
├── [04] Features
├── [05] Daily Ritual (How It Works)
├── [06] App Preview
├── [07] Comparison
├── [08] Download CTA
└── [09] Footer
```

---

## 3. Section Specifications

---

### [00] Navbar

**Layout:** Fixed, top, full-width. Blurs/darkens on scroll.

**Height:** 60px

**Left:** Logo — `Stoa` wordmark in Cormorant Garamond 400, 22px, color `--color-text-primary`. Followed by a small badge: `macOS` in mono font, `--color-text-tertiary`, with a `--color-border` border.

**Center (Desktop):** Navigation links — `Features`, `How It Works`, `Preview`, `Download`. DM Sans 400, 14px, `--color-text-secondary`. Hover: `--color-text-primary`. Transition 150ms.

**Right:** 
- `Sign up for early access` → Ghost button  
- `Download for free` → Primary CTA button (smaller: 10px 20px padding)

**Scroll behavior:**
```css
/* On scroll > 20px: */
background: rgba(8, 8, 8, 0.85);
backdrop-filter: blur(20px) saturate(180%);
border-bottom: 1px solid var(--color-border);
transition: all 300ms ease;
```

**Mobile:** Hamburger menu, slides in from right. Full-screen overlay dark panel.

---

### [01] Hero

**Layout:** 100vh, centered content, max-width 900px, horizontally centered.

**Background:** Pure `--color-bg`. The Arch SVG (Signature Element) renders centered behind text at 55% viewport height, z-index 0. Text z-index 1.

**Animation sequence on page load:**
```
0ms   → Arch SVG fades in, opacity 0 → 0.15 (1000ms, ease-out)
200ms → Eyebrow label fades up
350ms → Headline line 1 fades up
450ms → Headline line 2 fades up
600ms → Subheadline fades up
750ms → CTA buttons fade up
900ms → Scroll indicator fades in
```

**Content:**

```
[EYEBROW LABEL]
— For those who think deeply and work quietly —

[HEADLINE — Cormorant Garamond 300, display size, centered]
Your mind,
finally quiet.

[SUBHEADLINE — DM Sans 300, lead size, --color-text-secondary, max-width 520px, centered]
Stoa is a sanctuary for laptop workers who are tired of managing
their task manager. Choose five. Do the work. Rest.

[CTA BUTTONS — horizontal stack, centered, gap 12px]
[Primary]  Download for macOS  — free
[Ghost]    See how it works ↓

[SCROLL INDICATOR — bottom center, 40px from bottom]
Small animated line pulsing downward, --color-text-tertiary
```

**Arch SVG Spec:**
```
viewBox: 0 0 600 700
Stroke: var(--color-border-light) — #2E2E2E
Stroke-width: 1px
Fill: none
Opacity: 0.18 (desktop), 0.08 (mobile)
Shape: Classical Roman/Greek arch — two vertical columns, semicircular top arch, 
       with subtle entablature lines. Minimalist — 8-12 path segments maximum.
Mouse parallax: ±4px on X and Y axis, 600ms ease-out response
```

---

### [02] Problem Statement

**Layout:** Full-width section. `--section-gap` top and bottom padding. Max-width container centered.

**Background:** Same as bg. A very subtle horizontal hairline divides top.

**Content:**

```
[SECTION LABEL]
The Problem

[LARGE PULL QUOTE — Cormorant Garamond 300, clamp(40px, 5.5vw, 72px), max-width 800px]
"Most to-do apps
make you feel busier,
not more focused."

[BODY PARAGRAPH — DM Sans 300, 18px, --color-text-secondary, max-width 600px, margin-top 40px]
You open your task list. 47 items stare back.
You close it without doing anything.
Sound familiar?

[THREE PROBLEM CARDS — horizontal grid, 3 columns desktop / 1 column mobile]
```

**Problem Cards (3 cards):**

```
Card 1:
Icon: A single line that branches into too many paths (SVG)
Title: Task Overload Anxiety
Body: Seeing every task at once triggers paralysis, not action. 
      Your brain freezes before you begin.

Card 2:
Icon: Two overlapping circles (context switching)
Title: Constant Context Switching
Body: Notifications, browser tabs, Slack pings — your laptop is 
      the world's most advanced distraction machine.

Card 3:
Icon: An empty checkbox that repeats infinitely
Title: Lists That Don't Evolve
Body: Yesterday's tasks bleed into today. Old guilt piles on 
      new anxiety. The list never ends.
```

**Card style:** Feature Card spec from 1.5. Icons: SVG, hand-drawn minimal style, 32px, `--color-accent` stroke.

**Animation:** Cards stagger-fade-up on scroll. 80ms delay between each.

---

### [03] Philosophy

**Layout:** Full-width. Dark surface panel (`--color-surface`) spanning full width, acting as a visual break. Generous vertical padding (96px).

**Content:**

```
[SECTION LABEL — centered]
The Stoa Approach

[TITLE — Cormorant Garamond 400, title size, centered, max-width 700px]
Built on 2,000 years
of focused thought.

[STOIC QUOTE — Cormorant Garamond 300 italic, 28px, --color-text-secondary, 
               centered, max-width 600px, with decorative quotation marks in --color-accent]
"Confine yourself to the present."
— Marcus Aurelius

[BODY — DM Sans 300, 17px, --color-text-secondary, centered, max-width 560px, margin-top 32px]
The Stoics didn't fight chaos — they ignored it.
Stoa is built around that same principle: instead of organizing 
everything, focus on what matters right now, today.
One day. Five tasks. Full presence.

[THREE PRINCIPLES — horizontal, centered, gap 48px, margin-top 64px]
```

**Three Principles (inline, minimal):**

```
I. Control
Only what you choose enters your day.
Everything else waits in The Vault.

II. Presence  
One task at a time. Focus Mode locks 
the world out so you can go deep.

III. Reflection
Each evening, review. Each morning, 
begin again with clarity.
```

**Principle style:** Number in Cormorant Garamond 300 italic `--color-accent` 40px. Title DM Sans 500 14px uppercase `--color-text-primary`. Body DM Sans 300 15px `--color-text-secondary`. Dividing hairline in `--color-border` between each on mobile.

---

### [04] Features

**Layout:** Standard section. Section label top-left. Title top-left. Then a 2-column asymmetric grid: left 55%, right 45% (alternates on each row).

**Section Label:** Features  
**Title:**
```
[Cormorant Garamond 400, title size, max-width 480px]
Everything you need.
Nothing you don't.
```

**Features List (5 features, alternating layout):**

---

**Feature 1 — Daily Sanctuary**
```
Label: Home Screen
Title: Five tasks. That's your day.
Body: Every morning, Stoa asks you one question: what truly matters today? 
      Choose up to five things. The rest waits. Your day suddenly feels possible.
Visual: Dark mockup of home screen showing 5 task items, clean typography, 
        a subtle time greeting at top. Screenshot or illustrated UI.
Keyboard shortcut badge: Space — Add task
```

**Feature 2 — Focus Mode**
```
Label: Deep Work
Title: Enter the room. Close the door.
Body: Activate Focus Mode and the world disappears. One task. A Pomodoro timer. 
      No notifications. Just you and the work that matters.
Visual: Full-screen mockup of Focus Mode — single task name large, timer below, 
        everything else dark.
Keyboard shortcut badge: Enter — Start focus
```

**Feature 3 — Natural Language Input**
```
Label: Quick Capture
Title: Type like you think.
Body: "Revisi proposal besok jam 2 siang" — Stoa understands. Date, time, 
      and priority parsed instantly. No menus. No friction. No lost thoughts.
Visual: Animated typewriter showing natural language being parsed into structured task.
        Input field → parsed result with tags appearing. Loop animation.
```

**Feature 4 — The Vault**
```
Label: Smart Backlog
Title: Everything stored. Nothing forgotten.
Body: Tasks not chosen for today live in The Vault — organized, searchable, 
      sorted by energy level and deadline. Out of sight, never out of mind.
Visual: Side-panel UI mockup of The Vault showing tasks grouped by energy: 
        ⚡ High / 🌿 Medium / 🌙 Low
Keyboard shortcut badge: ⌘ V — Open Vault
```

**Feature 5 — Evening Reflection**
```
Label: Daily Ritual
Title: End the day with intention.
Body: Each evening, Stoa shows you what you accomplished and asks one question. 
      Incomplete tasks move to the Vault — no guilt, just tomorrow.
Visual: Reflection screen mockup — completion ring animation, single prompt question,
        simple text input below.
```

---

**Feature Layout Pattern (per feature):**
```
Row (alternating left/right):
├── Text side (55%): Label → Title → Body → Optional shortcut badge → spacer
└── Visual side (45%): App mockup or animation in rounded dark frame
    Frame style: background #0D0D0D, border 1px solid --color-border, 
                 border-radius 20px, padding 24px, subtle drop shadow
```

**Animation:** Each feature row fades in on scroll. Visual side has slight parallax (scrolls 10% slower than viewport).

---

### [05] Daily Ritual (How It Works)

**Layout:** Full-width dark surface section (`--color-surface`). Centered content. Timeline-style layout.

**Section Label:** How It Works  
**Title:**
```
[Cormorant Garamond 400, title size, centered]
A rhythm your day
will thank you for.
```

**Subtitle:**
```
[DM Sans 300, 18px, --color-text-secondary, centered, max-width 480px]
Stoa doesn't just hold your tasks — it shapes your day 
into three intentional moments.
```

**Three Ritual Steps (vertical timeline, centered, max-width 600px):**

```
Step 1 — Morning Planning (⌘ M)
Time: 7:00 – 9:00 AM
Title: Begin with intention
Body: Open Stoa. See yesterday's reflection. Choose your five for today 
      from The Vault or create new ones. Close the app. Start working.
Indicator: Accent dot on left timeline line

Step 2 — Focus Sessions (Enter)
Time: Throughout the day  
Title: Go deep, one task at a time
Body: Whenever you're ready to work, activate Focus Mode. 
      25 minutes of presence. 5 minutes of rest. Repeat.
Indicator: Accent dot on left timeline line

Step 3 — Evening Reflection (⌘ .)
Time: 6:00 – 8:00 PM
Title: Close the loop
Body: See what you accomplished. Answer one question about your day. 
      Unfinished tasks move to The Vault — no judgment, just tomorrow.
Indicator: Accent dot on left timeline line
```

**Timeline style:**
- Vertical line: 1px solid `--color-border`, centered at left of content
- Dots: 8px circle, `--color-accent`, with subtle glow (box-shadow: 0 0 12px var(--color-accent-glow))
- Each step: opacity 0 on load → fades in as its dot passes viewport center on scroll
- Time label: DM Sans 500 11px uppercase `--color-text-tertiary` tracking 0.1em
- Title: DM Sans 500 18px `--color-text-primary`
- Body: DM Sans 300 15px `--color-text-secondary`

---

### [06] App Preview

**Layout:** Full-width. Large centered app screenshot mockup framed in a macOS window chrome.

**Section Label:** Preview  
**Title:**
```
[Cormorant Garamond 400, title size, centered]
Designed for the way
you actually work.
```

**Subtitle:**
```
[DM Sans 300, 17px, --color-text-secondary, centered, max-width 500px]
Dark by default. Keyboard-first. Four accent colors to choose from.
Exactly as minimal as it needs to be.
```

**Main Visual:**
```
macOS window mockup (fake traffic-light buttons: close/minimize/fullscreen)
Window chrome: background #0F0F0F, border-radius 14px
Padding inside: 0 (content fills edge to edge below title bar)
Content: High-fidelity mockup of Stoa home screen
         - Top: "Good evening, Rizky." greeting in Cormorant Garamond 300
         - Task list: 3 tasks visible, one checked with a subtle line-through
         - Bottom bar: Focus Mode button, Vault icon, Reflection icon
Width: max 960px, centered on page
Shadow: 0 40px 120px rgba(0,0,0,0.8), 0 0 0 1px var(--color-border)
```

**Below main preview — 3 mini previews side by side:**
```
[Focus Mode screen]   [The Vault screen]   [Evening Reflection screen]
Each: rounded rect, 280px wide, same dark frame style
Hover: scale(1.02), shadow intensifies
```

**Accent Color Switcher (interactive):**
```
Label: "Choose your accent"
4 small circular swatches:
  ○ Stone   #A89268 (default)
  ○ Forest  #6B8F71
  ○ Dusk    #8B7BA8
  ○ Ink     #7890A8

On click: CSS custom property --color-accent updates across entire preview section
Animation: 300ms color transition across all accent elements in preview
```

---

### [07] Comparison

**Layout:** Standard section. Title left-aligned. Then a single comparison table centered.

**Section Label:** Why Stoa  
**Title:**
```
[Cormorant Garamond 400, title size, max-width 480px]
Not another
task manager.
```

**Body:**
```
[DM Sans 300, 17px, --color-text-secondary, max-width 480px]
The difference isn't features — it's philosophy.
Stoa is built around what you remove, not what you add.
```

**Comparison Table:**

```
| Feature                          | Stoa  | Notion | Todoist | Reminders |
|----------------------------------|-------|--------|---------|-----------|
| Daily 5-task limit               |  ✦   |   —    |   —     |    —      |
| Built-in Focus + Pomodoro        |  ✦   |   —    |   —     |    —      |
| Evening reflection ritual        |  ✦   |   —    |   —     |    —      |
| Natural language task input      |  ✦   |   ◐    |   ✦     |    —      |
| Energy-based task sorting        |  ✦   |   —    |   —     |    —      |
| Keyboard-first macOS design      |  ✦   |   ◐    |   ◐     |    ◐      |
| Minimalist, distraction-free UI  |  ✦   |   —    |   ◐     |    ✦      |
| Philosophy-driven productivity   |  ✦   |   —    |   —     |    —      |

Legend:
✦ = Full support  |  ◐ = Partial  |  — = Not supported
```

**Table style:**
```
Background: --color-surface
Border: --color-border
Border-radius: --radius-lg
Header row: DM Sans 500 12px uppercase tracking 0.08em --color-text-secondary
Stoa column header: --color-accent colored, subtle --color-accent-glow background
✦ mark: --color-accent, DM Sans 600 16px
◐ mark: --color-text-secondary
— mark: --color-text-tertiary
Row hover: background rgba(255,255,255,0.02)
```

---

### [08] Download CTA

**Layout:** Full-width section. Centered. The arch SVG reappears here at lower opacity (0.08) — creating visual bookend with hero.

**Background:** Subtle radial gradient from center:  
`radial-gradient(ellipse at center, rgba(168, 146, 104, 0.06) 0%, transparent 70%)`

**Content:**

```
[SECTION LABEL — centered]
Get Stoa

[TITLE — Cormorant Garamond 300, display size, centered, max-width 700px]
Your sanctuary
awaits.

[SUBHEADLINE — DM Sans 300, 18px, --color-text-secondary, centered, max-width 460px]
Free for macOS. Built for humans who want 
to do their best work — quietly.

[CTA BUTTONS — centered, stacked slightly, margin-top 48px]
[Primary — larger: 14px 36px padding] 
  ↓  Download for macOS  — Free
  
[Below button — DM Sans 300 13px --color-text-tertiary]
  macOS 14.0 or later · No account required · Works offline

[SOCIAL PROOF ROW — margin-top 40px, centered]
"Finally, a to-do app I actually want to open."
— Early access user
[small avatar circle placeholder] [small avatar circle placeholder] [small avatar circle placeholder]
+200 people on the waitlist
```

**Animation:**  
Title fades up. CTA button pulses once on enter (scale 1 → 1.03 → 1, 600ms). Arch appears as gentle parallax element.

---

### [09] Footer

**Layout:** Full-width. Border-top 1px `--color-border`. Padding 48px vertical.

**Content:**

```
LEFT:
Stoa wordmark (same as nav)
DM Sans 300 13px --color-text-tertiary
"Built with intention. Designed for focus."
© 2025 Stoa. Made for macOS.

CENTER:
Links (DM Sans 400 13px --color-text-secondary):
  Features  |  How It Works  |  Preview  |  Privacy Policy

RIGHT:
"Applying to Apple Developer Academy"
[Apple logo small icon, --color-text-tertiary]
Universitas Ciputra Surabaya · 2025
```

**Mobile:** Stacked vertically, center-aligned.

---

## 4. Responsive Breakpoints

```css
/* Mobile */
@media (max-width: 640px) {
  /* Hero: headline 48px, subhead 16px, buttons stacked */
  /* Feature rows: single column, visual below text */
  /* Problem cards: single column */
  /* Comparison table: horizontally scrollable */
  /* Nav: hamburger menu */
  /* Arch SVG: opacity 0.06, scale 80% */
}

/* Tablet */
@media (min-width: 641px) and (max-width: 1024px) {
  /* Feature rows: 50/50 split */
  /* Problem cards: 2+1 grid */
  /* Comparison table: full width */
}

/* Desktop */
@media (min-width: 1025px) {
  /* Full design as specified above */
}
```

---

## 5. Performance Notes

- All images/mockups: WebP format, max 200KB each
- SVGs inline (not as `<img>`) for smooth animation control
- Fonts: `display=swap`, preload critical weights
- Lenis smooth scroll: initialize after `DOMContentLoaded`
- Motion: initialize Intersection Observer for scroll triggers, not scroll event listeners
- Accent color switcher: single CSS variable update on `:root` (no JS DOM loop)

---

## 6. Astro File Structure

```
src/
├── components/
│   ├── Navbar.astro
│   ├── Hero.astro
│   ├── Problem.astro
│   ├── Philosophy.astro
│   ├── Features.astro
│   ├── DailyRitual.astro
│   ├── AppPreview.astro
│   ├── Comparison.astro
│   ├── Download.astro
│   └── Footer.astro
├── layouts/
│   └── Layout.astro          ← Global CSS vars, font imports, Lenis init
├── assets/
│   ├── arch.svg              ← The signature arch illustration
│   └── mockups/              ← App screen mockups (WebP)
├── styles/
│   └── global.css            ← Token definitions, reset, base styles
└── pages/
    └── index.astro           ← Assembles all components
```

---

## 7. Tailwind Config Extensions

```javascript
// tailwind.config.mjs
export default {
  theme: {
    extend: {
      colors: {
        stoa: {
          bg:       '#080808',
          surface:  '#111111',
          surface2: '#1A1A1A',
          border:   '#242424',
          'border-light': '#2E2E2E',
          text:     '#F0EDE6',
          muted:    '#7A7773',
          dim:      '#3D3B38',
          accent:   '#A89268',
          'accent-dim': '#6B5E42',
          success:  '#6B8F71',
        }
      },
      fontFamily: {
        display: ['Cormorant Garamond', 'Georgia', 'serif'],
        body:    ['DM Sans', 'sans-serif'],
        mono:    ['JetBrains Mono', 'monospace'],
      },
      animation: {
        'fade-up':    'fadeUp 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards',
        'pulse-once': 'pulseOnce 0.6s cubic-bezier(0.34, 1.56, 0.64, 1)',
        'breathe':    'breathe 4s ease-in-out infinite',
      },
      keyframes: {
        fadeUp: {
          '0%':   { opacity: '0', transform: 'translateY(20px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
        pulseOnce: {
          '0%':   { transform: 'scale(1)' },
          '50%':  { transform: 'scale(1.03)' },
          '100%': { transform: 'scale(1)' },
        },
        breathe: {
          '0%, 100%': { opacity: '0.15' },
          '50%':      { opacity: '0.22' },
        },
      }
    }
  }
}
```

---

## 8. Copy Summary (All Text Content)

| Section | Headline | Subheadline |
|---------|----------|-------------|
| Hero | "Your mind, finally quiet." | "Stoa is a sanctuary for laptop workers who are tired of managing their task manager. Choose five. Do the work. Rest." |
| Problem | "Most to-do apps make you feel busier, not more focused." | "You open your task list. 47 items stare back. You close it without doing anything. Sound familiar?" |
| Philosophy | "Built on 2,000 years of focused thought." | "The Stoics didn't fight chaos — they ignored it. Stoa is built around that same principle." |
| Features | "Everything you need. Nothing you don't." | — |
| How It Works | "A rhythm your day will thank you for." | "Stoa doesn't just hold your tasks — it shapes your day into three intentional moments." |
| Preview | "Designed for the way you actually work." | "Dark by default. Keyboard-first. Four accent colors to choose from." |
| Comparison | "Not another task manager." | "The difference isn't features — it's philosophy." |
| Download | "Your sanctuary awaits." | "Free for macOS. Built for humans who want to do their best work — quietly." |

---

## 9. Agent Build Instructions

When building this landing page, follow this sequence:

1. **Setup** — Initialize Astro project, install Tailwind, configure `tailwind.config.mjs` with Stoa tokens
2. **Global styles** — CSS custom properties in `global.css`, font imports, base reset
3. **Layout.astro** — Head meta, font preloads, Lenis initialization script
4. **Arch SVG** — Create/source the arch illustration, optimize as inline SVG
5. **Build components top to bottom** — Navbar → Hero → Problem → Philosophy → Features → DailyRitual → AppPreview → Comparison → Download → Footer
6. **App mockups** — Use CSS/HTML to simulate app screens if no actual screenshots exist. Dark frames with realistic UI typography.
7. **Animations** — Add Motion scroll triggers after all components render correctly
8. **Accent switcher** — Wire up interactive color swatches in AppPreview
9. **Responsive** — Test and fix breakpoints mobile → tablet → desktop
10. **Performance** — Optimize images, check Lighthouse score

**Critical design rules:**
- Never use pure white (#FFFFFF) for body text — always `--color-text-primary: #F0EDE6`
- Never add more than one accent color to any single view
- Minimum 40px between any two sections
- All section labels must be uppercase with tracking
- Cormorant Garamond is ONLY for display/title text — never use for body
- Every feature visual must be in the same dark rounded frame style
- The arch SVG must appear in BOTH hero and download CTA — it bookends the page

---

*End of Design Document — Stoa Landing Page v1.0*
