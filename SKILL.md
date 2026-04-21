# SKILL.md — Personal Branding Website Builder

## For: Krishna Mohan MS MBA · AI Consultant · Educator

---

## PURPOSE

This skill guides the construction of Krishna Mohan's personal branding website. It defines design decisions, component patterns, content rules, and build constraints so that any future Claude session can pick up exactly where the last left off — with no ambiguity.

Always read CLAUDE.md first. This file handles the *how*; CLAUDE.md handles the *what*.

---

## DESIGN SYSTEM SNAPSHOT

### Color tokens (CSS variables)

```css
:root {
  --navy:        #0D1B2A;  /* primary dark */
  --navy-mid:    #162536;  /* dark section bg */
  --navy-light:  #1F3448;  /* hover state on dark */
  --amber:       #D4922A;  /* primary accent, CTAs */
  --amber-light: #F0B855;  /* hover, italic accents */
  --amber-glow:  rgba(212,146,42,0.10); /* subtle glow overlays */
  --cream:       #F5F0E8;  /* page background */
  --cream-dark:  #E8E0D0;  /* borders on light bg */
  --white:       #FFFFFF;  /* cards, inputs */
  --text-body:   #4A5568;  /* body copy on light */
  --text-muted:  #6B7A8A;  /* captions, secondary */
  --border-dark: rgba(245,240,232,0.08); /* borders on dark bg */
  --muted-dark:  rgba(245,240,232,0.45); /* text on dark bg */
}
```

### Typography stack

```css
/* Import */
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600&family=DM+Sans:wght@300;400;500&display=swap');

/* Display headings */
font-family: 'Cormorant Garamond', serif;
font-weight: 700;

/* Body / UI */
font-family: 'DM Sans', sans-serif;
font-weight: 300; /* body */
font-weight: 400; /* default */
font-weight: 500; /* labels, buttons */
```

### Section label pattern (used across all sections)

```css
.section-label {
  font-family: 'DM Sans', sans-serif;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--amber);
  margin-bottom: 12px;
}
```

### KM Logo SVG (nav size — 36×36)

```svg
<svg width="36" height="36" viewBox="0 0 36 36" fill="none">
  <circle cx="18" cy="18" r="17" stroke="#D4922A" stroke-width="0.6" opacity="0.5"/>
  <line x1="10" y1="9" x2="10" y2="27" stroke="#F5F0E8" stroke-width="2" stroke-linecap="round"/>
  <line x1="10" y1="18" x2="19" y2="9" stroke="#F5F0E8" stroke-width="1.8" stroke-linecap="round"/>
  <line x1="10" y1="18" x2="19" y2="27" stroke="#F5F0E8" stroke-width="1.8" stroke-linecap="round"/>
  <line x1="20" y1="9" x2="20" y2="27" stroke="#D4922A" stroke-width="2" stroke-linecap="round"/>
  <line x1="20" y1="9" x2="25" y2="18" stroke="#D4922A" stroke-width="1.8" stroke-linecap="round"/>
  <line x1="25" y1="18" x2="30" y2="9" stroke="#D4922A" stroke-width="1.8" stroke-linecap="round"/>
  <line x1="30" y1="9" x2="30" y2="27" stroke="#D4922A" stroke-width="2" stroke-linecap="round"/>
  <circle cx="10" cy="18" r="2.5" fill="#D4922A"/>
  <circle cx="25" cy="18" r="2" fill="#F5F0E8" opacity="0.6"/>
</svg>
```

- On dark backgrounds: K strokes = #F5F0E8, M strokes = #D4922A
- On light backgrounds: K strokes = #0D1B2A, M strokes = #D4922A

---

## LAYOUT ARCHITECTURE

### Overall page background

- Base: `background: var(--cream)` — warm cream throughout
- Dark sections (stats bar, credibility, contact): `background: var(--navy)`
- Mid sections (problem, testimonials): `background: var(--white)` or `background: var(--cream)`
- **No dark mode** — this is a permanently light site

### Section padding standard

```css
section { padding: 96px 72px; }
@media (max-width: 768px) { section { padding: 64px 24px; } }
```

### Watermark text pattern (James David style)

```css
.watermark {
  position: absolute;
  font-family: 'Cormorant Garamond', serif;
  font-weight: 700;
  font-size: clamp(80px, 14vw, 180px);
  color: var(--navy);
  opacity: 0.04;
  letter-spacing: -2px;
  white-space: nowrap;
  pointer-events: none;
  user-select: none;
  z-index: 0;
}
/* In hero: bottom: 0; left: 0; right: 0; text: "KRISHNA MOHAN" */
/* In footer: bottom: 0; centered; text: "KRISHNA MOHAN" */
```

---

## COMPONENT PATTERNS

### NAV

```
Position: fixed, top, full width
Height: 72px
Background: rgba(245,240,232,0.92) + backdrop-filter: blur(16px)
Border-bottom: 1px solid var(--cream-dark)
Left: KM logo mark + "Krishna Mohan" in Cormorant Garamond 18px/600
Center: Nav links in DM Sans 12px/400, letter-spacing: 1.5px, uppercase
Right: "Book a Call" button — navy bg, cream text, 3px border-radius
On scroll: border-bottom becomes more visible (opacity transition)
Mobile: hamburger menu, full-screen nav overlay
```

### HERO (James David pattern)

```
Layout: CSS Grid, 2 columns (55% left, 45% right) on desktop
Background: var(--cream)
Watermark: "KRISHNA MOHAN" large text, absolute, behind everything

LEFT COLUMN:
  - Section label: "AI Consultant · Educator · Practitioner" (amber caps)
  - H1: "Build AI Mindset to" + line break + "Grow Your Company"
    Font: Cormorant Garamond 700, size: clamp(44px, 6vw, 80px)
    Color: var(--navy)
  - Body: LinkedIn summary opening paragraph, DM Sans 300, 16px
  - CTAs: "Book a Conversation" (amber pill button) + "See How I Work →" (ghost)
  - Stats row: 30 yrs · 1000+ mentored · 250+ led (small, below CTAs)

RIGHT COLUMN:
  - Krishna's speaking photo (KM_Presenting_Cropped_2.jpg)
  - Image sits in a rounded rect container (border-radius: 20px)
  - Subtle amber overlay: mix-blend-mode or ::after overlay at 8% amber
  - Slight rotation or offset for visual interest (optional: -2deg)
  - Decorative amber square block behind/below photo (James David style)
```

### STATS BAR

```
Background: var(--navy)
Layout: flex row, 4 items, equal width, center-aligned
Each stat:
  - Number: Cormorant Garamond 700, 52px, var(--amber)
  - Label: DM Sans 300, 11px, letter-spacing: 2px, uppercase, --muted-dark
  - Separator: 1px vertical line, --border-dark, between items
Padding: 48px 72px
```

### PROBLEM SECTION

```
Background: var(--white)
Layout: 2 columns, 50/50
Left:
  - Label "About Me" in amber caps
  - H2: "Feeling Stuck or Overwhelmed" (navy) + "by AI?" (amber)
  - 3 pain point bullets with amber checkmark icons
  - CTA: "Let's Talk →" amber pill button
Right:
  - Headshot photo (KM_-_Copy.jpeg) in rounded container
  - 3 mini feature cards stacked: Growth Stuck / Wearing Hats / Lack Clarity
    (James David style — white cards, subtle shadow, small icon)
```

### SERVICE CARDS

```
Layout: 3-column grid, gap: 2px (flush grid — James David style)
Each card:
  Background: var(--white)
  Padding: 48px 40px
  Border: 1px solid var(--cream-dark)
  Hover: translateY(-6px), bottom border 3px amber appears
  
  Content:
  - Large ghost number (01, 02, 03) — Cormorant Garamond, 72px, opacity: 0.05
  - Service title: Cormorant Garamond 600, 26px, var(--navy)
  - Description: DM Sans 300, 14px, var(--text-body), line-height: 1.8
  - "Learn More →" link in amber
```

### CREDIBILITY BAR

```
Background: var(--navy)
Layout: flex row, centered
Label above: "Experience & Mentorship Across" in DM Sans caps, amber
Institutions as text-logo items:
  Thomson Reuters · Clarivate · Great Learning · MIT · Johns Hopkins · TAPMI
  Font: Cormorant Garamond 600, 18px, opacity: 0.4 default → 1.0 on hover
  Separated by thin vertical lines
```

### TESTIMONIAL CARDS

```
Background: var(--cream)
Layout: 3 cards in a row (or stacked on mobile)
Each card:
  Background: var(--white)
  Border-left: 3px solid var(--amber)
  Padding: 32px
  Quote text: Cormorant Garamond italic 400, 18px, var(--navy)
  Opening quote mark: " in amber, 64px, Cormorant Garamond, opacity 0.3
  Attribution: DM Sans 300, 11px, caps, letter-spacing: 2px, --text-muted
  Attribution text: "Great Learning — MIT/Johns Hopkins Program"
```

### BLOG CARDS

```
Layout: 1 large card left + 2 smaller cards stacked right (James David pattern)
Large card:
  - Full image top
  - Category tag (amber pill)
  - Title: Cormorant Garamond 600, 22px
  - Excerpt: DM Sans 300, 13px
  - "Read More →" in amber
Small cards:
  - Image left (100px wide) + text right
  - Same typography, smaller image
All cards link to LinkedIn Pulse articles
"View All on LinkedIn →" button below grid
```

### LINKEDIN FOLLOW BANNER

```
Background: warm amber gradient (from #F5F0E8 to #E8D5B0) — James David salmon style
Layout: 2 columns — photo left, text right
Photo: KM_-_Copy.jpeg, no border, natural
Text: "Follow for practical AI insights for Indian businesses — no hype, just what works."
  Font: Cormorant Garamond 700, 28px
CTA: "Follow on LinkedIn →" — navy pill button
```

### CONTACT SECTION

```
Background: var(--navy)
Layout: 2 columns — form left, info right
Form fields: navy-mid background, cream text, amber focus border
  Fields: Name, Email, Company Name, Enquiry Type (select), Message
  Submit: amber button "Send Message →"
Right column:
  Headline: "Let's Have That Conversation" (Cormorant, italic, cream)
  Sub: amber tagline
  Contact links: email, LinkedIn
  Small KM logo mark
```

### FOOTER

```
Background: var(--navy-mid)
Border-top: 1px solid --border-dark
Watermark: "KRISHNA MOHAN" large, low opacity, absolute at bottom
Layout: 3 rows
  Row 1: Logo left | Nav links center | Social icons right
  Row 2: Newsletter signup (email input + amber subscribe button)
  Row 3: © 2025 Krishna Mohan · Privacy Policy · All rights reserved
```

---

## ANIMATION RULES

Keep animations subtle and purposeful. No heavy motion.

```css
/* Scroll reveal — apply to all major sections */
.reveal {
  opacity: 0;
  transform: translateY(28px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}
.reveal.visible { opacity: 1; transform: translateY(0); }

/* Stagger children */
.reveal-child:nth-child(1) { transition-delay: 0s; }
.reveal-child:nth-child(2) { transition-delay: 0.1s; }
.reveal-child:nth-child(3) { transition-delay: 0.2s; }

/* Hero elements animate on load */
/* Use animation-delay: 0.2s, 0.4s, 0.6s, 0.8s for hero label, h1, sub, CTAs */

/* Intersection Observer for scroll reveals */
/* Trigger: threshold 0.15 */
```

Button hover states:

- Amber buttons: background → var(--amber-light), translateY(-2px)
- Ghost links: color opacity 0.4 → 1.0
- Cards: translateY(-6px) + amber bottom border appears

---

## PHOTO HANDLING

### Available photos

| File | Best use |
|---|---|
| `KM_Presenting_Cropped_2.jpg` | Hero (right column), Speaking section header |
| `KM_-_Copy.jpeg` | About/Problem section, LinkedIn follow banner |

### Photo treatment rules

- Never heavy filters or desaturation
- Subtle amber tint overlay acceptable: `rgba(212, 146, 42, 0.08)` via CSS ::after
- Round containers: `border-radius: 16px` on hero, `border-radius: 12px` on smaller
- Hero photo: allow slight visual crop on mobile to maintain face visibility
- Always include meaningful `alt` text

### Embedding photos in HTML

Since this is a single-file HTML, reference images with:

```html
<img src="KM_Presenting_Cropped_2.jpg" alt="Krishna Mohan presenting at an AI conference" ...>
```

When serving locally, place image files in same directory as HTML file.

---

## RESPONSIVE BREAKPOINTS

```css
/* Desktop: default styles (1200px+ base) */
/* Tablet */
@media (max-width: 1024px) {
  /* Reduce font sizes, compress spacing */
}
/* Mobile */
@media (max-width: 768px) {
  /* Stack all 2-col layouts to 1 col */
  /* Nav: hide links, show hamburger */
  /* Hero: photo moves below headline */
  /* Service cards: 1 per row */
  /* Stats: 2x2 grid */
  /* Blog: stack all cards */
}
```

---

## CONTENT RULES (what to write and how)

### Tone checker — before writing any copy ask

- Does it sound like a trusted expert, not a salesperson?
- Is it specific? (avoid "leverage", "synergy", "transform")
- Would an SMB founder in India understand and relate to it?
- Does it end with a clear action or insight?

### Approved phrases (from Krishna's own voice)

- "No hype, just what works"
- "Not as a pilot project that dies in a boardroom"
- "Cut through the noise"
- "The gap I work in"
- "AI to work as a real lever for growth"
- "Move from AI curiosity to AI execution"

### Avoid

- "AI transformation journey"
- "Leverage cutting-edge AI"
- "Future-proof your business"
- "Disruptive innovation"

---

## BUILD CHECKLIST

Before calling the website complete, verify:

- [ ] KM logo renders correctly in nav (dark bg version) and any light sections
- [ ] Hero watermark text is visible but subtle (opacity ~0.04)
- [ ] Both photos load and display with correct aspect ratios
- [ ] Amber color used ONLY for: labels, CTAs, numbers, accents — not body text
- [ ] All section labels are in uppercase DM Sans, amber, letter-spaced
- [ ] Testimonials include attribution
- [ ] Blog links point to correct LinkedIn Pulse URLs
- [ ] Contact form has all 5 fields + submit button
- [ ] Email `mailto:kmohan10@gmail.com` works in all CTA buttons
- [ ] LinkedIn URL `linkedin.com/in/krishnamohn` is correct everywhere
- [ ] Mobile layout: all 2-col grids stack to 1 col
- [ ] Nav CTA "Book a Call" is always visible
- [ ] Scroll reveal animations work via IntersectionObserver
- [ ] No dark mode applied — site stays light always
- [ ] Font minimum: 11px
- [ ] Page loads without console errors

---

## FUTURE ITERATIONS (v2+)

- Add Calendly embed replacing email CTA
- Ghost/Substack blog CMS integration
- Case studies / client stories page
- Resource library (AI guides for SMBs)
- Custom domain deployment guide (Netlify / Vercel)
- Language: consider Hindi version for broader SMB reach
- Add Open Graph meta tags for LinkedIn sharing preview
