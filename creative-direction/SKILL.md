---
name: creative-direction
description: >
  End-to-end creative direction for e-commerce projects. Platform-agnostic
  methodology (Shopify, Lightspeed, WooCommerce, etc.) with deep Shopify expertise.
  Covers brand research, theme analysis, brief generation, design system output
  (tokens.css + DESIGN.md), and dev handoff — all with a human Creative Director
  as the final approval gate. Use this skill whenever the user mentions creative
  briefs, brand direction, theme styling, design tokens, client intake,
  agency handoff, e-commerce visual identity, or any task involving translating
  a brand's identity into actionable design specifications for developers. Also
  trigger when the user says "run the pipeline", "new client project", "generate
  a brief", "CD review", or references the agent sequence (Intel → Theme → Brief
  → QA → CD approval). Even partial tasks — like "audit this brand" or "analyze
  this theme" — should trigger this skill for the relevant phase.
---

# Creative Direction for E-Commerce

A structured methodology for producing creative direction packages that give
developers clear, specific, actionable design specifications — eliminating
guesswork and interpretation from the build process.

## Philosophy

The product is never a website. It's a market position made tangible.

A theme is a constraint, not a creative sandbox. The constraint should
be the client's competitive reality, not the theme's defaults. Every creative
decision must be traceable to a strategic rationale — "it looks good" is not
direction, it's preference.

**Pricing philosophy:** Value-based, not hourly. Price what the agency avoids
(20–40 hours of design interpretation, revision cycles, brand drift) — not
what it costs to deliver (~5 hours). The deliverable replaces guesswork at
scale; that's what agencies pay for.

## When to Use This Skill

- New client project intake and brand research
- Theme analysis and constraint mapping (any e-commerce platform)
- Creative brief generation from research inputs
- Design system output: tokens.css, DESIGN.md
- QA review of brief against standards
- Dev handoff preparation (HTML page with interactive checklists)
- Post-launch creative retainer work
- Any request to "run the pipeline" or "start a project"

## The Pipeline

Five agents run in sequence. Each has one job, a clear input, a clear output
format, and a quality benchmark. The Creative Director (human) is the final
approval gate — never bypassed.

```
Service Agreement → Client Intake → Intel Agent → Theme Analyst → Brief Generator → QA Agent → CD Review → Deploy
```

**First-time agency onboarding:** Before any creative work, generate a pre-filled
service agreement (EN or FR) using `python scripts/generate-agreement-pdf.py [en|fr|both]`.
The PDF includes branded styling, terracotta placeholder fields, and signature areas.
Agreement defines scope, revision limits, and payment terms upfront.

### Pipeline Rules

1. **Never skip phases.** Even for "simple" projects.
2. **Structured JSON between agents.** No free-text handoffs.
3. **Standards are non-negotiable.** Agents flag violations, never override.
4. **Conflicts surface, never resolve silently.** If intake and brand audit
   conflict, escalate to CD — don't pick a side.
5. **Every correction becomes a standards entry.** The system learns from
   every project through the training loop.
6. **CD tone pass before client delivery.** Pipeline agents write analytically;
   client-facing deliverables need warm, diplomatic language. Remove critical
   framing, unverified statistics, and internal jargon (e.g., "T2 violation")
   before publishing. The brief should make clients feel good about their
   brand while still being honest about the direction.
7. **Theme Package = validated starting points, not production code.** Generated
   Liquid files are positioned as starting points for dev teams. They import and
   customize — we don't debug their implementation. Revision rounds cover creative
   adjustments (palette, typography, layout), not code issues.

## Agent Sequence

Load the relevant reference file for the agent you're executing:

| Agent | Reference | Trigger |
|-------|-----------|---------|
| Intel Agent | [references/intel-agent.md](references/intel-agent.md) | Brand research, competitor analysis, audience signals |
| Theme Analyst | [references/theme-analyst.md](references/theme-analyst.md) | E-commerce theme constraint mapping |
| Brief Generator | [references/brief-generator.md](references/brief-generator.md) | Assembling the creative brief |
| QA Agent | [references/qa-agent.md](references/qa-agent.md) | Scoring and flagging the brief |
| Implementation Generator | [references/implementation-generator.md](references/implementation-generator.md) | Shopify Liquid code generation + preview page generation (post-approval) |
| Orchestrator | [references/orchestrator.md](references/orchestrator.md) | Full pipeline coordination |

## Deliverable Stack

Every project produces these artifacts:

1. **Brand Audit** — internal reference document from Intel Agent
2. **Creative Brief** — HTML page hosted at `creative.pascalchampagne.com/[slug]/`
3. **tokens.css** — CSS custom properties mapped to brief decisions (theme-agnostic)
4. **DESIGN.md** — machine-readable design tokens for Claude Code / Cursor / AI agents
5. **Dev Handoff (dev-handoff.html)** — styled HTML implementation guide with interactive
   checklists, progress tracking, download links for tokens.css and DESIGN.md. Uses the
   project's color palette. Hosted at `creative.pascalchampagne.com/[slug]/dev`
6. **Walkthrough Video** — 3–4 min Loom, CD narrates creative decisions to dev team
7. **Theme Package** (Shopify projects only) — validated Liquid section files (.liquid),
   locales (FR + EN), and an implementation guide. Generated by the Implementation
   Generator agent (Agent 6) after CD approval. Each file passes Shopify's theme-check
   validation. Positioned as "validated starting points" — dev teams import and customize,
   not deploy as-is. Saves 10–20 hours of design-to-code translation per project.
   Non-Shopify projects receive the specification stack (items 1–6) without code generation.
   Delivered as a downloadable ZIP on the Dev Handoff page, containing `sections/`,
   `locales/`, and a `README.md` with install instructions.
8. **Theme Preview** — client-facing HTML page rendering all generated sections with real
   CSS, placeholder content, and project typography. No dev chrome (no section labels,
   scheme badges, or filenames). Used for client approval/revision cycle.

### Four-Link Delivery Model

Each project shares exactly 4 links with the agency:

1. **Creative Brief** (client-facing) — `creative.pascalchampagne.com/[slug]/brief/`
2. **Theme Preview** (client-facing) — `creative.pascalchampagne.com/[slug]/preview/`
3. **Dev Handoff** (dev-facing, with DESIGN.md + tokens.css + Theme Package downloads) — `creative.pascalchampagne.com/[slug]/dev/`
4. **Dev Feedback Form** — `creative.pascalchampagne.com/feedback/`

Clean audience separation: client sees the brief and preview, dev team gets the handoff + assets,
feedback form closes the loop. Theme Package files are downloadable from the Dev Handoff page.

### Gate Page Structure

Every project gets a password-protected gate page at `creative.pascalchampagne.com/[slug]/`.
After authentication, users see a three-button landing:

1. **View Creative Brief** (primary button) — links to `brief/`
2. **View Dev Handoff** (secondary button) — links to `dev/`
3. **View Theme Preview** (secondary button) — links to `preview/`

Auth uses `sessionStorage` (simple hash gate — not security-critical, prevents casual browsing).
All sub-pages check `sessionStorage` and redirect to gate if not authenticated.
Gate UX: clicking a button before authenticating shows the password form, then redirects
directly to the chosen page after password entry (no double-click). The `pendingDest`
pattern stores the intended destination before auth.

### Cross-Linking Between Deliverables

Each deliverable links to the others so the recipient always has access to the full package:

- **Creative Brief** nav bar includes "Theme Preview →" link (opens new tab, primary color)
- **Dev Handoff** files section includes a "View Theme Preview" card alongside the
  Creative Brief and download cards
- **Theme Preview** top bar includes "Creative Brief" link (opens new tab)

### Creative Brief Structure (v2)

The client-facing brief HTML page follows this section order:

1. **Cover** — brand name, subtitle, meta (CD, agency, theme, date, version)
2. **01 Brand Position** — mood statement, "is/is not" columns, context metrics
3. **02 Color Palette** — swatches with hex, role, and rationale + palette rules
4. **03 Typography** — heading + body specimens with type scale
5. **04 Tone of Voice** — personality adjectives, do/don't examples with rationale
6. **05 Page Structure** — full-page wireframe (SVG, click-to-enlarge lightbox) in
   two-column layout with section-by-section descriptions + Dawn section types
7. **06 Section Direction** — each section gets a card with:
   - Mini SVG wireframe (brand-integrated style, using project palette)
   - Structured key-value fields: Layout, Headline, Subtext, CTA, Key rules
   - Implementation note (Dawn section type + scheme)
   - Priority badge (High/Medium/Low)
   - Dark-preview variant for Scheme 2 sections (with adequate text contrast)
8. **07 Photography** — background treatment, style reference, asset guidance
9. **08 Interactive Elements** — 2-row × 3-column grid:
   Row 1: Primary CTA, Secondary CTA, Product Card Hover (live demos + spec tables)
   Row 2: Text Link, Form Input, Rationale (design logic summary)
10. **09 Theme Constraints** — native vs. custom CSS breakdown with effort estimates
11. **10 Deliverables** — explains tokens.css, DESIGN.md, and dev-handoff.html with
    usage instructions (where to place, how AI tools read them)
12. **11 Decisions to Confirm** — numbered items with context + question format.
    Warm, explanatory tone. No jargon. Replaces the old QA Appendix.

**Important:** QA scores, standards compliance checklists, and anti-pattern checks
are internal tools — they stay in the pipeline JSON files (`04-qa-report.json`),
never shown to clients. The "Decisions to Confirm" section replaces the old
"CD Attention Items" with client-friendly language.

## Design Standards (Global)

These rules apply to EVERY project. They are non-negotiable and must be checked
by the QA Agent before any brief is approved.

### Typography

- Never use more than 2 typeface families per project.
- Heading and body fonts must have a clear visual contrast — weight alone is
  not enough differentiation.
- System fonts and generic sans-serifs (Arial, Helvetica) are never acceptable
  as a creative choice. They are fallbacks only.
- Every font choice must include a one-sentence rationale tied to brand
  personality — "clean and modern" is not a rationale.
- Font loading: specify Google Fonts or Adobe Fonts embed. Include weight
  variants needed. Never load more than 4 weights total.

### Color

- Every palette must include: dominant background, surface/card, primary brand
  accent, secondary accent, body text, and border/divider.
- Each color gets a hex value AND a one-sentence rationale.
- Never specify a color without testing it against the client's existing
  photography. The photography IS the brand.
- Pure black (#000000) and pure white (#FFFFFF) are never used — always tint
  toward the brand hue.
- Check every palette against the anti-patterns log before approving.

### Layout & Spacing

- Minimum section padding: 60px top and bottom for premium feel.
- Use a spacing scale (8, 16, 32, 64, 96px) — never arbitrary values.
- Product image background treatment must be specified: white/transparent,
  lifestyle, or textured. This is a brand decision.

### Tone of Voice

- Every brief includes: 3 personality adjectives, one headline the brand WOULD
  write, one headline the brand would NEVER write, and a one-sentence rationale
  explaining the difference.
- Copy tone must feel like it was written by a brand that looks this way.

### E-Commerce Specific

- "Add to cart" must be the most visually prominent interactive element on any
  product page. Brief must specify its color and weight.
- For any retail brand with a physical location, reference the store's physical
  aesthetic as a visual anchor for the digital experience.
- For brands under $500K annual revenue, avoid aspirational luxury positioning
  unless it's genuinely their market. Authenticity over aspiration.

## Anti-Patterns

These are patterns that have been explicitly identified as problems. The QA Agent
checks every brief against this list.

- **The Shopify Default** — Using Dawn's default palette with minor tweaks.
  Every project must establish its own visual identity.
- **The Purple Gradient** — Purple-to-blue gradients on white backgrounds.
  Overused in AI-generated design. Never.
- **The Inter Everything** — Inter, Roboto, or system fonts as a "design choice."
  These are defaults, not decisions.
- **The Generic Hero** — Full-bleed lifestyle image + white text overlay +
  "Shop Now" button. If every competitor's site looks like this, so does yours.
- **The Color Soup** — More than 5 colors in the palette with no clear hierarchy.
  Dominant + accent outperforms distributed palettes.
- **The Vague Brief** — "Clean and modern" / "minimal but impactful" / "elevated
  feel." These mean nothing. Every direction must be specific enough that two
  different developers would make the same visual choice.

## tokens.css Structure

The design tokens file maps creative brief decisions to CSS custom properties.
For Shopify, it's dropped into `theme/assets/` and referenced in `theme.liquid`.
For other platforms, adapt the integration method accordingly.

```css
:root {
  /* ── PALETTE ── */
  --color-base-background-1: ;  /* page background */
  --color-base-background-2: ;  /* cards, sections */
  --color-base-accent-1: ;      /* primary CTA, links */
  --color-base-accent-2: ;      /* secondary accent */
  --color-base-text: ;          /* body text */
  --color-base-border: ;        /* dividers, outlines */

  /* ── TYPOGRAPHY ── */
  --font-heading-family: ;
  --font-heading-weight: ;
  --font-body-family: ;
  --font-body-weight: ;

  /* ── SPACING ── */
  --spacing-xs: 8px;
  --spacing-sm: 16px;
  --spacing-md: 32px;
  --spacing-lg: 64px;
  --spacing-xl: 96px;

  /* ── SECTIONS ── */
  --section-padding-top: 60px;
  --section-padding-bottom: 60px;
}
```

Every value must trace back to a specific brief decision with rationale.

## DESIGN.md Structure

The machine-readable design document that AI coding agents (Claude Code, Cursor,
Windsurf) read automatically when placed in a project root.

Required sections:
- **How to Use This File** — plain-language instructions for three audiences:
  developers using AI tools, developers without AI tools, and project managers.
  Must be the first section after the header. Written in clear, non-technical
  language so anyone opening the file knows exactly what it's for.
- **Brand Position** — 3 sentences establishing the creative stance, plus a
  "What this means for code" one-liner that translates brand into implementation.
- **Palette** — table with role, name, hex, RGB, usage, and rationale per color.
  Include Color Rules sub-section.
- **Typography** — families, weights, providers, scale, rationale. Include
  Typography Rules sub-section.
- **Tone** — personality adjectives, do/don't examples with rationale.
- **Component Notes** — section-by-section styling guidance. Include plain-language
  explanations for any entry that assumes theme familiarity (e.g., why "Image with
  text" instead of "Image banner", what "dynamic checkout buttons" are).
- **Constraints** — what the Shopify theme prevents or limits. Each constraint
  includes a technical description followed by an italicized *In plain language:*
  explanation written for developers who have never used the theme before.
- **Anti-Patterns** — project-specific things to avoid, with brand-specific
  rationale (not generic best practice).

## Dev Handoff Structure

The step-by-step implementation guide for the dev team. Generated as a styled
HTML page (dev-handoff.html) — not Markdown. Uses the project's color palette,
includes interactive checklists with progress tracking, and provides download
links for tokens.css and DESIGN.md. Must be usable by a junior developer who
has never worked with the theme before.

Required sections:
- **Quick Glossary** — defines all theme-specific terms used in the document
  (e.g., Dawn, Scheme, tokens.css, Section, "Dawn native", dynamic checkout
  buttons, DESIGN.md). Placed at the very top, before any instructions.
- **What You're Building** — 2-3 sentences about the project and links to the
  other deliverables (brief page, tokens.css, DESIGN.md).
- **Files** — download cards in this order: Theme Package ZIP (highlighted, accent
  border — most prominent), tokens.css, DESIGN.md, "View Creative Brief" link,
  and "View Theme Preview" link. All external links open in new tabs via
  `target="_blank"`. The Theme Package card is followed by a "Quick Install"
  guide (numbered steps: unzip, copy sections, merge locales, add via Theme
  Editor, upload tokens.css).
- **Quick Start Checklist** — numbered steps: Theme Setup, Fonts, Color Schemes,
  Homepage Sections (table with order, section type, scheme, padding),
  Section-Specific Settings, Product Page Template.
- **Section-by-Section** — implementation details for each homepage section. Each
  section card includes a **mini SVG wireframe thumbnail** on the left (matching
  the brief's visual style, using project palette colors) and settings/config
  details on the right. Dark cards for Scheme 2 sections.
- **Product Pages** — Collection page (grid, sorting, editorial banners) and PDP
  (layout, price styling, CTA, collapsible content). Each with wireframe
  thumbnails matching the homepage section cards.
- **Custom CSS Notes** — table explaining each tokens.css override (what it does,
  how many lines, why the theme can't do it natively).
- **What NOT to Do** — each rule includes a bold directive followed by a
  plain-language *Why:* explanation. The "why" should be understandable by a
  developer who has no design background — explain what would go wrong visually
  or brand-wise if they broke the rule.
- **Assets Needed from Client** — checklist of files the dev team needs.
- **Questions for Client** — open items needing client answers before build.
- **Timeline Recommendation** — phase/scope/estimate table.

## Session Protocol

When working in Claude Code, follow the session protocol:

1. **On session start:** Read CLAUDE.md → read session-protocol.md → orient.
2. **During session:** Work toward the stated goal. Flag decisions for CD review.
3. **On session end:**
   - Update session-protocol.md (last accomplished, next goal, open decisions)
   - Update CURRENT_STATE.md
   - Add decisions to DECISIONS.md
   - Git commit with descriptive message
   - Confirm to CD: what's in the commit, what's next

## Standards Library

The standards library is the system's compounding asset. It starts with ~20 global
rules and grows with every project through the training loop.

- **Global standards** (`standards/global.md`) — non-negotiable rules checked on every project
- **Anti-patterns** (`standards/anti-patterns.md`) — known bad patterns flagged by QA
- **Industry profiles** (`standards/industries/`) — category-specific rules layered on top

Standards never get deleted — only marked superseded with rationale. The library
is what makes brief #50 dramatically better than brief #1 without Pascal spending
more time. It's also a deliverable: Retainer and Partner tiers include access to
a project-specific or custom standards library.

## Training Loop

Every CD correction becomes a permanent learning:

1. CD reviews brief, makes corrections in `brief-final.md`
2. System captures diff between `brief-original.md` and `brief-final.md`
3. CD explains the "why" for each correction
4. Corrections are categorized and added to `standards/global.md` or
   `standards/anti-patterns.md`
5. Future QA Agent runs check against the expanded standards

The system gets better with every project. Standards compound.

## Pricing Reference (CAD)

| Tier | Price | Scope |
|------|-------|-------|
| Brief Sprint | $1,500/project | 1 brief, 1 revision round, 48h delivery |
| Studio Retainer | $3,500/month | Up to 3 briefs, 2 revision rounds/brief, 24h delivery, monthly strategy call, standards library |
| Brand Partner | $6,500/month | Up to 8 briefs, 2 revision rounds/brief, client onboarding calls, quarterly brand strategy, custom standards library, white-label, exclusive territory |

**Pricing methodology:** Value-based. The Sprint replaces 20–40 hours of agency
design interpretation time. Internal add-on rates and negotiation floors are
documented separately in `plan/pricing-reference.md` — never shown publicly.
Package names ("Brief Sprint", "Studio Retainer", "Brand Partner") stay in
English even in French communications — they function as brand names.

## Client Intake & Forms

All forms are **self-hosted branded HTML** on Netlify with Netlify Forms backend.
No third-party form tools (Tally, Typeform, etc.) — consistent brand experience
on the same domain as deliverables.

- **Intake form** (`creative.pascalchampagne.com/intake/`) — 5-section form
  covering brand basics, visual direction, audience, content/assets, and project
  scope. Generated per-project when possible: pre-fills known data from prior
  email exchange, only asks for what's missing. A generic "cold start" version
  exists for direct discovery leads.
- **Feedback form** (`creative.pascalchampagne.com/feedback/`) — 3-question dev
  feedback form sent with delivery. If no response within follow-up window,
  send a reminder via agency contact.
- **Contact form** — inline on the landing page with tier pre-selection from
  pricing cards. Pre-qualifies leads before first exchange.

### Language Handling

Per-project language: Pascal specifies EN, FR, or both per project. All forms
and deliverables support EN/FR toggle (using `data-fr` attributes + JS swap).
Language persists via `localStorage` across form → thank-you page redirects.

**French terminology decisions:**
- "Référentiel de standards" (not "librairie" or "bibliothèque")
- "Transfert" for handoff in descriptions, but "Dev Handoff" stays as product name
- "Appel stratégique" (not "appel stratégie")
- Package names stay English in French version

## Industry Profiles

Load the relevant industry profile from `standards/industries/` when available:

- Fashion & apparel
- Food & beverage
- Wellness & beauty
- Music & entertainment
- Sporting goods
- General retail

Each profile adds category-specific rules on top of the global standards.

## File Architecture

```
cd-system/
├── CLAUDE.md              # Session orientation (read first)
├── session-protocol.md    # Working memory between sessions
├── CURRENT_STATE.md       # Current phase and status
├── DECISIONS.md           # Decision log
├── plan/                  # Phased action plans
├── standards/             # Living standards library
│   ├── global.md          # Non-negotiable rules
│   ├── anti-patterns.md   # Known bad patterns
│   └── industries/        # Category-specific profiles
├── prompts/               # Agent system prompts (versioned)
├── site/                  # Netlify deploy root
│   └── [project-slug]/    # Per-project deploy
│       ├── index.html     # Gate page (password + 3-button landing)
│       ├── brief/         # Creative brief (HTML)
│       ├── dev/           # Dev handoff (HTML)
│       ├── preview/       # Theme preview (HTML, Shopify only)
│       └── assets/        # DESIGN.md, tokens.css
├── comms/                 # Communication templates
├── contracts/             # Service agreement
├── templates/             # tokens.css, DESIGN.md, brief-page.html
├── briefs/                # Per-project output folders
└── scripts/               # Automation (deploy, training loop, etc.)
```

## Key Principle

Standards never get deleted — only marked superseded with rationale.
Prompts are version-controlled — every change committed with reasoning.
The system evolves. It never regresses.
