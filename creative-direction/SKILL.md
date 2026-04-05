---
name: creative-direction
description: >
  End-to-end creative direction for e-commerce projects, specializing in Shopify.
  Covers brand research, theme analysis, brief generation, design system output
  (tokens.css + DESIGN.md), and dev handoff — all with a human Creative Director
  as the final approval gate. Use this skill whenever the user mentions creative
  briefs, brand direction, Shopify theme styling, design tokens, client intake,
  agency handoff, e-commerce visual identity, or any task involving translating
  a brand's identity into actionable design specifications for developers. Also
  trigger when the user says "run the pipeline", "new client project", "generate
  a brief", "CD review", or references the agent sequence (Intel → Theme → Brief
  → QA → CD approval). Even partial tasks — like "audit this brand" or "analyze
  this Shopify theme" — should trigger this skill for the relevant phase.
---

# Creative Direction for E-Commerce

A structured methodology for producing creative direction packages that give
developers clear, specific, actionable design specifications — eliminating
guesswork and interpretation from the build process.

## Philosophy

The product is never a website. It's a market position made tangible.

A Shopify theme is a constraint, not a creative sandbox. The constraint should
be the client's competitive reality, not the theme's defaults. Every creative
decision must be traceable to a strategic rationale — "it looks good" is not
direction, it's preference.

## When to Use This Skill

- New client project intake and brand research
- Shopify theme analysis and constraint mapping
- Creative brief generation from research inputs
- Design system output: tokens.css, DESIGN.md
- QA review of brief against standards
- Dev handoff preparation (Notion page, walkthrough)
- Post-launch creative retainer work
- Any request to "run the pipeline" or "start a project"

## The Pipeline

Five agents run in sequence. Each has one job, a clear input, a clear output
format, and a quality benchmark. The Creative Director (human) is the final
approval gate — never bypassed.

```
Client Intake → Intel Agent → Theme Analyst → Brief Generator → QA Agent → CD Review → Deploy
```

### Pipeline Rules

1. **Never skip phases.** Even for "simple" projects.
2. **Structured JSON between agents.** No free-text handoffs.
3. **Standards are non-negotiable.** Agents flag violations, never override.
4. **Conflicts surface, never resolve silently.** If intake and brand audit
   conflict, escalate to CD — don't pick a side.
5. **Every correction becomes a standards entry.** The system learns from
   every project through the training loop.

## Agent Sequence

Load the relevant reference file for the agent you're executing:

| Agent | Reference | Trigger |
|-------|-----------|---------|
| Intel Agent | [references/intel-agent.md](references/intel-agent.md) | Brand research, competitor analysis, audience signals |
| Theme Analyst | [references/theme-analyst.md](references/theme-analyst.md) | Shopify theme constraint mapping |
| Brief Generator | [references/brief-generator.md](references/brief-generator.md) | Assembling the creative brief |
| QA Agent | [references/qa-agent.md](references/qa-agent.md) | Scoring and flagging the brief |
| Orchestrator | [references/orchestrator.md](references/orchestrator.md) | Full pipeline coordination |

## Deliverable Stack

Every project produces these six artifacts:

1. **Brand Audit** — internal reference document from Intel Agent
2. **Creative Brief** — HTML page hosted at `brief.pascalchampagne.com/[slug]`
3. **tokens.css** — Shopify CSS custom properties mapped to brief decisions
4. **DESIGN.md** — machine-readable design tokens for Claude Code / Cursor / AI agents
5. **Notion Dev Handoff** — implementation notes, asset specs, theme-specific quirks
6. **Walkthrough Video** — 7–10 min, CD narrates creative decisions to dev team

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

The design tokens file maps creative brief decisions to Shopify CSS custom
properties. It is dropped into `theme/assets/` and referenced in `theme.liquid`.

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
- **Brand Position** — 3 sentences establishing the creative stance
- **Palette** — role, hex, rationale for each color
- **Typography** — families, weights, providers, scale, rationale
- **Tone** — personality adjectives, do/don't examples
- **Component Notes** — section-by-section styling guidance
- **Constraints** — what the Shopify theme prevents or limits
- **Anti-Patterns** — project-specific things to avoid

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

| Tier | Price | Includes |
|------|-------|----------|
| Brief Sprint | $950 | One-time creative direction package |
| Studio Retainer | $3,200/mo | Ongoing CD + monthly optimization |
| Brand Partner | $6,500/mo | Full strategic partnership |

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
