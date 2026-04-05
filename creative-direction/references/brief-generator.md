# Brief Generator

The creative core of the pipeline. Synthesizes research inputs into a structured
creative brief that gives developers clear, specific, actionable direction.

*Refine this prompt after every 3 projects.*

## Input

- `brand_audit_json`: from Intel Agent
- `theme_analysis_json`: from Theme Analyst
- `intake_json`: full client intake form answers
- `standards_global`: global rules (non-negotiable)
- `industry_profile`: category-specific rules (if available)
- `anti_patterns`: patterns to explicitly avoid

## Output (JSON)

```json
{
  "project_id": "client-slug-YYYY-MM",
  "mood_direction": "3 sentences that establish the creative position. Not vague — specific enough that two different developers would make the same visual choice.",
  "palette": [
    {
      "role": "dominant | primary | accent | text | border | surface",
      "hex": "#000000",
      "rationale": "one sentence — why this color for THIS brand, not just 'it looks good'"
    }
  ],
  "typography": {
    "heading": {
      "family": "font name",
      "weight": "500",
      "provider": "Google Fonts | Adobe Fonts | system",
      "rationale": "one sentence connecting font personality to brand personality"
    },
    "body": {
      "family": "font name",
      "weight": "400",
      "provider": "Google Fonts | Adobe Fonts | system",
      "rationale": "one sentence"
    }
  },
  "tone_of_voice": {
    "descriptor": "3 adjectives",
    "do": "example headline this brand WOULD write",
    "dont": "example headline this brand would NEVER write",
    "rationale": "one sentence on what separates the two"
  },
  "section_notes": [
    {
      "section": "section name from theme analysis",
      "direction": "specific creative direction for this section",
      "priority": "high | medium | low"
    }
  ],
  "photography_guidance": {
    "product_background": "white | transparent | lifestyle | textured",
    "style_reference": "description of the photographic aesthetic to aim for",
    "existing_assets_usable": true
  },
  "cta_specification": {
    "primary_color": "#hex",
    "text_style": "uppercase | sentence case | lowercase",
    "weight": "bold | semibold | medium",
    "rationale": "why this treatment for the primary action"
  }
}
```

## Process

1. **Read the brand audit first.** Understand who this brand actually is before
   deciding who they should become.
2. **Cross-reference with intake answers.** Where the client's aspirations and
   their current reality diverge, that's the creative opportunity.
3. **Check theme constraints.** Every recommendation must be implementable within
   the analyzed theme's actual controls. If it requires custom code, flag it.
4. **Apply global standards.** Every rule in standards_global is checked. Every
   color against anti-patterns. Every font against anti-patterns.
5. **Apply industry profile.** Layer category-specific rules on top.
6. **Write the mood direction LAST.** After all decisions are made, synthesize
   the creative position. If you can't describe it in 3 specific sentences,
   the decisions aren't coherent yet — revisit.

## Non-Negotiable Rules

- Apply every rule in standards_global. Flag violations — never override.
- Check every palette color against anti_patterns.
- Check every typography choice against anti_patterns.
- If the intake form and brand audit conflict, surface the conflict explicitly.
  Never silently resolve it — that's a CD decision.
- Every color gets a rationale. "It looks good" is not a rationale.
- Every font gets a rationale tied to brand personality, not aesthetic preference.
- Mood direction must be specific enough that two different developers reading
  it would make the same visual choices. If it's ambiguous, it fails.

## Escalation Triggers

Flag for CD review if:
- Brand audit and client intake contradict on fundamental positioning
- Photography quality constrains the creative direction
- Theme limitations require compromising more than 2 brief specifications
- Multiple equally valid creative directions exist — CD picks the one
- Anti-pattern is the client's explicit request (e.g., they want the purple gradient)
