# UI polish

Small visual details compound: nested radii, optical alignment, surfaces, and icons.
Keep the project's component library, tokens, and density. Translate implementation
examples into the target platform's idiom.

Use [better-animation](better-animation.md) for motion behavior and
[better-accessibility](better-accessibility.md) for hit areas, focus, and semantics.
Text rendering belongs to [better-typography](better-typography.md); grouping and
spacing belong to [better-layout](better-layout.md). Load those only when relevant.

## Concentric border radius

Outer radius = inner radius + padding. Mismatched radii on nested elements is the most common thing that makes an interface feel off. Radius, shadow and outline recipes are in [surfaces.md](better-polish/surfaces.md).

## Optical over geometric alignment

When geometric centering looks off, align optically. Buttons with icons, play triangles and asymmetric icons all need a manual nudge.

## Shadows for elevation, borders for structure

Where a border exists only to create depth, prefer layered transparent `box-shadow` values. Keep borders that communicate structure or state: dividers, separators and selected or focus states.

## Image outlines

Give images a `1px` outline at low opacity for consistent depth. Pure black in light mode (`oklch(0 0 0 / 0.1)`), pure white in dark (`oklch(1 0 0 / 0.1)`). Never a near-black like slate or zinc and never a tinted neutral. A tinted outline picks up the surface underneath and reads as dirt on the image edge.

## Match icon stroke to text weight

An icon next to text carries the text's optical weight: `1.5px` stroke beside regular (400) text, `2px` beside semibold (600). One stroke weight per icon set and one icon library per surface. Sizing and RTL flipping are in [icons.md](better-polish/icons.md).

## One SVG, recolored per state

Icons use `currentColor` and take hover, selected and disabled states from CSS color and opacity, never from separate assets. Outline is the default variant; fill marks the active state.

## Reporting

For reviews, cite the affected file and line, current behavior, proposed correction,
and user impact. HIGH blocks an interaction or hides state; MEDIUM is a visible
inconsistency; LOW is isolated polish. Inspect relevant rendered states and mark
unrun checks as not verified. For a cross-domain review, use
[better-interface](better-interface.md)'s shared severity and format.
