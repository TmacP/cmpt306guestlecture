# CMPT 306 Guest Lecture Slides (Marp)

This repository uses [Marp](https://marp.app/) to author presentation slides in Markdown.

## Quick Start
```bash
npm install
npm run marp:watch
```
Open `slides/deck.md` in VS Code. If you install the "Marp for VS Code" extension you will get live preview and export options.

## NPM Scripts
- `npm run marp:watch` : Watch `slides/` and auto-generate HTML into `dist/`.
- `npm run marp:build` : One-off build of all Markdown in `slides/` to HTML.
- `npm run marp:pdf` : Export `deck.md` to PDF.
- `npm run marp:pptx` : Export `deck.md` to PPTX.

## Directory Structure
```
slides/        # Markdown sources
slides/deck.md # Main slide deck
dist/          # Generated outputs (HTML/PDF/PPTX)
```

## Adding Another Deck
Create a new file `slides/another.md` and run build/watch again. You can export individually:
```bash
npx marp slides/another.md --pdf --output dist/another.pdf
```

## Custom Theme (Optional)
1. Create `theme/custom.css` (you may need to make a `theme/` folder) with a theme definition.
2. Use: `npx marp slides/deck.md --theme-set theme/custom.css --html --output dist/deck.html`.

Example skeleton:
```css
/* theme/custom.css */
@import 'default';
section {
  font-family: 'Helvetica', sans-serif;
}
```

## Presenter Notes
Add notes with HTML comments starting with `presenter:`:
```markdown
<!-- presenter: Emphasize collaborative benefits -->
```
These appear in presenter view (Marp extension).

## VS Code Extension
Recommend installing: `marp-team.marp-vscode`. See `.vscode/extensions.json` for automatic suggestion.

## Export Manually
```bash
# HTML
npx marp slides/deck.md --html --output dist/deck.html
# PDF
npm run marp:pdf
# PPTX
npm run marp:pptx
```

## Troubleshooting
- If watch does nothing: ensure `slides/` exists and files end in `.md`.
- If PDF/PPTX export fails: make sure you are not blocking Chromium (some corporate policies).

## License
Content license: (add if desired). Code scaffolding ISC per default npm init.
