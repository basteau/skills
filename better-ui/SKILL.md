---
name: better-ui
description: "Explicitly invoked router that loads only the interface guidance relevant to the user's prompt."
disable-model-invocation: true
---

# Better UI

Match the user's request to the references below. Read the smallest relevant set,
then follow deeper links only when their detail is needed. These are Markdown
references, not separate skills; do not load the whole collection by default.

| Prompt concerns | Read |
| --- | --- |
| Keyboard, focus, semantics, screen readers, hit areas, reduced motion | [better-accessibility](references/better-accessibility.md) |
| Grouping, spacing, alignment, responsive layout, overflow, RTL | [better-layout](references/better-layout.md) |
| Labels, errors, empty states, instructions, tone, terminology | [better-writing](references/better-writing.md) |
| Fonts, hierarchy, wrapping, line height, truncation, text rendering | [better-typography](references/better-typography.md) |
| Palettes, tokens, themes, contrast, semantic color | [better-colors](references/better-colors.md) |
| Surfaces, radii, shadows, icons, optical alignment | [better-polish](references/better-polish.md) |
| Transitions, easing, gestures, springs, motion performance, effect names | [better-animation](references/better-animation.md) |
| Mobile web or PWA feel, sticky hover, tap feedback, viewport or keyboard bugs, safe areas, scroll ownership, browser chrome | [better-pwa](references/better-pwa.md) |
| Review or audit spanning domains | [better-interface](references/better-interface.md), plus the relevant domains |

Route by intent, not isolated words. Combine domains when the problem crosses their
boundaries; a label rewrite does not need typography unless its rendering is also
at issue. Load all applicable domains only when the requested scope requires their
combined guidance. Use better-pwa for mobile web and installed web apps; native-app
motion still belongs to better-animation.
If the request has no actionable scope, infer it from the current task or ask for
the missing surface or problem.

Answer, build, fix, or review as requested. Reference reporting sections apply only
to reviews. Keep the project's stack and conventions; adapt platform-specific
examples rather than prescribing a technology. Motion belongs to better-animation;
mobile browser behavior belongs to better-pwa; accessibility requirements belong to
better-accessibility. Report shared causes once.
