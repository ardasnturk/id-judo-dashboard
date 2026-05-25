# CLAUDE.md — ID-Judo Classification Dashboard

## Project Overview

A single-file, no-build standalone web app for classifying athletes with intellectual disabilities (ID) in judo. Deployed via GitHub Pages.

**Live URL:** https://ardasnturk.github.io/id-judo-dashboard/

---

## Architecture

- **Single file:** All logic, UI, and data live in `index.html`. There is no build step, no `package.json`, no bundler.
- **Runtime stack:** React 18 (UMD) + ReactDOM + Babel Standalone — all loaded via CDN (`unpkg.com`). JSX is transpiled in-browser.
- **Deployment:** GitHub Pages from the `main` branch root directory.

---

## File Structure

```
id-judo-dashboard/
├── index.html   ← entire application
├── logo.png     ← logo used as favicon and header image
├── README.md
└── CLAUDE.md
```

---

## Key Implementation Details

### Scoring

| Class | Score Range | Meaning |
|-------|-------------|---------|
| WK 1  | ≥ 85 / 114  | High competition readiness |
| WK 2  | 50 – 84     | Limited competition readiness |
| WK 3  | < 50        | High support needs |

- `MAX_SCORE = 114` — sum of all question weights
- 30 questions total, 10 per section (A, B, C)
- Each question has a fixed weight (2–5 points)
- **If you add or remove questions, recalculate `MAX_SCORE` manually**

### Translations (`T` object)

Three languages: `de` (German), `en` (English), `tr` (Turkish).

Each language entry must include:
- All UI strings (`title`, `subtitle`, `ja`, `nein`, `calculate`, etc.)
- All reading mode strings (`readingBtn`, `qLabel`, `ofLabel`, `repeatBtn`, `exitBtn`, `backBtn`)
- `sections[]` with matching question count and weights

**When adding a new language:** duplicate an existing language block, translate all strings, add the language code to the `["de","en","tr"]` array in the toolbar.

### Reading Mode

- Triggered by the `📖 Lesemodus / Reading Mode / Okuma Modu` button
- Uses `window.speechSynthesis` (Web Speech API) — no external TTS service
- TTS language codes: `de-DE`, `en-US`, `tr-TR`
- Each question triggers TTS automatically via `useEffect([rmIdx, rm])`
- Result is spoken via `useEffect([rmDone, result])`
- If `speechSynthesis` is unavailable (some browsers/contexts), the UI still works silently

### Accessibility

- All interactive elements have `aria-pressed`, `aria-label`, or `aria-labelledby`
- Keyboard navigation: `Enter` and `Space` activate all buttons (`onKeyDown` handlers)
- Focus ring: `*:focus-visible` CSS rule with high-visibility orange outline
- Result region uses `aria-live="polite"` for screen reader announcements
- `document.documentElement.lang` is updated dynamically on language switch

### High Contrast Mode

- Toggled via the `◑ Kontrast` button
- All colors are controlled through the `c` (color tokens) object — never hardcode colors
- `resColors` also has high-contrast variants for result cards

---

## Important Constraints

1. **No build step** — do not introduce `npm install`, Vite, Webpack, or any bundler. Keep everything in `index.html`.
2. **Single file** — avoid splitting into multiple JS/CSS files. The entire app should remain self-contained.
3. **CDN only** — dependencies are loaded from `unpkg.com`. Do not add local `node_modules`.
4. **Mobile first** — the app is primarily used on phones. Test all changes on narrow viewports.
5. **`MAX_SCORE`** — always update this constant if question weights change. It is used for the progress bar percentage.
6. **Answer array is always length 30** — tied to question count across all sections. If sections change, update the initial `useState(Array(30).fill(false))`.

---

## Common Tasks

### Add a question
1. Add to the relevant section in all three language blocks (`de`, `en`, `tr`)
2. Recalculate `MAX_SCORE`
3. Update initial answers array length if total question count changes

### Add a language
1. Duplicate an existing language entry in `T`
2. Translate all string keys
3. Add the language code to the toolbar `["de","en","tr"]` map
4. Add a TTS language code mapping in `speak()` and `buildResultTts()`

### Change scoring thresholds
Update the `calcResult` function — the `score >= 85` and `score >= 50` conditions.

### Change the logo
Replace `logo.png` in the repo root. No code changes needed.
