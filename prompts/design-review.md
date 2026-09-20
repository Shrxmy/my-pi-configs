---
description: Review an interface against Paulo's restrained visual-design standards without changing code
argument-hint: "[page, screenshot, component, or file path]"
---
Review ${ARGUMENTS:-the current interface} without editing code. Evaluate it as a senior product designer.

Focus on:

- consistency with the existing product theme and design tokens
- visual hierarchy, alignment, spacing rhythm, responsive behavior, and overlap risks
- restrained color use rather than rainbow action colors, neon accents, or excessive saturation
- avoiding decorative solid borders, outlined pills, and clusters of tiny outlined buttons
- avoiding oversized pastel rounded containers around single ordinary actions
- uniform, deliberate typography rather than casual font mixing
- thoughtful use of Space Grotesk, Playfair Display, Figtree, Archivo, Inter, Poppins, and Montserrat when present
- clear primary, secondary, and destructive action hierarchy
- accessibility, contrast, focus visibility, touch targets, and reduced motion
- whether the result feels specific and original rather than copied from a generic UI template

Organize findings by impact: Critical, High, Medium, and Low. For each finding, cite the relevant screen, component, or file; explain the visual or usability impact; and recommend a concrete correction. Skip categories with no findings. End with the five highest-value improvements in priority order.
