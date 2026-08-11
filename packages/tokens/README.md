# @spec-kitty/tokens

Design tokens for the Spec Kitty design system — distributed as CSS custom properties (`--sk-*` namespace).

## Installation

```bash
npm install @spec-kitty/tokens
```

Then import in your CSS or JS entry point:

```css
@import '@spec-kitty/tokens/dist/tokens.css';
```

Or in JavaScript:

```js
import '@spec-kitty/tokens';
```

## CDN

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@spec-kitty/tokens/dist/tokens.css"
/>
```

## Usage

All tokens are CSS custom properties available on `:root`. Reference them with `var()`:

```css
.my-component {
  background-color: var(--sk-bg-2);
  color: var(--sk-fg-1);
  border: 1px solid var(--sk-border);
  border-radius: var(--sk-radius-md);
  font-family: var(--sk-font-sans);
  font-size: var(--sk-fs-body);
  padding: var(--sk-space-4);
  transition: background-color var(--sk-dur-base) var(--sk-ease-out);
}

.my-cta {
  background-color: var(--sk-color-yellow);
  color: var(--sk-accent-fg);
  border-radius: var(--sk-radius-pill);
  box-shadow: var(--sk-shadow-glow-yellow);
}
```

## Token Catalogue

The full list of `--sk-*` property names, grouped by category, is available in:

```
dist/token-catalogue.json
```

Or via the package export:

```js
import catalogue from '@spec-kitty/tokens/catalogue';
console.log(catalogue.categories.color.tokens);
// ['--sk-color-yellow', '--sk-color-yellow-soft', ...]
```

To regenerate the catalogue after adding tokens:

```bash
npx nx run tokens:catalogue
```

## Token Categories

| Category | Prefix | Purpose |
|---|---|---|
| `color` | `--sk-color-` | Raw brand palette values |
| `tint` | `--sk-tint-` | Tinted card/panel surfaces |
| `on` | `--sk-on-` | Foreground color for tinted surfaces |
| `bg` | `--sk-bg-` | Page and surface background scale |
| `fg` | `--sk-fg-` | Text/foreground scale |
| `border` | `--sk-border-` | Border colors |
| `font` | `--sk-font-` | Font family stacks |
| `fs` | `--sk-fs-` | Font size scale |
| `lh` | `--sk-lh-` | Line height scale |
| `fw` | `--sk-fw-` | Font weight scale |
| `space` | `--sk-space-` | Spacing scale |
| `radius` | `--sk-radius-` | Border radius scale |
| `shadow` | `--sk-shadow-` | Box shadow tokens |
| `dur` | `--sk-dur-` | Motion duration |
| `ease` | `--sk-ease-` | Motion easing curves |

## Semantic Pairing Rule

Always pair a `--sk-bg-*` or `--sk-tint-*` surface token with its corresponding foreground token to ensure accessible contrast:

| Surface token | Foreground token |
|---|---|
| `--sk-bg-0` through `--sk-bg-3` | `--sk-fg-0` through `--sk-fg-4` |
| `--sk-tint-mint` | `--sk-on-mint` |
| `--sk-tint-butter` | `--sk-on-butter` |
| `--sk-tint-lilac` | `--sk-on-lilac` |
| `--sk-tint-sky` | `--sk-on-sky` |
| `--sk-color-yellow` (button bg) | `--sk-accent-fg` |

Using a surface token without its paired foreground token is a contract violation under ADR-003.

## Light Mode

Add `data-theme="light"` to `<html>` or wrap content in a `.sk-light` class to activate the light mode token overrides.

## Linting

This package is excluded from the Stylelint `--sk-*` enforcement rule (it defines the tokens). All other CSS files in the monorepo must use `var(--sk-*)` references rather than hardcoded values.

## Design Reference

Token values are derived from `docs/architecture/decisions/ADR-003-addendum-token-values.md` and the design reference in `tmp/Spec Kitty Design System(1)/colors_and_type.css`.


## Fonts

This package does **not** ship font binaries and does not declare `@font-face`.

It previously vendored 30 Falling Sky and Swansea `.otf`/`.ttf` files with no licence file
anywhere in the repository, published via `files: ["fonts/**"]`. That is not something we can
redistribute through a registry on an unverified licence, and it was the wrong typeface
regardless: Brand Book v1.1 specifies **Inter**, which is what the shipped TeamSpace product
uses.

The font-family tokens name Inter with a full fallback stack, so a consumer that loads no
webfont still renders sensibly. Loading Inter is the consumer's job.

The binaries have also been **removed from the repository**. This repo is public, so leaving
them in the tree while merely un-publishing them kept 30 unlicensed commercial font files
publicly downloadable — the packaging was never what created that exposure. Removing them
from `HEAD` does not rewrite history, which is a separate problem.

Inter has no condensed, extended, outline or "boldplus" cut, so `--sk-font-condensed`,
`--sk-font-extended`, `--sk-font-outline` and `--sk-font-boldplus` all resolve to Inter. They
are kept as names so consumers do not break, but they no longer express a distinct typeface.
Use weight (800/900) where `boldplus` was doing display work.

## Publishing

Published to **public npm** as `@spec-kitty/tokens`.

```bash
npm publish   # publishConfig pins registry + public access
```

Public npm rather than GitHub Packages, for a reason worth keeping: **`Priivacy-ai/spec-kitty`
is a public, open-source repository.** A private-registry dependency would break `npm install`
for every external contributor, which defeats the point of core being open. GitHub Packages
also could not host this name — `npm.pkg.github.com` requires the scope to match the
repository owner (`Priivacy-ai` vs `@spec-kitty`), so it would have forced a rename.

There is nothing to protect by keeping it private: this repository is already public, and
`spec-kitty-cli` already ships on public PyPI.

## Relationship to the shipped TeamSpace token layer

`spec-kitty-saas` has its own 590-line token layer at `assets/styles/spec-kitty-tokens.css`,
guarded by `test_design_token_adoption.py`. **It is not the same architecture as this
package** and reconciling them is tracked separately on `spec-kitty-saas#725`:

| | this package | spec-kitty-saas |
|---|---|---|
| Values | hex (`--sk-surface-page: #F8F5EC`) | HSL triplets (`--sk-surface-hsl: 45 20% 98%`) consumed as `hsl(var(--sk-*-hsl))` so Tailwind/DaisyUI can compose them |
| Base mode | **dark**, with `:root[data-theme="light"]` override | **light**, with `[data-theme="dark"]` override |

Note that #725 originally asserted the opposite of that last row. It is worth measuring
before acting on it.
