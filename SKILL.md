---
name: connect-ds
description: >-
  The published design token set for Connect - DS, with the CSS custom property names
  to emit. 259 tokens across 6 collections (Colors, sizes, Space, border radius,
  Typography, Units), one mode only — no light/dark pair. Use this skill whenever you
  are writing or reviewing UI code for a Connect - DS surface and are about to choose a
  color, spacing value, radius, size, font weight, or border — even when the request
  never mentions tokens, Figma, or the design system. Also use it when reviewing a diff
  or PR for hardcoded values, when turning a mockup or screenshot into code, and when
  someone asks what the token for something is or whether a value is on-scale. Provides
  the current published values so you never invent a hex or an off-scale number.
---

<!-- GENERATED:START -- do not edit, overwritten by .github/scripts/recompile.mjs -->
Compiled from this file's Variables section (content fingerprint 47f12baed840).
259 tokens, 6 collection(s), 2 mode(s): Inter, Mode 1.
<!-- GENERATED:END -->

## What this system contains

<!-- CONTENTS:START -- do not edit, overwritten by .github/scripts/recompile.mjs -->
259 tokens across 6 collection(s): border radius, Colors, sizes, Space, Typography, Units.

**Modes.** Inter, Mode 1. A token's value differs per mode; read the mode you're targeting from `tokenSync.resolved` (or `tokenSync.modes` for the unresolved alias reference, if present — it may have been trimmed, see the Variables section note).

**Primitive vs semantic.** Tokens under a `Semantic.*`-style path carry intent; `Primitive.*`-style paths are raw palette/scale entries — inferred from path segments, not a flag. Prefer the semantic one.

| Collection | Shape | Examples |
|---|---|---|
| border radius | flat-named | `Sharp`, `Soft` |
| Colors | flat-named | `Primitive.brand.0`, `Primitive.brand.5` |
| sizes | flat-named | `size-4`, `size-6` |
| Space | flat-named | `space-4`, `space-8` |
| Typography | flat-named | `Primitive typography.Font-Family.family`, `Primitive typography.Letter-spacing.Letter spacing` |
| Units | flat-named | `unit-0`, `unit-1` |

Alias chains present: `space-4 → size-4 → unit-4`.

`$value` may be an unresolved reference (`{other.token.path}`); `$extensions.tokenSync.resolved` holds the literal. Always read the resolved value, never the raw `$value`, when the two might differ.
<!-- CONTENTS:END -->

## What this system does NOT publish

<!-- MISSING:START -- do not edit, overwritten by .github/scripts/recompile.mjs -->
No tokens exist for: shadow/elevation, z-index, motion/duration, easing, breakpoints, opacity.
<!-- MISSING:END -->

If a task needs one of the categories listed above, say so and stop — don't improvise a
value and don't assume a token exists under a name you haven't seen. Improvised values are
the main way a token system erodes, because they look intentional in review.

## Writing code: emit CSS custom properties

Reference the CSS custom property by name (see the generated block below for the real
names); never inline the literal it resolves to. The indirection
exists so values can be re-pointed centrally — a value baked into a stylesheet can't be
updated by republishing the design system, a `var()` reference can.

```css
/* Good */
.button-primary {
  background: var(--action-primary);
  padding: var(--gap-md);
}

/* Bad — looks identical today, breaks the moment the source token changes */
.button-primary {
  background: #1B6AE8;
  padding: 8px;
}
```

**Deriving a variable name from a token path:** lowercase, drop the `Semantic.` /
`Primitive.` prefix (or your library's equivalent path segments — see "What this system
contains" above for whether that split exists here), join the remaining segments with
hyphens. Flat scale tokens keep their name as-is. When two different token paths would
derive the same variable name, the generator prefixes the shorter one with its collection
name to disambiguate — check the generated CSS block below for the actual name if a
derived one looks off; don't reconstruct it by hand from the path alone.

## Choosing the right token

**Colors.** Reach for `Semantic.*` first. The semantic layer covers four surfaces —
`background`, `text`, `stroke`, `icon` — each with a strength ramp, plus a `state` group
(`success`, `warning`, `error`, `information`, `away`, `feature`, `neutral`, `verified`).
Use `Primitive.*` only when no semantic token expresses the intent; brand color is the main
case where you have no choice (see Known issues).

One trap in the background ramp: it is **not** ordered light-to-dark the way the names suggest.
`--background-strong` (#111927) and `--background-surface` (#384250) are dark; `weak`, `weaker`,
and `weakest` are the near-whites. "Strong" means strong contrast against the page, not a strong
tint. Pair the two dark backgrounds with `--text-white` and `--icon-white`.

**Spacing vs sizing.** `--space-*` is for gaps, padding, and margins. `--size-*` is for dimensions
— icon boxes, control heights, avatars. They overlap numerically but express different intent, and
`--size-*` has finer granularity (6, 10, 18, 22, 28, 30, 38, 56, 60) where `--space-*` deliberately
doesn't. Picking `--size-16` for padding isn't wrong-looking, but it muddies the intent that lets
the scales be tuned independently later.

**Radius.** Four values only: `--radius-sharp` (0), `--radius-soft` (8), `--radius-softer` (16),
`--radius-round` (999, for pills and circles). Anything between these is off-scale.

**Typography.** Usable today: `--font-family` (Inter), `--letter-spacing` (0),
`--font-weight-regular|medium|semi-bold|bold`, and a line-height set
(`--line-height-tightest` 110% through `--line-height-spacious` 200%, plus `--line-height-auto`).
Font *size* is not reliably published — see Known issues for what to do instead.

## Known issues

Auto-detected problems in the published data, each with a concrete workaround — not a
style opinion, a specific thing that will bite you if you don't know about it.

<!-- LINT:START -- do not edit, overwritten by .github/scripts/recompile.mjs -->
1. `"Primitive typography.Font-size.--font-size-xs: 12px; --font-size-sm: 14px; --font-size-md: 16px; --font-size-lg: 20px; --font-size-xl: 24px; --font-size-2xl: 32px;"` contains a character (`:`, `;`, `{`, or a newline) that looks like pasted CSS rather than a token name — check the source variable in Figma.
2. Group `Primitive typography.Font-size` mixes types (number, string) across siblings: `Primitive typography.Font-size.--font-size-xs`, `Primitive typography.Font-size.--font-size-sm`, `Primitive typography.Font-size.Semi bold`, `Primitive typography.Font-size.--font-size-xs: 12px; --font-size-sm: 14px; --font-size-md: 16px; --font-size-lg: 20px; --font-size-xl: 24px; --font-size-2xl: 32px;` — check whether one was published with the wrong type.
3. Group `Semantic typography.Universal` has the exact same members and values as `Primitive typography.Letter-spacing` — likely a duplicate publish, not two intentionally-identical scales. Not auto-removed (a false positive here would silently drop real tokens); confirm by hand before deleting either.
4. Group `Semantic typography.Brand` has the exact same members and values as `Primitive typography.Font-height` — likely a duplicate publish, not two intentionally-identical scales. Not auto-removed (a false positive here would silently drop real tokens); confirm by hand before deleting either.
5. `unit-70 2` looks like an unrenamed Figma duplicate (ends in "2") — confirm this is an intentional variant, not a copy-paste leftover.
6. `Primitive.brand.0` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
7. `Primitive.brand.5` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
8. `Primitive.brand.10` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
9. `Primitive.brand.20` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
10. `Primitive.brand.30` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
11. `Primitive.brand.40` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
12. `Primitive.brand.50` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
13. `Primitive.brand.60` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
14. `Primitive.brand.70` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
15. `Primitive.brand.80` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
16. `Primitive.brand.90` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
17. `Primitive.brand.100` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
18. `Primitive.base.black` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
19. `Primitive.neutral.150` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
20. `Primitive.neutral.600` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
21. `Primitive.neutral.800` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
22. `Primitive.neutral.950` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
23. `Primitive.blue.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
24. `Primitive.blue.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
25. `Primitive.blue.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
26. `Primitive.blue.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
27. `Primitive.green.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
28. `Primitive.green.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
29. `Primitive.green.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
30. `Primitive.green.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
31. `Primitive.orange.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
32. `Primitive.orange.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
33. `Primitive.orange.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
34. `Primitive.orange.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
35. `Primitive.yellow.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
36. `Primitive.yellow.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
37. `Primitive.yellow.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
38. `Primitive.yellow.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
39. `Primitive.red.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
40. `Primitive.red.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
41. `Primitive.red.red-[Custom]` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
42. `Primitive.red.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
43. `Primitive.red.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
44. `Primitive.purple.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
45. `Primitive.purple.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
46. `Primitive.purple.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
47. `Primitive.purple.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
48. `Primitive.pink.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
49. `Primitive.pink.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
50. `Primitive.pink.base` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
51. `Primitive.pink.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
52. `Primitive.pink.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
53. `Primitive.teal.lighter` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
54. `Primitive.teal.light` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
55. `Primitive.teal.dark` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
56. `Primitive.teal.darker` is a primitive that no semantic token aliases — either it's unused, or something references it in a way this check can't see. The "prefer semantic" rule has no semantic equivalent to point to here.
<!-- LINT:END -->

<!-- Anything you add below this line, outside the LINT markers, survives recompilation —
     use it for issues you've found that the automated checks can't catch. -->

## Anti-patterns

- **Raw hex, `rgb()`, or `rgba()` values in a stylesheet** instead of a token reference.
- **`#fff` and `#000`** — too obvious to question in review, which is exactly why they
  survive in otherwise-tokenized codebases.
- **Off-scale numbers** for spacing, radius, or sizing — snap to the nearest published
  value, don't split the difference between two tokens.
- **Reaching into a primitive** when a semantic token with the same intent exists.
- **Inventing a plausible-looking variable name** that wasn't actually published — a
  guessed name is a silent no-op at runtime that looks correct in review, which makes it
  strictly worse than a hardcoded value. Only use a name from the generated CSS block
  below.
- **Deriving a value from a token's *name*** instead of its resolved value — a token
  called `blue-500` is not necessarily blue; read `tokenSync.resolved`, never guess from
  the label.

## CSS variables

<!-- CSS:START -- do not edit, overwritten by .github/scripts/recompile.mjs -->
```css
:root {
  --background-soft: #E5E7EB;
  --background-strong: #111927;
  --background-surface: #384250;
  --background-weak: #F3F4F6;
  --background-weaker: #F9FAFB;
  --background-weakest: #FCFCFD;
  --background-white: #FFFFFF;
  --base-black: #000000;
  --base-white: #FFFFFF;
  --blue-base: #375DFB;
  --blue-dark: #253EA7;
  --blue-darker: #162664;
  --blue-light: #C2D6FF;
  --blue-lighter: #EBF1FF;
  --brand-0: rgba(80, 90, 172, 0);
  --brand-100: #505AAC;
  --brand-10: rgba(80, 90, 172, 0.1);
  --brand-20: rgba(80, 90, 172, 0.2);
  --brand-30: rgba(80, 90, 172, 0.3);
  --brand-40: rgba(80, 90, 172, 0.4);
  --brand-50: rgba(80, 90, 172, 0.5);
  --brand-5: rgba(80, 90, 172, 0.05);
  --brand-60: rgba(80, 90, 172, 0.6);
  --brand-70: rgba(80, 90, 172, 0.7);
  --brand-80: rgba(80, 90, 172, 0.8);
  --brand-90: rgba(80, 90, 172, 0.9);
  --green-base: #38C793;
  --green-dark: #2D9F75;
  --green-darker: #176448;
  --green-light: #CBF5E5;
  --green-lighter: #EFFAF6;
  --icon-disabled: #D2D6DB;
  --icon-soft: #9DA4AE;
  --icon-strong: #111927;
  --icon-sub: #6C737F;
  --icon-white: #FFFFFF;
  --neutral-100: #F3F4F6;
  --neutral-150: #F2F3F5;
  --neutral-200: #E5E7EB;
  --neutral-25: #FCFCFD;
  --neutral-300: #D2D6DB;
  --neutral-400: #9DA4AE;
  --neutral-500: #6C737F;
  --neutral-50: #F9FAFB;
  --neutral-600: #4D5761;
  --neutral-700: #384250;
  --neutral-800: #1F2A37;
  --neutral-900: #111927;
  --neutral-950: #0D121C;
  --orange-base: #F17B2C;
  --orange-dark: #C2540A;
  --orange-darker: #6E330C;
  --orange-light: #FFDAC2;
  --orange-lighter: #FEF3EB;
  --pink-base: #E255F2;
  --pink-dark: #9C23A9;
  --pink-darker: #620F6C;
  --pink-light: #F9C2FF;
  --pink-lighter: #FDEBFF;
  --primitive-typography-font-family-family: Inter;
  --primitive-typography-font-height-auto: Auto;
  --primitive-typography-font-height-compact: 140%;
  --primitive-typography-font-height-loose: 180%;
  --primitive-typography-font-height-medium: 160%;
  --primitive-typography-font-height-spacious: 200%;
  --primitive-typography-font-height-tight: 120%;
  --primitive-typography-font-height-tightest: 110%;
  --primitive-typography-font-size-font-size-sm: medium;
  --primitive-typography-font-size-font-size-xs-12px-font-size-sm-14px-font-size-md-16px-font-size-lg-20px-font-size-xl-24px-font-size-2xl-32px: Bold;
  --primitive-typography-font-size-font-size-xs: 12px;
  --primitive-typography-font-size-semi-bold: semi bold;
  --primitive-typography-font-weight-bold: Bold;
  --primitive-typography-font-weight-medium: medium;
  --primitive-typography-font-weight-regular: regular;
  --primitive-typography-font-weight-semi-bold: semi bold;
  --primitive-typography-letter-spacing-letter-spacing: 0px;
  --purple-base: #6E3FF3;
  --purple-dark: #5A36BF;
  --purple-darker: #2B1664;
  --purple-light: #CAC2FF;
  --purple-lighter: #EEEBFF;
  --red-base: #DF1C41;
  --red-dark: #AF1D38;
  --red-darker: #710E21;
  --red-light: #F8C9D2;
  --red-lighter: #FDEDF0;
  --red-red-custom: #DD4A4A;
  --round: 999px;
  --semantic-typography-brand-auto: Auto;
  --semantic-typography-brand-compact: 140%;
  --semantic-typography-brand-loose: 180%;
  --semantic-typography-brand-medium: 160%;
  --semantic-typography-brand-spacious: 200%;
  --semantic-typography-brand-tight: 120%;
  --semantic-typography-brand-tightest: 110%;
  --semantic-typography-universal-letter-spacing: 0px;
  --sharp: 0px;
  --size-10: 10px;
  --size-12: 12px;
  --size-14: 14px;
  --size-16: 16px;
  --size-18: 18px;
  --size-20: 20px;
  --size-22: 22px;
  --size-24: 24px;
  --size-28: 28px;
  --size-30: 30px;
  --size-32: 32px;
  --size-38: 38px;
  --size-40: 40px;
  --size-46: 46px;
  --size-4: 4px;
  --size-56: 56px;
  --size-60: 60px;
  --size-64: 64px;
  --size-6: 6px;
  --size-8: 8px;
  --soft: 8px;
  --softer: 16px;
  --space-12: 12px;
  --space-14: 14px;
  --space-16: 16px;
  --space-20: 20px;
  --space-24: 24px;
  --space-32: 32px;
  --space-40: 40px;
  --space-46: 46px;
  --space-4: 4px;
  --space-64: 64px;
  --space-8: 8px;
  --state-away: #F2AE40;
  --state-error: #DF1C41;
  --state-feature: #6E3FF3;
  --state-information: #375DFB;
  --state-neutral: #9DA4AE;
  --state-success: #38C793;
  --state-verified: #35B9E9;
  --state-warning: #F17B2C;
  --stroke-disabled: #F3F4F6;
  --stroke-soft: #E5E7EB;
  --stroke-strong: #111927;
  --stroke-sub: #D2D6DB;
  --stroke-white: #FFFFFF;
  --teal-base: #35B9E9;
  --teal-dark: #1F87AD;
  --teal-darker: #164564;
  --teal-light: #C2EFFF;
  --teal-lighter: #EBFAFF;
  --text-disabled: #D2D6DB;
  --text-main: #111927;
  --text-soft: #9DA4AE;
  --text-sub: #6C737F;
  --text-white: #FFFFFF;
  --unit-0: 0px;
  --unit-100: 100px;
  --unit-10: 10px;
  --unit-11: 11px;
  --unit-12: 12px;
  --unit-13: 13px;
  --unit-14: 14px;
  --unit-15: 15px;
  --unit-16: 16px;
  --unit-17: 17px;
  --unit-18: 18px;
  --unit-19: 19px;
  --unit-1: 1px;
  --unit-20: 20px;
  --unit-21: 21px;
  --unit-22: 22px;
  --unit-23: 23px;
  --unit-24: 24px;
  --unit-25: 25px;
  --unit-26: 26px;
  --unit-27: 27px;
  --unit-28: 28px;
  --unit-29: 29px;
  --unit-2: 2px;
  --unit-30: 30px;
  --unit-31: 31px;
  --unit-32: 32px;
  --unit-33: 33px;
  --unit-34: 34px;
  --unit-35: 35px;
  --unit-36: 36px;
  --unit-37: 37px;
  --unit-38: 38px;
  --unit-39: 39px;
  --unit-3: 3px;
  --unit-40: 40px;
  --unit-41: 41px;
  --unit-42: 42px;
  --unit-43: 43px;
  --unit-44: 44px;
  --unit-45: 45px;
  --unit-46: 46px;
  --unit-47: 47px;
  --unit-48: 48px;
  --unit-49: 49px;
  --unit-4: 4px;
  --unit-50: 50px;
  --unit-51: 51px;
  --unit-52: 52px;
  --unit-53: 53px;
  --unit-54: 54px;
  --unit-55: 55px;
  --unit-56: 56px;
  --unit-57: 57px;
  --unit-58: 58px;
  --unit-59: 59px;
  --unit-5: 5px;
  --unit-60: 60px;
  --unit-61: 61px;
  --unit-62: 62px;
  --unit-63: 63px;
  --unit-64: 64px;
  --unit-65: 65px;
  --unit-66: 66px;
  --unit-67: 67px;
  --unit-68: 68px;
  --unit-69: 69px;
  --unit-6: 6px;
  --unit-70-2: 71px;
  --unit-70: 70px;
  --unit-71: 72px;
  --unit-72: 73px;
  --unit-73: 74px;
  --unit-74: 75px;
  --unit-75: 76px;
  --unit-76: 77px;
  --unit-77: 78px;
  --unit-78: 79px;
  --unit-7: 7px;
  --unit-80: 80px;
  --unit-81: 81px;
  --unit-82: 82px;
  --unit-83: 83px;
  --unit-84: 84px;
  --unit-85: 85px;
  --unit-86: 86px;
  --unit-87: 87px;
  --unit-88: 88px;
  --unit-89: 89px;
  --unit-8: 8px;
  --unit-90: 90px;
  --unit-91: 91px;
  --unit-92: 92px;
  --unit-93: 93px;
  --unit-94: 94px;
  --unit-95: 95px;
  --unit-96: 96px;
  --unit-97: 97px;
  --unit-98: 98px;
  --unit-99: 99px;
  --unit-9: 9px;
  --yellow-base: #F2AE40;
  --yellow-dark: #B47818;
  --yellow-darker: #693D11;
  --yellow-light: #FBDFB1;
  --yellow-lighter: #FEF7EC;
}
```
<!-- CSS:END -->

## Variables

Full DTCG-formatted token tree, trimmed of fields that carry no information for this
library (see the emission rules in `docs/repo-per-file-design.md` if a field you expect
is missing — it's a deliberate drop, not lost data) and embedded here rather than as a
separate file. Search this section for the path you need rather than reading the whole
block whenever this skill triggers.

<!-- VARIABLES:START -- do not edit, overwritten by .github/scripts/recompile.mjs -->
```json
{
  "Sharp": {
    "$type": "number",
    "$value": "{unit-0}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-0}"
      },
      "tokenSync.resolved": {
        "Mode 1": 0
      },
      "tokenSync.collection": "border radius"
    }
  },
  "Soft": {
    "$type": "number",
    "$value": "{unit-8}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-8}"
      },
      "tokenSync.resolved": {
        "Mode 1": 8
      },
      "tokenSync.collection": "border radius"
    }
  },
  "Softer": {
    "$type": "number",
    "$value": "{unit-16}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-16}"
      },
      "tokenSync.resolved": {
        "Mode 1": 16
      },
      "tokenSync.collection": "border radius"
    }
  },
  "Round": {
    "$type": "number",
    "$value": 999,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 999
      },
      "tokenSync.resolved": {
        "Mode 1": 999
      },
      "tokenSync.collection": "border radius"
    }
  },
  "Primitive": {
    "brand": {
      "0": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "5": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.05)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.05)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.05)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "10": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.1)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.1)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.1)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "20": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.2)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.2)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.2)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "30": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.3)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.3)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.3)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "40": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.4)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.4)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.4)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "50": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.5)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.5)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.5)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "60": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.6)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.6)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.6)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "70": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.7)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.7)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.7)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "80": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.8)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.8)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.8)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "90": {
        "$type": "color",
        "$value": "rgba(80, 90, 172, 0.9)",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "rgba(80, 90, 172, 0.9)"
          },
          "tokenSync.resolved": {
            "Mode 1": "rgba(80, 90, 172, 0.9)"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "100": {
        "$type": "color",
        "$value": "#505AAC",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#505AAC"
          },
          "tokenSync.resolved": {
            "Mode 1": "#505AAC"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "base": {
      "white": {
        "$type": "color",
        "$value": "#FFFFFF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FFFFFF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FFFFFF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "black": {
        "$type": "color",
        "$value": "#000000",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#000000"
          },
          "tokenSync.resolved": {
            "Mode 1": "#000000"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "neutral": {
      "25": {
        "$type": "color",
        "$value": "#FCFCFD",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FCFCFD"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FCFCFD"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "50": {
        "$type": "color",
        "$value": "#F9FAFB",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F9FAFB"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F9FAFB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "100": {
        "$type": "color",
        "$value": "#F3F4F6",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F3F4F6"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F3F4F6"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "150": {
        "$type": "color",
        "$value": "#F2F3F5",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F2F3F5"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F2F3F5"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "200": {
        "$type": "color",
        "$value": "#E5E7EB",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#E5E7EB"
          },
          "tokenSync.resolved": {
            "Mode 1": "#E5E7EB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "300": {
        "$type": "color",
        "$value": "#D2D6DB",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#D2D6DB"
          },
          "tokenSync.resolved": {
            "Mode 1": "#D2D6DB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "400": {
        "$type": "color",
        "$value": "#9DA4AE",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#9DA4AE"
          },
          "tokenSync.resolved": {
            "Mode 1": "#9DA4AE"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "500": {
        "$type": "color",
        "$value": "#6C737F",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#6C737F"
          },
          "tokenSync.resolved": {
            "Mode 1": "#6C737F"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "600": {
        "$type": "color",
        "$value": "#4D5761",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#4D5761"
          },
          "tokenSync.resolved": {
            "Mode 1": "#4D5761"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "700": {
        "$type": "color",
        "$value": "#384250",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#384250"
          },
          "tokenSync.resolved": {
            "Mode 1": "#384250"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "800": {
        "$type": "color",
        "$value": "#1F2A37",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#1F2A37"
          },
          "tokenSync.resolved": {
            "Mode 1": "#1F2A37"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "900": {
        "$type": "color",
        "$value": "#111927",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#111927"
          },
          "tokenSync.resolved": {
            "Mode 1": "#111927"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "950": {
        "$type": "color",
        "$value": "#0D121C",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#0D121C"
          },
          "tokenSync.resolved": {
            "Mode 1": "#0D121C"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "blue": {
      "lighter": {
        "$type": "color",
        "$value": "#EBF1FF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#EBF1FF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#EBF1FF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#C2D6FF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#C2D6FF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#C2D6FF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#375DFB",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#375DFB"
          },
          "tokenSync.resolved": {
            "Mode 1": "#375DFB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#253EA7",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#253EA7"
          },
          "tokenSync.resolved": {
            "Mode 1": "#253EA7"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#162664",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#162664"
          },
          "tokenSync.resolved": {
            "Mode 1": "#162664"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "green": {
      "lighter": {
        "$type": "color",
        "$value": "#EFFAF6",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#EFFAF6"
          },
          "tokenSync.resolved": {
            "Mode 1": "#EFFAF6"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#CBF5E5",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#CBF5E5"
          },
          "tokenSync.resolved": {
            "Mode 1": "#CBF5E5"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#38C793",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#38C793"
          },
          "tokenSync.resolved": {
            "Mode 1": "#38C793"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#2D9F75",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#2D9F75"
          },
          "tokenSync.resolved": {
            "Mode 1": "#2D9F75"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#176448",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#176448"
          },
          "tokenSync.resolved": {
            "Mode 1": "#176448"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "orange": {
      "lighter": {
        "$type": "color",
        "$value": "#FEF3EB",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FEF3EB"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FEF3EB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#FFDAC2",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FFDAC2"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FFDAC2"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#F17B2C",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F17B2C"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F17B2C"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#C2540A",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#C2540A"
          },
          "tokenSync.resolved": {
            "Mode 1": "#C2540A"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#6E330C",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#6E330C"
          },
          "tokenSync.resolved": {
            "Mode 1": "#6E330C"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "yellow": {
      "lighter": {
        "$type": "color",
        "$value": "#FEF7EC",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FEF7EC"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FEF7EC"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#FBDFB1",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FBDFB1"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FBDFB1"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#F2AE40",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F2AE40"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F2AE40"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#B47818",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#B47818"
          },
          "tokenSync.resolved": {
            "Mode 1": "#B47818"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#693D11",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#693D11"
          },
          "tokenSync.resolved": {
            "Mode 1": "#693D11"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "red": {
      "lighter": {
        "$type": "color",
        "$value": "#FDEDF0",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FDEDF0"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FDEDF0"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#F8C9D2",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F8C9D2"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F8C9D2"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "red-[Custom]": {
        "$type": "color",
        "$value": "#DD4A4A",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#DD4A4A"
          },
          "tokenSync.resolved": {
            "Mode 1": "#DD4A4A"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#DF1C41",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#DF1C41"
          },
          "tokenSync.resolved": {
            "Mode 1": "#DF1C41"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#AF1D38",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#AF1D38"
          },
          "tokenSync.resolved": {
            "Mode 1": "#AF1D38"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#710E21",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#710E21"
          },
          "tokenSync.resolved": {
            "Mode 1": "#710E21"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "purple": {
      "lighter": {
        "$type": "color",
        "$value": "#EEEBFF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#EEEBFF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#EEEBFF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#CAC2FF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#CAC2FF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#CAC2FF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#6E3FF3",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#6E3FF3"
          },
          "tokenSync.resolved": {
            "Mode 1": "#6E3FF3"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#5A36BF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#5A36BF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#5A36BF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#2B1664",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#2B1664"
          },
          "tokenSync.resolved": {
            "Mode 1": "#2B1664"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "pink": {
      "lighter": {
        "$type": "color",
        "$value": "#FDEBFF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#FDEBFF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FDEBFF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#F9C2FF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#F9C2FF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F9C2FF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#E255F2",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#E255F2"
          },
          "tokenSync.resolved": {
            "Mode 1": "#E255F2"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#9C23A9",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#9C23A9"
          },
          "tokenSync.resolved": {
            "Mode 1": "#9C23A9"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#620F6C",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#620F6C"
          },
          "tokenSync.resolved": {
            "Mode 1": "#620F6C"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "teal": {
      "lighter": {
        "$type": "color",
        "$value": "#EBFAFF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#EBFAFF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#EBFAFF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "light": {
        "$type": "color",
        "$value": "#C2EFFF",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#C2EFFF"
          },
          "tokenSync.resolved": {
            "Mode 1": "#C2EFFF"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "base": {
        "$type": "color",
        "$value": "#35B9E9",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#35B9E9"
          },
          "tokenSync.resolved": {
            "Mode 1": "#35B9E9"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "dark": {
        "$type": "color",
        "$value": "#1F87AD",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#1F87AD"
          },
          "tokenSync.resolved": {
            "Mode 1": "#1F87AD"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "darker": {
        "$type": "color",
        "$value": "#164564",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "#164564"
          },
          "tokenSync.resolved": {
            "Mode 1": "#164564"
          },
          "tokenSync.collection": "Colors"
        }
      }
    }
  },
  "Semantic": {
    "background": {
      "strong": {
        "$type": "color",
        "$value": "{Primitive.neutral.900}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.900}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#111927"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "surface": {
        "$type": "color",
        "$value": "{Primitive.neutral.700}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.700}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#384250"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "soft": {
        "$type": "color",
        "$value": "{Primitive.neutral.200}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.200}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#E5E7EB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "weak": {
        "$type": "color",
        "$value": "{Primitive.neutral.100}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.100}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F3F4F6"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "weaker": {
        "$type": "color",
        "$value": "{Primitive.neutral.50}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.50}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F9FAFB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "weakest": {
        "$type": "color",
        "$value": "{Primitive.neutral.25}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.25}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FCFCFD"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "white": {
        "$type": "color",
        "$value": "{Primitive.base.white}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.base.white}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FFFFFF"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "text": {
      "main": {
        "$type": "color",
        "$value": "{Primitive.neutral.900}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.900}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#111927"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "sub": {
        "$type": "color",
        "$value": "{Primitive.neutral.500}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.500}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#6C737F"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "soft": {
        "$type": "color",
        "$value": "{Primitive.neutral.400}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.400}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#9DA4AE"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "disabled": {
        "$type": "color",
        "$value": "{Primitive.neutral.300}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.300}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#D2D6DB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "white": {
        "$type": "color",
        "$value": "{Primitive.base.white}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.base.white}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FFFFFF"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "stroke": {
      "strong": {
        "$type": "color",
        "$value": "{Primitive.neutral.900}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.900}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#111927"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "sub": {
        "$type": "color",
        "$value": "{Primitive.neutral.300}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.300}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#D2D6DB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "soft": {
        "$type": "color",
        "$value": "{Primitive.neutral.200}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.200}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#E5E7EB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "disabled": {
        "$type": "color",
        "$value": "{Primitive.neutral.100}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.100}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F3F4F6"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "white": {
        "$type": "color",
        "$value": "{Primitive.base.white}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.base.white}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FFFFFF"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "icon": {
      "strong": {
        "$type": "color",
        "$value": "{Primitive.neutral.900}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.900}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#111927"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "sub": {
        "$type": "color",
        "$value": "{Primitive.neutral.500}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.500}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#6C737F"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "soft": {
        "$type": "color",
        "$value": "{Primitive.neutral.400}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.400}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#9DA4AE"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "disabled": {
        "$type": "color",
        "$value": "{Primitive.neutral.300}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.300}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#D2D6DB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "white": {
        "$type": "color",
        "$value": "{Primitive.base.white}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.base.white}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#FFFFFF"
          },
          "tokenSync.collection": "Colors"
        }
      }
    },
    "state": {
      "success": {
        "$type": "color",
        "$value": "{Primitive.green.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.green.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#38C793"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "warning": {
        "$type": "color",
        "$value": "{Primitive.orange.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.orange.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F17B2C"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "error": {
        "$type": "color",
        "$value": "{Primitive.red.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.red.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#DF1C41"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "information": {
        "$type": "color",
        "$value": "{Primitive.blue.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.blue.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#375DFB"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "away": {
        "$type": "color",
        "$value": "{Primitive.yellow.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.yellow.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#F2AE40"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "feature": {
        "$type": "color",
        "$value": "{Primitive.purple.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.purple.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#6E3FF3"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "neutral": {
        "$type": "color",
        "$value": "{Primitive.neutral.400}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.neutral.400}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#9DA4AE"
          },
          "tokenSync.collection": "Colors"
        }
      },
      "verified": {
        "$type": "color",
        "$value": "{Primitive.teal.base}",
        "$extensions": {
          "tokenSync.modes": {
            "Mode 1": "{Primitive.teal.base}"
          },
          "tokenSync.resolved": {
            "Mode 1": "#35B9E9"
          },
          "tokenSync.collection": "Colors"
        }
      }
    }
  },
  "size-4": {
    "$type": "number",
    "$value": "{unit-4}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-4}"
      },
      "tokenSync.resolved": {
        "Mode 1": 4
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-6": {
    "$type": "number",
    "$value": "{unit-6}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-6}"
      },
      "tokenSync.resolved": {
        "Mode 1": 6
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-8": {
    "$type": "number",
    "$value": "{unit-8}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-8}"
      },
      "tokenSync.resolved": {
        "Mode 1": 8
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-10": {
    "$type": "number",
    "$value": "{unit-10}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-10}"
      },
      "tokenSync.resolved": {
        "Mode 1": 10
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-12": {
    "$type": "number",
    "$value": "{unit-12}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-12}"
      },
      "tokenSync.resolved": {
        "Mode 1": 12
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-14": {
    "$type": "number",
    "$value": "{unit-14}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-14}"
      },
      "tokenSync.resolved": {
        "Mode 1": 14
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-16": {
    "$type": "number",
    "$value": "{unit-16}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-16}"
      },
      "tokenSync.resolved": {
        "Mode 1": 16
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-18": {
    "$type": "number",
    "$value": "{unit-18}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-18}"
      },
      "tokenSync.resolved": {
        "Mode 1": 18
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-20": {
    "$type": "number",
    "$value": "{unit-20}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-20}"
      },
      "tokenSync.resolved": {
        "Mode 1": 20
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-22": {
    "$type": "number",
    "$value": "{unit-22}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-22}"
      },
      "tokenSync.resolved": {
        "Mode 1": 22
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-24": {
    "$type": "number",
    "$value": "{unit-24}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-24}"
      },
      "tokenSync.resolved": {
        "Mode 1": 24
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-28": {
    "$type": "number",
    "$value": "{unit-28}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-28}"
      },
      "tokenSync.resolved": {
        "Mode 1": 28
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-30": {
    "$type": "number",
    "$value": "{unit-30}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-30}"
      },
      "tokenSync.resolved": {
        "Mode 1": 30
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-32": {
    "$type": "number",
    "$value": "{unit-32}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-32}"
      },
      "tokenSync.resolved": {
        "Mode 1": 32
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-38": {
    "$type": "number",
    "$value": "{unit-38}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-38}"
      },
      "tokenSync.resolved": {
        "Mode 1": 38
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-40": {
    "$type": "number",
    "$value": "{unit-40}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-40}"
      },
      "tokenSync.resolved": {
        "Mode 1": 40
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-46": {
    "$type": "number",
    "$value": "{unit-46}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-46}"
      },
      "tokenSync.resolved": {
        "Mode 1": 46
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-56": {
    "$type": "number",
    "$value": "{unit-56}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-56}"
      },
      "tokenSync.resolved": {
        "Mode 1": 56
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-60": {
    "$type": "number",
    "$value": "{unit-60}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-60}"
      },
      "tokenSync.resolved": {
        "Mode 1": 60
      },
      "tokenSync.collection": "sizes"
    }
  },
  "size-64": {
    "$type": "number",
    "$value": "{unit-64}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{unit-64}"
      },
      "tokenSync.resolved": {
        "Mode 1": 64
      },
      "tokenSync.collection": "sizes"
    }
  },
  "space-4": {
    "$type": "number",
    "$value": "{size-4}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-4}"
      },
      "tokenSync.resolved": {
        "Mode 1": 4
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-8": {
    "$type": "number",
    "$value": "{size-8}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-8}"
      },
      "tokenSync.resolved": {
        "Mode 1": 8
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-12": {
    "$type": "number",
    "$value": "{size-12}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-12}"
      },
      "tokenSync.resolved": {
        "Mode 1": 12
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-14": {
    "$type": "number",
    "$value": "{size-14}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-14}"
      },
      "tokenSync.resolved": {
        "Mode 1": 14
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-16": {
    "$type": "number",
    "$value": "{size-16}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-16}"
      },
      "tokenSync.resolved": {
        "Mode 1": 16
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-20": {
    "$type": "number",
    "$value": "{size-20}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-20}"
      },
      "tokenSync.resolved": {
        "Mode 1": 20
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-24": {
    "$type": "number",
    "$value": "{size-24}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-24}"
      },
      "tokenSync.resolved": {
        "Mode 1": 24
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-32": {
    "$type": "number",
    "$value": "{size-32}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-32}"
      },
      "tokenSync.resolved": {
        "Mode 1": 32
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-40": {
    "$type": "number",
    "$value": "{size-40}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-40}"
      },
      "tokenSync.resolved": {
        "Mode 1": 40
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-46": {
    "$type": "number",
    "$value": "{size-46}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-46}"
      },
      "tokenSync.resolved": {
        "Mode 1": 46
      },
      "tokenSync.collection": "Space"
    }
  },
  "space-64": {
    "$type": "number",
    "$value": "{size-64}",
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": "{size-64}"
      },
      "tokenSync.resolved": {
        "Mode 1": 64
      },
      "tokenSync.collection": "Space"
    }
  },
  "Primitive typography": {
    "Font-Family": {
      "family": {
        "$type": "string",
        "$value": "Inter",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "Inter"
          },
          "tokenSync.resolved": {
            "Inter": "Inter"
          },
          "tokenSync.collection": "Typography"
        }
      }
    },
    "Letter-spacing": {
      "Letter spacing": {
        "$type": "number",
        "$value": 0,
        "$extensions": {
          "tokenSync.modes": {
            "Inter": 0
          },
          "tokenSync.resolved": {
            "Inter": 0
          },
          "tokenSync.collection": "Typography"
        }
      }
    },
    "Font-height": {
      "Auto": {
        "$type": "string",
        "$value": "Auto",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "Auto"
          },
          "tokenSync.resolved": {
            "Inter": "Auto"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Tightest": {
        "$type": "string",
        "$value": "110%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "110%"
          },
          "tokenSync.resolved": {
            "Inter": "110%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Tight": {
        "$type": "string",
        "$value": "120%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "120%"
          },
          "tokenSync.resolved": {
            "Inter": "120%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Compact": {
        "$type": "string",
        "$value": "140%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "140%"
          },
          "tokenSync.resolved": {
            "Inter": "140%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Medium": {
        "$type": "string",
        "$value": "160%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "160%"
          },
          "tokenSync.resolved": {
            "Inter": "160%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Loose": {
        "$type": "string",
        "$value": "180%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "180%"
          },
          "tokenSync.resolved": {
            "Inter": "180%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Spacious": {
        "$type": "string",
        "$value": "200%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "200%"
          },
          "tokenSync.resolved": {
            "Inter": "200%"
          },
          "tokenSync.collection": "Typography"
        }
      }
    },
    "Font-weight": {
      "Regular": {
        "$type": "string",
        "$value": "regular",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "regular"
          },
          "tokenSync.resolved": {
            "Inter": "regular"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Medium": {
        "$type": "string",
        "$value": "medium",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "medium"
          },
          "tokenSync.resolved": {
            "Inter": "medium"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Semi bold": {
        "$type": "string",
        "$value": "semi bold",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "semi bold"
          },
          "tokenSync.resolved": {
            "Inter": "semi bold"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Bold": {
        "$type": "string",
        "$value": "Bold",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "Bold"
          },
          "tokenSync.resolved": {
            "Inter": "Bold"
          },
          "tokenSync.collection": "Typography"
        }
      }
    },
    "Font-size": {
      "--font-size-xs": {
        "$type": "number",
        "$value": "{unit-12}",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "{unit-12}"
          },
          "tokenSync.resolved": {
            "Inter": 12
          },
          "tokenSync.collection": "Typography"
        }
      },
      "--font-size-sm": {
        "$type": "string",
        "$value": "medium",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "medium"
          },
          "tokenSync.resolved": {
            "Inter": "medium"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Semi bold": {
        "$type": "string",
        "$value": "semi bold",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "semi bold"
          },
          "tokenSync.resolved": {
            "Inter": "semi bold"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "--font-size-xs: 12px; --font-size-sm: 14px; --font-size-md: 16px; --font-size-lg: 20px; --font-size-xl: 24px; --font-size-2xl: 32px;": {
        "$type": "string",
        "$value": "Bold",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "Bold"
          },
          "tokenSync.resolved": {
            "Inter": "Bold"
          },
          "tokenSync.collection": "Typography"
        }
      }
    }
  },
  "Semantic typography": {
    "Universal": {
      "Letter spacing": {
        "$type": "number",
        "$value": 0,
        "$extensions": {
          "tokenSync.modes": {
            "Inter": 0
          },
          "tokenSync.resolved": {
            "Inter": 0
          },
          "tokenSync.collection": "Typography"
        }
      }
    },
    "Brand": {
      "Auto": {
        "$type": "string",
        "$value": "Auto",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "Auto"
          },
          "tokenSync.resolved": {
            "Inter": "Auto"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Tightest": {
        "$type": "string",
        "$value": "110%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "110%"
          },
          "tokenSync.resolved": {
            "Inter": "110%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Tight": {
        "$type": "string",
        "$value": "120%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "120%"
          },
          "tokenSync.resolved": {
            "Inter": "120%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Compact": {
        "$type": "string",
        "$value": "140%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "140%"
          },
          "tokenSync.resolved": {
            "Inter": "140%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Medium": {
        "$type": "string",
        "$value": "160%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "160%"
          },
          "tokenSync.resolved": {
            "Inter": "160%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Loose": {
        "$type": "string",
        "$value": "180%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "180%"
          },
          "tokenSync.resolved": {
            "Inter": "180%"
          },
          "tokenSync.collection": "Typography"
        }
      },
      "Spacious": {
        "$type": "string",
        "$value": "200%",
        "$extensions": {
          "tokenSync.modes": {
            "Inter": "200%"
          },
          "tokenSync.resolved": {
            "Inter": "200%"
          },
          "tokenSync.collection": "Typography"
        }
      }
    }
  },
  "unit-0": {
    "$type": "number",
    "$value": 0,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 0
      },
      "tokenSync.resolved": {
        "Mode 1": 0
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-1": {
    "$type": "number",
    "$value": 1,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 1
      },
      "tokenSync.resolved": {
        "Mode 1": 1
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-2": {
    "$type": "number",
    "$value": 2,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 2
      },
      "tokenSync.resolved": {
        "Mode 1": 2
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-3": {
    "$type": "number",
    "$value": 3,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 3
      },
      "tokenSync.resolved": {
        "Mode 1": 3
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-4": {
    "$type": "number",
    "$value": 4,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 4
      },
      "tokenSync.resolved": {
        "Mode 1": 4
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-5": {
    "$type": "number",
    "$value": 5,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 5
      },
      "tokenSync.resolved": {
        "Mode 1": 5
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-6": {
    "$type": "number",
    "$value": 6,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 6
      },
      "tokenSync.resolved": {
        "Mode 1": 6
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-7": {
    "$type": "number",
    "$value": 7,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 7
      },
      "tokenSync.resolved": {
        "Mode 1": 7
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-8": {
    "$type": "number",
    "$value": 8,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 8
      },
      "tokenSync.resolved": {
        "Mode 1": 8
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-9": {
    "$type": "number",
    "$value": 9,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 9
      },
      "tokenSync.resolved": {
        "Mode 1": 9
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-10": {
    "$type": "number",
    "$value": 10,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 10
      },
      "tokenSync.resolved": {
        "Mode 1": 10
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-11": {
    "$type": "number",
    "$value": 11,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 11
      },
      "tokenSync.resolved": {
        "Mode 1": 11
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-12": {
    "$type": "number",
    "$value": 12,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 12
      },
      "tokenSync.resolved": {
        "Mode 1": 12
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-13": {
    "$type": "number",
    "$value": 13,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 13
      },
      "tokenSync.resolved": {
        "Mode 1": 13
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-14": {
    "$type": "number",
    "$value": 14,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 14
      },
      "tokenSync.resolved": {
        "Mode 1": 14
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-15": {
    "$type": "number",
    "$value": 15,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 15
      },
      "tokenSync.resolved": {
        "Mode 1": 15
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-16": {
    "$type": "number",
    "$value": 16,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 16
      },
      "tokenSync.resolved": {
        "Mode 1": 16
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-17": {
    "$type": "number",
    "$value": 17,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 17
      },
      "tokenSync.resolved": {
        "Mode 1": 17
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-18": {
    "$type": "number",
    "$value": 18,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 18
      },
      "tokenSync.resolved": {
        "Mode 1": 18
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-19": {
    "$type": "number",
    "$value": 19,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 19
      },
      "tokenSync.resolved": {
        "Mode 1": 19
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-20": {
    "$type": "number",
    "$value": 20,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 20
      },
      "tokenSync.resolved": {
        "Mode 1": 20
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-21": {
    "$type": "number",
    "$value": 21,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 21
      },
      "tokenSync.resolved": {
        "Mode 1": 21
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-22": {
    "$type": "number",
    "$value": 22,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 22
      },
      "tokenSync.resolved": {
        "Mode 1": 22
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-23": {
    "$type": "number",
    "$value": 23,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 23
      },
      "tokenSync.resolved": {
        "Mode 1": 23
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-24": {
    "$type": "number",
    "$value": 24,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 24
      },
      "tokenSync.resolved": {
        "Mode 1": 24
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-25": {
    "$type": "number",
    "$value": 25,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 25
      },
      "tokenSync.resolved": {
        "Mode 1": 25
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-26": {
    "$type": "number",
    "$value": 26,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 26
      },
      "tokenSync.resolved": {
        "Mode 1": 26
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-27": {
    "$type": "number",
    "$value": 27,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 27
      },
      "tokenSync.resolved": {
        "Mode 1": 27
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-28": {
    "$type": "number",
    "$value": 28,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 28
      },
      "tokenSync.resolved": {
        "Mode 1": 28
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-29": {
    "$type": "number",
    "$value": 29,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 29
      },
      "tokenSync.resolved": {
        "Mode 1": 29
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-30": {
    "$type": "number",
    "$value": 30,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 30
      },
      "tokenSync.resolved": {
        "Mode 1": 30
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-31": {
    "$type": "number",
    "$value": 31,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 31
      },
      "tokenSync.resolved": {
        "Mode 1": 31
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-32": {
    "$type": "number",
    "$value": 32,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 32
      },
      "tokenSync.resolved": {
        "Mode 1": 32
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-33": {
    "$type": "number",
    "$value": 33,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 33
      },
      "tokenSync.resolved": {
        "Mode 1": 33
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-34": {
    "$type": "number",
    "$value": 34,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 34
      },
      "tokenSync.resolved": {
        "Mode 1": 34
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-35": {
    "$type": "number",
    "$value": 35,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 35
      },
      "tokenSync.resolved": {
        "Mode 1": 35
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-36": {
    "$type": "number",
    "$value": 36,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 36
      },
      "tokenSync.resolved": {
        "Mode 1": 36
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-37": {
    "$type": "number",
    "$value": 37,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 37
      },
      "tokenSync.resolved": {
        "Mode 1": 37
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-38": {
    "$type": "number",
    "$value": 38,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 38
      },
      "tokenSync.resolved": {
        "Mode 1": 38
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-39": {
    "$type": "number",
    "$value": 39,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 39
      },
      "tokenSync.resolved": {
        "Mode 1": 39
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-40": {
    "$type": "number",
    "$value": 40,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 40
      },
      "tokenSync.resolved": {
        "Mode 1": 40
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-41": {
    "$type": "number",
    "$value": 41,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 41
      },
      "tokenSync.resolved": {
        "Mode 1": 41
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-42": {
    "$type": "number",
    "$value": 42,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 42
      },
      "tokenSync.resolved": {
        "Mode 1": 42
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-43": {
    "$type": "number",
    "$value": 43,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 43
      },
      "tokenSync.resolved": {
        "Mode 1": 43
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-44": {
    "$type": "number",
    "$value": 44,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 44
      },
      "tokenSync.resolved": {
        "Mode 1": 44
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-45": {
    "$type": "number",
    "$value": 45,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 45
      },
      "tokenSync.resolved": {
        "Mode 1": 45
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-46": {
    "$type": "number",
    "$value": 46,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 46
      },
      "tokenSync.resolved": {
        "Mode 1": 46
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-47": {
    "$type": "number",
    "$value": 47,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 47
      },
      "tokenSync.resolved": {
        "Mode 1": 47
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-48": {
    "$type": "number",
    "$value": 48,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 48
      },
      "tokenSync.resolved": {
        "Mode 1": 48
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-49": {
    "$type": "number",
    "$value": 49,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 49
      },
      "tokenSync.resolved": {
        "Mode 1": 49
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-50": {
    "$type": "number",
    "$value": 50,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 50
      },
      "tokenSync.resolved": {
        "Mode 1": 50
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-51": {
    "$type": "number",
    "$value": 51,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 51
      },
      "tokenSync.resolved": {
        "Mode 1": 51
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-52": {
    "$type": "number",
    "$value": 52,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 52
      },
      "tokenSync.resolved": {
        "Mode 1": 52
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-53": {
    "$type": "number",
    "$value": 53,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 53
      },
      "tokenSync.resolved": {
        "Mode 1": 53
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-54": {
    "$type": "number",
    "$value": 54,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 54
      },
      "tokenSync.resolved": {
        "Mode 1": 54
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-55": {
    "$type": "number",
    "$value": 55,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 55
      },
      "tokenSync.resolved": {
        "Mode 1": 55
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-56": {
    "$type": "number",
    "$value": 56,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 56
      },
      "tokenSync.resolved": {
        "Mode 1": 56
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-57": {
    "$type": "number",
    "$value": 57,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 57
      },
      "tokenSync.resolved": {
        "Mode 1": 57
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-58": {
    "$type": "number",
    "$value": 58,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 58
      },
      "tokenSync.resolved": {
        "Mode 1": 58
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-59": {
    "$type": "number",
    "$value": 59,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 59
      },
      "tokenSync.resolved": {
        "Mode 1": 59
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-60": {
    "$type": "number",
    "$value": 60,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 60
      },
      "tokenSync.resolved": {
        "Mode 1": 60
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-61": {
    "$type": "number",
    "$value": 61,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 61
      },
      "tokenSync.resolved": {
        "Mode 1": 61
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-62": {
    "$type": "number",
    "$value": 62,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 62
      },
      "tokenSync.resolved": {
        "Mode 1": 62
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-63": {
    "$type": "number",
    "$value": 63,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 63
      },
      "tokenSync.resolved": {
        "Mode 1": 63
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-64": {
    "$type": "number",
    "$value": 64,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 64
      },
      "tokenSync.resolved": {
        "Mode 1": 64
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-65": {
    "$type": "number",
    "$value": 65,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 65
      },
      "tokenSync.resolved": {
        "Mode 1": 65
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-66": {
    "$type": "number",
    "$value": 66,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 66
      },
      "tokenSync.resolved": {
        "Mode 1": 66
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-67": {
    "$type": "number",
    "$value": 67,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 67
      },
      "tokenSync.resolved": {
        "Mode 1": 67
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-68": {
    "$type": "number",
    "$value": 68,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 68
      },
      "tokenSync.resolved": {
        "Mode 1": 68
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-69": {
    "$type": "number",
    "$value": 69,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 69
      },
      "tokenSync.resolved": {
        "Mode 1": 69
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-70": {
    "$type": "number",
    "$value": 70,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 70
      },
      "tokenSync.resolved": {
        "Mode 1": 70
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-70 2": {
    "$type": "number",
    "$value": 71,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 71
      },
      "tokenSync.resolved": {
        "Mode 1": 71
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-71": {
    "$type": "number",
    "$value": 72,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 72
      },
      "tokenSync.resolved": {
        "Mode 1": 72
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-72": {
    "$type": "number",
    "$value": 73,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 73
      },
      "tokenSync.resolved": {
        "Mode 1": 73
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-73": {
    "$type": "number",
    "$value": 74,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 74
      },
      "tokenSync.resolved": {
        "Mode 1": 74
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-74": {
    "$type": "number",
    "$value": 75,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 75
      },
      "tokenSync.resolved": {
        "Mode 1": 75
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-75": {
    "$type": "number",
    "$value": 76,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 76
      },
      "tokenSync.resolved": {
        "Mode 1": 76
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-76": {
    "$type": "number",
    "$value": 77,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 77
      },
      "tokenSync.resolved": {
        "Mode 1": 77
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-77": {
    "$type": "number",
    "$value": 78,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 78
      },
      "tokenSync.resolved": {
        "Mode 1": 78
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-78": {
    "$type": "number",
    "$value": 79,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 79
      },
      "tokenSync.resolved": {
        "Mode 1": 79
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-80": {
    "$type": "number",
    "$value": 80,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 80
      },
      "tokenSync.resolved": {
        "Mode 1": 80
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-81": {
    "$type": "number",
    "$value": 81,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 81
      },
      "tokenSync.resolved": {
        "Mode 1": 81
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-82": {
    "$type": "number",
    "$value": 82,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 82
      },
      "tokenSync.resolved": {
        "Mode 1": 82
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-83": {
    "$type": "number",
    "$value": 83,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 83
      },
      "tokenSync.resolved": {
        "Mode 1": 83
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-84": {
    "$type": "number",
    "$value": 84,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 84
      },
      "tokenSync.resolved": {
        "Mode 1": 84
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-85": {
    "$type": "number",
    "$value": 85,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 85
      },
      "tokenSync.resolved": {
        "Mode 1": 85
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-86": {
    "$type": "number",
    "$value": 86,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 86
      },
      "tokenSync.resolved": {
        "Mode 1": 86
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-87": {
    "$type": "number",
    "$value": 87,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 87
      },
      "tokenSync.resolved": {
        "Mode 1": 87
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-88": {
    "$type": "number",
    "$value": 88,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 88
      },
      "tokenSync.resolved": {
        "Mode 1": 88
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-89": {
    "$type": "number",
    "$value": 89,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 89
      },
      "tokenSync.resolved": {
        "Mode 1": 89
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-90": {
    "$type": "number",
    "$value": 90,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 90
      },
      "tokenSync.resolved": {
        "Mode 1": 90
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-91": {
    "$type": "number",
    "$value": 91,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 91
      },
      "tokenSync.resolved": {
        "Mode 1": 91
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-92": {
    "$type": "number",
    "$value": 92,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 92
      },
      "tokenSync.resolved": {
        "Mode 1": 92
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-93": {
    "$type": "number",
    "$value": 93,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 93
      },
      "tokenSync.resolved": {
        "Mode 1": 93
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-94": {
    "$type": "number",
    "$value": 94,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 94
      },
      "tokenSync.resolved": {
        "Mode 1": 94
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-95": {
    "$type": "number",
    "$value": 95,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 95
      },
      "tokenSync.resolved": {
        "Mode 1": 95
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-96": {
    "$type": "number",
    "$value": 96,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 96
      },
      "tokenSync.resolved": {
        "Mode 1": 96
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-97": {
    "$type": "number",
    "$value": 97,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 97
      },
      "tokenSync.resolved": {
        "Mode 1": 97
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-98": {
    "$type": "number",
    "$value": 98,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 98
      },
      "tokenSync.resolved": {
        "Mode 1": 98
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-99": {
    "$type": "number",
    "$value": 99,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 99
      },
      "tokenSync.resolved": {
        "Mode 1": 99
      },
      "tokenSync.collection": "Units"
    }
  },
  "unit-100": {
    "$type": "number",
    "$value": 100,
    "$extensions": {
      "tokenSync.modes": {
        "Mode 1": 100
      },
      "tokenSync.resolved": {
        "Mode 1": 100
      },
      "tokenSync.collection": "Units"
    }
  }
}
```
<!-- VARIABLES:END -->
