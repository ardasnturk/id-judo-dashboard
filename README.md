# ID-Judo Classification Dashboard

A web-based assessment tool for classifying athletes with intellectual disabilities (ID) in judo competitions. Built as a single standalone HTML file — no installation required.

**Live:** https://ardasnturk.github.io/id-judo-dashboard/

---

## Features

- **30-question assessment** across 3 categories (Motor, Cognitive, Social skills)
- **3 classification levels:** WK 1 (high readiness), WK 2 (limited), WK 3 (high support needs)
- **Reading Mode** — full-screen guided mode with automatic text-to-speech, one question at a time, large Yes/No buttons, auto-advance
- **Multilingual** — German, English, Turkish
- **Accessibility** — keyboard navigation, screen reader support (ARIA), focus indicators
- **Font size control** — A− / A+ buttons (4 sizes)
- **High contrast mode** — for low-vision users

---

## Tech Stack

- React 18 (via CDN, no build step)
- Web Speech API (TTS)
- GitHub Pages

---

## Usage

Open the live URL on any device. No login or installation required.

**Normal mode:** Answer all 30 questions, then press *Calculate Classification*.

**Reading Mode:** Press the 📖 button. Each question is read aloud automatically. Tap ✓ or ✗ to answer and advance. Result is announced at the end.
