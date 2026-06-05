# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Japanese seminar document generator** — a zero-dependency, no-build-step project that produces self-contained HTML files for seminar planning (`kikakusho.html`) and participant announcements (`annai.html`). Everything runs directly in a browser without a server.

## No Build Tools

There is no `package.json`, no compiler, no bundler, no test runner.

- **Run**: Open any `.html` file directly in a browser (double-click or drag-and-drop)
- **Lint/Test**: None configured — validate visually in a browser

## Skill: Seminar Document Builder

The primary workflow is the custom skill at `.claude/commands/seminar-doc-builder-0603.md`. It triggers when the user asks for セミナー企画書, セミナー運営資料, 案内ページ, or related phrases — **not** on "セミナー" alone. It generates two fixed-name files:

| File | Purpose |
|---|---|
| `kikakusho.html` | Internal planning doc (7 sections, badge-numbered) |
| `annai.html` | Public participant announcement (hero → points → FAQ → CTA) |

After generating both files, commit and push: `git add kikakusho.html annai.html && git commit -m "Add seminar docs: [theme]" && git push -u origin <branch>`.

## Design System (must be followed in all HTML output)

All CSS lives inline in `<style>` tags. No external CSS, JS, CDN links, or image URLs — ever.

**CSS variables (`:root`)**:
```css
--navy:   #16365C   /* headers, text emphasis */
--gold:   #F4A300   /* accents, buttons, badges */
--bg:     #FBF8F1   /* page background (cream) */
--white:  #FFFFFF   /* card backgrounds */
--text:   #2D2D2D   /* body text */
--muted:  #5A6370   /* secondary text */
--shadow: 0 4px 16px rgba(22,54,92,.10)
```

**Layout rules**:
- Cards: `border-radius: 16px`, `box-shadow: var(--shadow)`, white background
- Section headings: left-aligned gold circle badge (36×36px, `.badge` class) with white number
- Font: `"Hiragino Kaku Gothic ProN", "Hiragino Sans", "Noto Sans JP", "Meiryo", sans-serif`
- `font-size: 15px`, `line-height: 1.7`
- Responsive via `clamp()` for headings; media query breakpoints at 900px and 640px

**Mandatory footer** (both files):
```html
<footer>
  Produced with Claude Code ✦ 主催：<span>【架空の主催者名】</span>
</footer>
```
Footer: `var(--navy)` background, `rgba(255,255,255,.45)` text, `<span>` in `var(--gold)`.

## kikakusho.html Structure

Seven badge-numbered card sections in this fixed order:
1. 開催概要 — 2-column table with gold left-border on label column
2. 背景・なぜ今やるのか — 2–3 sentences with stats/trends
3. BEFORE → AFTER goals — two-column layout with center `→` arrow (gray box / gold box)
4. プログラム — 3-column table (時刻 / 内容 / 所要時間), navy header, alternating row shading
5. こんな方におすすめ — 3 checklist items with gold ✓ badges
6. 持ち物・注意事項 — 2-column grid with gold-underlined subheadings
7. 備考・今後の展開 — 2–3 sentences on future plans

## annai.html Structure

1. **Hero** — navy-to-gold gradient, `::before` radial glow, title + meta badges + CTA button
2. **3 Points** — `HIGHLIGHTS` label, 3-column card grid, each card has `border-top: 4px solid var(--gold)` and theme-appropriate emoji
3. **Separator** — 1px gradient line (`transparent → #D9D3C5 → transparent`)
4. **FAQ** — 3 Q&A cards, gold badge on Q, navy "A." label
5. **CTA** — navy background, `<em>` highlights in gold, large `.btn.btn-large` button

## Output Conventions

- Prices: `X,XXX円` format (comma-separated + 円, no ¥ symbol)
- Dates: near-future weekend that fits the theme
- HTML section comments use `<!-- ── SECTION NAME ── -->` delimiters
- CSS class names in English (`.section-heading`, `.point-card`, `.break-row`); file/content language is Japanese
