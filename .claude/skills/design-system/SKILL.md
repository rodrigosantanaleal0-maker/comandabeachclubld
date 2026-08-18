---
name: design-system
description: Comanda Beach design system — exact color/spacing/shadow tokens, component patterns, and breakpoints. Load before styling or adding any section, card, button, or visual element on this landing page, so nothing invents a new color or one-off style.
---

# Comanda Beach — Design System

This is the single source of truth for visual decisions on this landing page. Before adding or editing any CSS, reuse what's below instead of inventing new values. If a genuinely new pattern is needed, add it as a named token in `:root` (light) **and** update both dark-mode blocks (`@media(prefers-color-scheme:dark)` and `:root[data-theme="dark"]`) — never as an inline one-off value.

## Explicit rules
- **Green is the primary identity color** — it dominates every section's visual weight.
- **Orange is reserved for conversion/CTA** — primary buttons, key emphasized words, small indicators/badges. Never as a large background fill.
- **Hero must keep the real product screenshot** as its dominant visual element, not a generic illustration.
- Must work responsively across desktop, tablet, and mobile.
- Microinteractions must stay consistent — same easing curve, same reveal pattern — not a new animation invented per section.
- Never create a new color or one-off style for a single section. Extend the token system instead.

## Color tokens (`:root`)
| Token | Value | Use |
|---|---|---|
| `--paper` | `#F7FAF8` | page background (light) |
| `--surface` | `#FFFFFF` | card/panel background |
| `--surface-tint` | `#EAF6F3` | subtle tinted panel background |
| `--ink` | `#006B63` | headings / strong text (green-based, not black) |
| `--ink-strong` | `#004840` | strongest heading text |
| `--body` | `#4A6B66` | body text |
| `--muted` | `#7C948F` | secondary/muted text |
| `--teal` | `#00B8A9` | **verde principal** — primary identity color |
| `--teal-strong` | `#006B63` | **verde profundo** — deep green |
| `--gold` | `#FF5A1F` | **laranja CTA** — primary button / conversion |
| `--gold-strong` | `#FF7A3D` | **laranja destaque** — accent emphasis |
| `--sand` / `--sand-deep` | `#FF7A3D` / `#FF5A1F` | legacy aliases of gold tokens, same values |
| `--charcoal` / `--deep` | `#04302B` | darkest tone, dark-mode surfaces |
| `--border` | `rgba(0,107,99,.14)` | hairline borders (teal-tinted, not gray) |
| `--border-soft` | `rgba(0,107,99,.08)` | subtler border |
| `--focus-ring` | `0 0 0 3px rgba(255,90,31,.45)` | orange focus ring |

Full dark-mode remap already exists for every token above, both via `@media(prefers-color-scheme:dark)` (guarded `:root:not([data-theme="light"])`) and `:root[data-theme="dark"]`. Any new color token needs both.

## Scale tokens
- **Radius**: `--radius-sm:10px`, `--radius-md:16px`, `--radius-lg:24px`, `--radius-pill:999px`. (The hero `.device` uses `28px` as a deliberate one-off "premium" exception — don't propagate that elsewhere.)
- **Shadow**: `--shadow-sm` (card lift), `--shadow-md` (hover/floating elements), `--shadow-lg` (hero-level prominence) — always teal-tinted (`rgba(0,107,99,...)`), never pure black.
- **Motion**: `--ease:cubic-bezier(.2,.7,.2,1)` is the one easing curve for everything. The standard scroll-entrance microinteraction is the `.reveal`/`.reveal.is-visible` pattern (opacity + translateY(18px), 640ms), driven by an IntersectionObserver, respecting `prefers-reduced-motion`.
- **Fonts**: `--font-display: 'Plus Jakarta Sans'` for headings, `--font-body: 'Inter'` for body text.

## Breakpoints in use
`480, 640, 760, 860, 900, 920, 980, 1080` (mixed min-/max-width). This spread is already a minor inconsistency — reuse the closest existing value rather than adding a new arbitrary one (e.g. don't add `850px` or `990px`).

## Established component patterns — reuse, don't reinvent
- `.badge` — pill shape, small dot + uppercase label, teal.
- `.btn-primary` (filled gold/orange) / `.btn-ghost` (outline) — the only two button styles on the site.
- `.problem-card`, `.order-card`, `.table-chip`, testimonial cards — bordered `--surface` cards, `--radius-md`, `--shadow-sm`.
- `.float-card` — small floating badge (icon + two-line label) used around the hero device. Icon background is teal-tinted by default; exactly one instance (`.float-2 .ico`) is recolored gold as the "one orange accent among green" pattern — keep that ratio (mostly green, orange sparingly) for any future decorative accents.
- `.device` (hero product-screenshot mockup) — the "premium SaaS hero" recipe established 2026-08-18: deep layered shadow, ~28px radius, static 3D tilt (`perspective()+rotateY+rotateX`), continuous subtle float keyframe (`device-float`, translateY ±9px, 6.5s loop), pulsing teal glow behind (`.hero-visual::before` / `glow-pulse`), fade+blur entrance (`device-fade-in`). Reuse this exact recipe for any future "feature screenshot" callout instead of inventing a new mockup style.
- `.hero-accent-orange` / `.hero-accent-teal` — tiny decorative dot/ring accents placed near a hero visual.
- FAQ/objections use native `<details>/<summary>` accordions, not custom JS.

## Deployment
This repo auto-deploys to Vercel (production) on every push to `main` (GitHub: `rodrigosantanaleal0-maker/comandabeachclubld`, live at `comanda-beach-landing.vercel.app`). Keep this file and the actual `index.html` `:root` tokens in sync — if tokens change, update this file in the same commit.
