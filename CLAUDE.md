# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

A seminar document generator. It contains a Claude Code skill that produces two self-contained Japanese HTML files — an internal planning document (`kikakusho.html`) and a public invitation page (`annai.html`) — plus a standalone 2026 calendar (`schedule.html`).

**Tech stack:** HTML5 + embedded CSS + vanilla JS. No build step, no package manager, no external dependencies. All files open directly in a browser.

## Key Files

- `.claude/commands/seminar-doc-builder-0603.md` — the skill definition; this is the authoritative source for document structure, design specs, and generation rules
- `kikakusho.html` — internal planning document (gardening seminar example)
- `annai.html` — public invitation page (gardening seminar example)
- `schedule.html` — 2026 annual calendar with JS date generation

## Generating Documents

The `seminar-doc-builder-0603` skill is the primary workflow. It triggers on phrases combining "セミナー" with "企画書", "案内", "運営資料", or "案内ページ". When triggered, it reads the skill file for the full spec, then creates `kikakusho.html` and `annai.html` from scratch.

Always follow the skill file's spec — do not improvise structure or deviate from the color palette.

## Design System (enforced across all files)

CSS custom properties that must be used consistently:
```css
--navy:   #16365C   /* headers, emphasis */
--gold:   #F4A300   /* accents, badges, buttons */
--bg:     #FBF8F1   /* page background */
--white:  #FFFFFF   /* card backgrounds */
--text:   #2D2D2D   /* body text */
--muted:  #5A6370   /* secondary text */
--shadow: 0 4px 16px rgba(22,54,92,.10)
```

Reusable component classes: `.badge` (gold circle, 36×36px, white text), `.card` (white, 16px radius, shadow), `.btn` (gold bg, navy text, 50px radius, hover lift), `.checklist`, `.section-heading`.

## Conventions

- **No external resources** — no CDN, no image URLs, no external CSS/JS; everything embedded in `<style>` and `<script>` tags
- **Currency format:** `X,XXX円` (comma thousands separator + 円, no space)
- **Date format:** Japanese `YYYY年M月D日（曜日）`; pick near-future weekends (3–6 weeks out)
- **Footer** (mandatory on all pages): `Produced with Claude Code ✦ 主催：[organizer name]` on navy background (`rgba(255,255,255,.45)` text, organizer name in gold)
- **Section headings:** always `<div class="section-heading"><div class="badge">N</div><h2>Title</h2></div>`
- **Responsive:** `@media (max-width: 640px)` breakpoint; fluid typography via `clamp()`

## Git Workflow

Active branches:
- `claude/claude-md-docs-6SPai` — current working branch
- `claude/gardening-seminar-materials-ObLRM` — previous feature branch

Typical flow: edit/generate files → `git add <files>` → `git commit` → `git push -u origin <branch>`. Never use `--no-verify` unless explicitly instructed.
