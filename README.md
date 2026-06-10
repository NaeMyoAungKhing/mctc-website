# MCTC — Train & Place

A single-page interactive site for **Myanmar Creative Technology College**, structured around the *Train and Place* model. Built as one self-contained HTML file with an animated, clickable node graph as the centrepiece.

---

## Quick Start

1. Open `index.html` in any modern browser — that's it. No build step, no dependencies.
2. To add real photos: drop image files into the `images/` folder using the names listed in `IMAGES.md`. The site auto-detects and swaps them in.

For local preview with a small web server (recommended so images load via HTTP, not file://):

```bash
# Python (any version 3.x)
python3 -m http.server 8080

# Node (if installed)
npx serve .
```

Then visit `http://localhost:8080`.

---

## What's in this folder

```
mctc-train-place/
├── index.html         ← the full site (single file, ~1600 lines)
├── README.md          ← you are here
├── IMAGES.md          ← image manifest: what's expected, where, what size
├── docs/
│   └── CHANGELOG.md   ← version history of the design iterations
└── images/            ← drop photos here (filenames must match IMAGES.md)
```

---

## Architecture

Everything lives in `index.html`:

- **HTML** — semantic sections (`#system`, `#culture`, `#train`, `#place`, `#loop`, `#contact`)
- **CSS** — inline `<style>` block at the top, custom-property driven (all colors and fonts in `:root`)
- **JavaScript** — inline `<script>` block at the bottom. Three things only: the node graph renderer, the popup modal, and an image auto-loader.

Fonts load from Google Fonts: **Fraunces** (display, italic), **Inter Tight** (body), **JetBrains Mono** (code/labels).

External dependencies: zero JavaScript libraries. The graph is hand-drawn SVG.

---

## Design system

All design tokens are CSS custom properties at the top of `<style>`. Edit them in one place to retheme:

```css
--violet: #8b5cf6;        /* primary brand */
--violet-soft: #a78bfa;   /* hover, accents */
--violet-deep: #6d28d9;   /* deeper shade */
--gold: #f5b942;          /* the flame — root node + accents */
--lavender: #c4b5fd;      /* Place branch cousin colour */
--bg: #0a0816;            /* dark violet base */
```

---

## The node graph

Defined as data, not markup. Open `index.html` and search for `const nodes = [`. Each entry looks like this:

```javascript
{
  id: 'ojt',
  x: 480, y: 480, r: 22,    // position and radius on a 1600×900 viewBox
  type: 'train',             // root | train | place | loop | flow
  label: 'A.2 · OJT',        // short code shown under the node
  title: 'On-Job Training',  // headline shown under the node + in modal
  desc: '...',               // modal description paragraph
  meta: ['Live Work', 'Mentored', 'Paid Roles'],   // pill tags
  details: [['Key', 'Value'], ...],                // structured detail list in modal
  link: '#train',             // section to scroll to on CTA click
}
```

To add a node: append to the `nodes` array and add edges in the `edges` array. The renderer picks it up automatically.

To reposition: change `x` and `y`. The canvas is 1600 wide, 900 tall.

---

## Image system

Every image slot has a `data-img` attribute matching a filename in the `images/` folder. The auto-loader tries `.jpg`, `.jpeg`, `.png`, `.webp` in that order. If a file is found, the placeholder fades out and the photo fades in. If not, the placeholder stays visible.

Adding a photo is therefore zero-code:

1. Save the file as `images/<name>.<ext>` (e.g. `images/classroom_session.jpg`)
2. Refresh the page

See `IMAGES.md` for the complete list of expected filenames, recommended dimensions, and what each one should depict.

---

## Sections (in order)

| # | Section | What it does |
|---|---------|--------------|
| Hero | `#system` | The node graph + title overlay. Click any node to open the popup. |
| — | Stats bar | One-line numbers strip (515 students · 21 programs · etc.) |
| 01 | `#culture` | Three pillars — Inspiration, Innovation, Intuition |
| 02 | `#train` | Sub-pillars A.1–A.4 + three image placeholders |
| 03 | `#place` | Sub-pillars B.1–B.3 + three image placeholders |
| 04 | `#loop` | The 75% stat + hero faculty image |
| 05 | Partners | Auto-scrolling brand marquee |
| Footer | `#contact` | Address, phones, emails |

---

## Browser support

Tested on current Chrome, Safari, Firefox, Edge. Mobile responsive (breakpoint at 900px). No IE support (uses CSS custom properties, `backdrop-filter`, modern animation).

---

## What still needs doing

- [ ] Drop real images into `images/` (see `IMAGES.md`)
- [ ] Confirm copy on the Culture cards — currently distilled from the business profile
- [ ] Optional: add a People section back in if you want named portraits before placement
- [ ] Optional: replace the "MCTC" logo mark with a proper flame-of-passion sigil SVG

---

## Contact

Built for MCTC, Yangon. Pearson UK Centre No. 92672.
