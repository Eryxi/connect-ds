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
Compiled from this file's Variables section (preview fingerprint 7444c951).
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "border radius",
      "tokenSync.figmaKey": "ba4ecce6538ed8d484f57679727e3360a2f2d616"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "border radius",
      "tokenSync.figmaKey": "4f8b6468c653fb9bfe7629f8ef8a7746004b0d4b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "border radius",
      "tokenSync.figmaKey": "6f15dece1efdac94210e80f350174f0d7834c0c0"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "border radius",
      "tokenSync.figmaKey": "a047b99c190ef516018d81f64bba021bd1c02e39"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "cc5fc63c75cf79e22379d014cbac222554ece198"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "5acbfacd74bb254ddf623090539f33fc50fb6d86"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "e66cb674f92bf26bada87029348aa4efdca59458"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "cba18371bba9e22d6ec1def758e13bc28fa52279"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "fd601d40adf51b6d1b54529dea4ec3df906f69e0"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2f160f6c88266d74102a3036119c5b4524a58d0f"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "81aa7530ce06314928d1ac9878f4bdbf5c2cfe85"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "c065f92a6f50dec1345d54991bc7a63cd67ab197"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "a719eb41ea77193aa0e5e911c41c1939343082fe"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "e75a83eb894e61c88688029715fac53a1773da95"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "a6a6786f211467f659b6056453a574a5b0a9b9e2"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "caf0b869e14fa920922a941f160130e54296af0b"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "65e7895df37b4720564a5b4616f324f5073830cc"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "5735fc0576776f81a0992ec24cc0124ec86c161d"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "e6d7060bbe6d03f277e6a2a444270c7481eb7929"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "61ad4197bd0b11fc2383fa9e078afe1ca9c693d2"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "675f286568c00ccf9d985caa282ae1ff2fcacec4"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "dbbbcb9b3c6cf7887a246aa55485b7bb0b2e29ec"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "3c85374ab9c55b168354d67c7ba0f153725da84a"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "baec5e9ad550594d61171db87c77f94d3f1e0829"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "c8db1f1c2a4f7f4e8d3dfbe6ed910e4e44771406"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "a907001d13d93f24844514d85016861160bba50c"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "5fcec084ce24e7f88eaaa76b61a55c8fb782e448"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "76f66417773255f1233a96ea47a948dbd3972e43"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "0d0f1a3d7939640961f265a61a1cb78459bea90d"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "16b8e55e0c4ea4da533d305d908df0c9da01c4da"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "11fde572b2107aa01f48c140ee1188e702c671ed"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "7960a3ff0a6c2230d21d4312d91de3b615c844bc"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "30cad4be820d1d22da881c5396b9566d452c19f8"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "9aa242a12b29d87b752c2039df2e304630ceef06"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "34a4fa247413aae0808c5c9141e8468f64936671"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "b2764cbdf7b353376da08f2b8b2a55814471b502"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "fe19f6321f215e08c37fcb696f4020c5b48d7540"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "be7671b4e3e5b946be3e4114b5f8758698a4522f"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "20ada8499e38278c759262939f17d90c1a943d12"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2675f3aab40674dedb83162366fd6fa2e208a09d"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "f62e0b4e350d1f64881cfb683bb8274f92daef92"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "5657ecf791f2122e8f85369e0e368fcd56db5a63"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "cb5064b5f4e259785c38a56e711b12f5605ca5ff"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "8757bb567fb763245b8d7ff18f3362ca5c8d16e0"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "375091d531ba2374c034926469942980c5b43aad"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "f1a84345f5edca21863e9c867ca430290f95d50f"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "b69721858689ef58d9f6e45df774e2f7f1aeaa61"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "cbb9d505b06bd1d04f877c9afcde7c729176b86b"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "31d968eea010deebc4f47fb9e92a422c36016db7"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "c41eaa28f88e0ee947da1a55f03bc1934ec75f6a"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "e40657d60815bd06f7c8a52e234b8f9edceba08f"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2fa949f5fd155407f074d258fd5b157f0fd111ef"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "5727c02e603ad61761b979417f818bc0e9fee7d3"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "0604f5aab54e29c26411761c43410bb5e62744c0"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "0441c52f8358f06df12fcbd58cdf3e8cd48ef7dd"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "7f5f9d296916bab684eb6bb8b328fb359313b755"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "8e09c93011af9262b209d080145950697a210826"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "74c50192d0a6c27a1eda4d5870d8df30c1328b70"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "dfbcecd52c22e18472fad2d634acd8596481aa1a"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "fd2c3496e85b7baf9657995b59eb0dbd78a47b19"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2daacfabc311786b1b6f7488ffdd37779f766ae9"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "6c560baf18f00f2990bfcf5691fe556df5166dc3"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "5c30abec848df66da5a9a3ac7b880882574c4c17"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "21d9014b59c27139f94415089fae3f3d6bc9dbf5"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "baad60ca860dcab38fd3298ca04d03d6d1083e3f"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "a6a137ee1dffe9a6d975c28799e26dbfd19f7892"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "665ffbf20457c31690afa5bcf9bfb5996ea447f4"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "246a6f4eefbdd4746bddc5c112f833626a08ed68"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "8336a8ab8cff95246eeb7d09d04f0ac0344f4850"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "48f39b65b52f6cfe8985f752848bc16eca354692"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "b6425e1ab19a4b59f327fa4676460f855235b032"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "51b2547a9c5ce5370cae74364bb230ec3e14f2a5"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "68b11cdda3ca8a9958213ee8489ccf6bce335d5a"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "18bfe766c76902bca35fd76dc6cf8ba43e820e9e"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "f21833ecd3b95c6c7dbe85ee2f3ae55d64290266"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "9c6155f04e709b25cee7a9d6d3b08a20f1f474b0"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "00b727a17848392e93ff1680a5a2f93ed4c88d75"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "4d0c63ab9ddc606e98ddf0a45c65ae693273df28"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2e176145eed89dcddcebdb392e2bd3f03f5e2e50"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "af35c898a3a4f701fbef0ea0ffca4e2ccdec5ea0"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "1aec42a1dd56191e312b00be0fe6b56aa7e6ccf8"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "86a5c816aeac6f64df169beb01a8cf0c03c20356"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "4c9136cbbe6311111ac57e38e899cacf4fc0cc98"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2f694c48ddc2bddb2a5991a93b651e3ed3416fa9"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "bf224eabd688fe4f7440f1b82e075b4e55c80395"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "7e92d0e90dbbd68b6eb4b76becf1260b8de05fe2"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "cc6080270858e59895a6f8d95d3e60df38a28636"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "15dd61a22e9896deba916413c976bf9aee5ba522"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "0a1e448f0fb19a583b9b56a36dc8f45b1142c609"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "6e27a15f2aed8fabd26794b353cd7c9bcc06f966"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "930bbdfc4cb306d0537a9233b7c75a72bf9434b9"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "a66909a6ce3f82749ec99f2463bc9de24a3525c9"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "45568b7f3e3614941067754623bf6b5bbb27e014"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "56b11ac9fb46735bea059f972a158cb65b004632"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "10587e5ae63f945b10bdc211cd360a334cddbf6c"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "d6241145236f85e5bcc5063981a0f789ec5414c9"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "df5119eb1bee0830e1d18f28b5661ea03c558572"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "58eef305d94bd4ca411fc9d53ef87448a2826e2f"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "2dc61d949554ade37ee21a3500ffd84e623b75ee"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "9d1fd9f71161a869302a0472e6143db33abc319d"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "28bbf2e38004d5858a4f4b2010e0cbfd7ce0f081"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Colors",
          "tokenSync.figmaKey": "24cc3b6cefa99a3f41c37891937e7eed51be8171"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "a6ed08c84fbba550866c28287edcfbb2f0352818"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "f86a82e070f789bf045da44ede40530f993e8386"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "a3537654a8dae552f87c04a699205e1c4a4f33e2"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "e2db87f19417bff105a8a8dd441ef1b2ec1217e3"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "d0c83c36af039074d523b614f0bafce43742590f"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "0df92dec07a3ae007c30d89a9f35ff5850098c9d"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "cb2ce8f24b8788f922880ee4b3c60668ab542218"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "80a62bd2314c037d72e50e968dc265176e7b886a"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "9c704a34b470ebafca2da56ac03040e3bc5d76fe"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "f7edbe9bde217e1d3c7b7c73e08cbb6e371e5cae"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "079341cae12bafcbb5621f91197b055bbd3b6461"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "c216f1381ec916ab429a97449728a86ca3b7ff81"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "2dacdffef09526dc1186fc357fe132b1db1c3e88"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "9f22ad820cc950c4dbfae6c1c996a67f952745d3"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "fb465de122c2d7bfb05c350fa7fc7c943052cf21"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "a1afeae55a403e1af83e70df8ef5fc4c1b34c0b9"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "16b5eaada836efce8c501a7b35a50fb288dd21b0"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "5d9bb6a0b8ca9ff777037fdeb9ac6bbc4d0f1c24"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "039018bfcccceb8af446bfdb1c516eeaff0516b6"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "sizes",
      "tokenSync.figmaKey": "82d5c21e23184b534d9e494cccb6b7d5ac34375f"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "825dbf1ea21cdaaf095414de80cb40b491f40cdf"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "b1b0909987ca6226e51a196b5fb204e0df9d066a"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "e5c86d7a465df2256a7b559cf061b301099fd93b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "bc51cd2981b406c405173d7a664248440ef7cc83"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "203bfef129962eb665820ab32203a331a61ee1d5"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "ba107607c72a74c4baa1c4b1cb8dbecf66697248"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "40e4e38d6a49d03890503295426b9a9ba72a05bc"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "f879ef7f27e8abc22712a1791a3d74dca0ec5cbf"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "168d95ba116a70a9ba4970ba608eccc49fa73f10"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "da15e92a10a57189352fd86bf7990af1dba75a26"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Space",
      "tokenSync.figmaKey": "fe575526fa996dbc965218e1582d32f003acb6a3"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "8af593fe8abd6fa2e0a7999d51d7440bfc97ba8e"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "520a8c8f856e5f0877746191f2a765fe2cf40ace"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "4eff3746d079e8fe17b9d85a5f83656db88207dd"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "76509a2d19ced0af24641d3e93cef0eee8dd7621"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "9cce901a399e8ba8e01fc9b9a07f255f037bbfd2"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "f3f5d524d9662610974471d05182f1846ff5953e"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "032f2c319063c7f0b6f50412b1d9145f2480aa17"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "04c3fe66bd26688c3154c8ff6469b47b767bfcfd"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "33ceecae8fdae7210c39b1462bf8e07b07391f6b"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "22cbe1a443e91c806f0ea08c94945faed2cbc16d"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "965bfe3e89a7edf9dc2f2f0a83c2ae31cc7721bf"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "f80e45153c622b53f145adaebc495eb01c1d1367"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "439c21f4d09d377fb33c6be355281dab6d3b863b"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "242c1d5e0e9dd8bc16769e2906df81fe780cd5ab"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "46ae7320df368e09c6539b0d9bd507997464828a"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "7ca59815fbe53bbfddd8e4e1587f62b7712262df"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "a1b6c248db8278a80a8cf178b02a56cceb5f3097"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "11b59b0cba739c6087b92e61adbb985b6ea618e6"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "13b866a5c64fa0ad38be32d3de003f21b20324bf"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "df3de40bfcd2377e4f20eaede5f492ebc8a25178"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "9131be007597113874bfcac17b9b577e53f751bb"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "524350321466bdaf025dcb10d1ba914f27671964"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "2a4f501de32114f651bda34028163c9a3a2e3222"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "3782564853179a6407bfbb714ba4caccf65d1ff1"
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
          "tokenSync.scopes": [
            "ALL_SCOPES"
          ],
          "tokenSync.collection": "Typography",
          "tokenSync.figmaKey": "94ccde5c0b6ea8cb1d38f04871c12b56c03fc84c"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "b724bef43d1d21e857e83912bd8b0e082f99c5a6"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "60f16dccfa34fed8c12dfe8a4ce2be52c6ca08a0"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "447cfafe9e703dae1b02440ca24bb0cfa7be3f7b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "980400163c6086d3c38072a478d64beefe5dfd3f"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2b44099f218cd6fe183d9bdfb7a095d28c2065bf"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "d15c89e03ddd0c9be3c9ed1bbf0853f355079aa5"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "7c26efa2be310b12851327203157f3c342728937"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a763f93b1d5d8b6644ffea60485aae04ccdf0c42"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "0dc57431accde4f0ac540326dfef3e4133c06ded"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "0994b5c673453bebc7e7261c731b03807bc7ad84"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "60ef8a5e5390001e578d1fb1261dc548278d326d"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "78e846b090619cb9853268cfe866bdf2086983d3"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "5c97d0b688db9ad39073d27f664bbd9ceb4b6041"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2d30280f279c36d6b099bd3d471b32339743229a"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f043f21d8a63f3d9ba78c2415bd8a00e965197a4"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "1061293a5b6e63f24d5d899bfcc4ca8aede25b4b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "0b7b339679fc7dc11cdd3699bf978f27726d2f88"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2151c180d1c025f8f60ed8ca365f6cd157ca435d"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "b12008a02c945a1da583775b42daa5a248aacf74"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "0886f2dc83eb275457999ca6873f41a10e0c9338"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "6473be971a1202661483667c9db3d0a3319c7f8b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "fc1365b2d658ea7eb60f9ee443741488acfd01b6"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2477448597d05de6daa9f44231df7b897e82f000"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "3d42f2f32a09308884f4a57137c9401ed736c544"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2042e423869ff884c3f4798bae110accbdca5485"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "854dc6f4b1ea512c0ab209e552f055364aaaa55e"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f340a2429c29ebe3d635bbdb12e4b9204e44a317"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "ffac2e72b67a8deed76175ed8eba74f98244e10f"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "41228cc043caeb7d3c993b486a1e79bd731a98b0"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "065226d8577fa3f8d248436cab9bbb13fa1d0bd7"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "22c759931c1c24dde1b3d2d8d02617036914107c"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "e602c2f6a3b7833ef22608053e84cfcea034bbca"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "ab8e20c47a2fbe08548e6d78d3bf23ef83464ba6"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "24087892f1b634d75f6789c85a053d070b6f0423"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "7b2a077e783682b371a89c64d89f81f2a4a01e53"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "3e7c052ca821d524e5253ef8b3299b2eb81097c5"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "d05f70b6910a0a9ffbdd5f0849d36a2ee25e543a"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "7f8783318e2c6d60631f8709a6b63acb4f2e54f9"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "d5a2cb39dbddc4bd19f3e8467e2cde3486cca1a1"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "6bdeaa4343d4d76d1a9a8fbd438b4718a5f8b67b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "c5309e85d0d86b74ade63b8b7d5f14cf2b89e573"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "d2627a16c9d54e5dd758741c1a2407041f9a7444"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "9c72a21d97a92daa830d584bb3d516621696afb3"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "42f9273fe69543596bfc5b9baa236bd2362523d7"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "1e4cd800514b88e826277980ebfff4399e8abd66"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "65654eb27f5ef3987329b3fcbbbe0435b7edf557"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a1b59723f111d0b9b7e10b9b3087c167d5573dd4"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "baff08fb3f32738c77005ad04dffca341687dafe"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "ea59557b124773083647e797280f903eab2d6649"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a5e9250a50d4427d784640bf637b5e8841bde364"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "458d6c0e795d243d87ce3c07ce3639c83f10c613"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "8e1e3ce828273ead25ce299e36e28d3281ce28aa"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "39d9369ffdc21db748f5630ae0bc0c7cff5082a7"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "e1ce74a0e13006cb7b7c2c27c5782b08c963b444"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "ccb7d5b1bdc717bfac6d8af6544dbbe9e82fd2e5"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "28f647b9afebcd783c6fb687a80d712d0890d101"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a9e41f450cdc174519118f0676abb4582052f31e"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a2173a2bb9f68110b69a81ee3d744081a9ea0ec5"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "ec07ca0c256cf79d7dc17c4056a275f2cd6d8e2c"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f7c012ea8ae58735342ff51781792928c3208cff"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "04cbf0852fef7c3696325591f11846079b7fede4"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "626b4ec6c1b07cadeb5b998d488fb747afdb2d19"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2afffcc1eef008eee90b5e9d4d77b595edefc10b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "db60389403e8455dac49745bcfd450a9cdeb4da3"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "53062816f1e48096bea22897b9def076d4df36f4"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "47506b0dad46df1bf8ca758f3933ee64e3daa47e"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "67248ebcd6e93555006f2cf9712848b89a6e67b5"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a00b4249cbdfb9d450f5505ddf8fc8b93789f8cd"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "764dd87c3bcb8167f7cff5d1675f23f8025a2244"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "86121a91d7e3f7582c12564f27e7e81467dc7985"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f14f2d5d1ff568e161cab118dba50010e3071b02"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "d57aed5b731da743067cfc88bb98f1aa80386d52"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "41d400914d73b9d6894fee2cd9e9c0bd15d92f1b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "8e74f30b6958cf64d2c654e79bc8f5b53fb923ea"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "68c83f58524d9006f974632a09fd117b1029c062"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "0b06a18cd8fbd303dceed5c4037deaa099e48fa7"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f5af045556c68f1621d66c932992fde8b3573c80"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "7952f2e1cdb4ad567c817ae01a2d25edde1cad46"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "0cae1b71ae42d17d389e2606344591a20400fa51"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "4c20336b8cab0c6ea5cc9e2dcf154a6118a48e68"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "8cc7999734d1e246615b6a2351ab1747cb36de89"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "4105c3fa6e7d6aa80ba797ce727424f9a3c6bbc3"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "b8542cfa6f1c34349e21a4c00a5f3bb0b74c5414"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "2286739b445091d459d2b6fade839a50e6c80ca8"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f72698b776b4f3087fb1b4df9502379d05c83f7a"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "186fc83b062a04a9cad1c5a74605fa9d2c2d4b43"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "51997a52fbcc2071b6fe48dbe2eb8bd173251745"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "63a8c66fbcef7f5e54a396916582d464a88efb9a"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "f17d82d53d8f282b540b3e1815b4efbe7cbb8bf1"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "fbb16c5d05d11a95b31158beb1e5f5fb5039c302"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "be5ce8c46f2e3339c8c11970633e5dd84ebdc11b"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "eb5ccb4772794c2fde4dca87084215c547179169"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "89616e7617738c0b29dbb84a1446654e31e7495d"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "e8ac813628ecc35e78d8f2d7e35a3b22f4113668"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "180a2c313357d4c6419a0dcd999e58f7f4c53a66"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "a6dda629da90d82eee019ac0ee4db02261d19dda"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "9a575a57d0f2c40a73e8c9c64c968816d839e4ed"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "9a969504d55e9cd974289f50cc4f633ecc05c531"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "e6df3ae1e8650e0027507e354eac1133f8138291"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "09ae1ece14e3ae43a6a3366bb53464622e607f08"
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
      "tokenSync.scopes": [
        "ALL_SCOPES"
      ],
      "tokenSync.collection": "Units",
      "tokenSync.figmaKey": "6ecd6ddeb9ec974408b43aaf6fd45e64b1c7e5e2"
    }
  }
}
```
<!-- VARIABLES:END -->
