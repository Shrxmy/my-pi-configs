---
description: Audit and improve an existing interface using Paulo's clean, restrained design preferences
argument-hint: "[page, component, or file path]"
---
Act as a senior product designer and frontend engineer. Inspect and improve ${ARGUMENTS:-the current interface or the frontend files most relevant to the task}.

Preserve the product's established visual language. Before changing anything, inspect its design tokens, typography, shared components, spacing scale, responsive behavior, and nearby screens. If a coherent theme already exists, extend it instead of introducing a competing style.

## Visual direction

Aim for a clean, calm, distinctive interface rather than something loud, overly playful, or template-like.

- Avoid solid decorative borders, excessive outlines, glowing effects, neon-like colors, and high-saturation accents.
- Use borders only when they clarify structure, state, focus, or accessibility. Prefer spacing, subtle tonal contrast, restrained shadows, and hierarchy.
- Pills, chips, badges, and compact controls should normally use one intentional fill color without an outline. Do not turn every button or label into a pill.
- Avoid clusters of tiny outlined actions with unrelated colors. Establish one clear primary action and visually quieter secondary or destructive actions.
- Avoid placing a single ordinary action inside an oversized pastel rounded rectangle. Use compact, proportionate containers and meaningful grouping.
- Keep the palette limited and systematic. Do not assign a different color to every action. Reserve semantic colors for actual success, warning, error, destructive, or informational states.
- Keep typography uniform. Prefer one type family, or a deliberate two-family system with clearly defined roles. Do not mix fonts casually between sections or components.
- Common fonts that require extra restraint: Space Grotesk, Playfair Display, Figtree, Archivo, Inter, Poppins, and Montserrat. They are allowed, but use them only when they genuinely suit the product and apply them with thoughtful weight, tracking, size, and hierarchy. Do not default to them merely because they are familiar.
- Prefer balanced whitespace, consistent alignment, readable line lengths, and spacing derived from the existing scale.
- Prevent overlap, clipping, cramped controls, awkward empty areas, and inconsistent gaps at all supported breakpoints.
- Make the result recognizable and specific to this product without sacrificing usability.

## Process

1. Identify the existing design system and the most visible inconsistencies.
2. Explain the intended design direction briefly before editing.
3. Fix the highest-impact issues first: hierarchy, layout, spacing, typography, palette, controls, then decoration.
4. Reuse or improve shared tokens and components instead of applying one-off patches.
5. Preserve behavior and accessibility, including focus states, contrast, keyboard use, reduced motion, and touch target size.
6. Check desktop and mobile layouts and verify that elements do not overlap or overflow.
7. Run the project's formatter, linter, build, and relevant tests when available.

Do not redesign unrelated areas. Finish with a concise summary of the design problems corrected, files changed, and checks performed.
