# 希望 · 愛 — Cat Particle Study

An interactive series of ink drawings of a cat, each rebuilt from tens of thousands of particles and paired with a kanji for what she's feeling. The type is made of particles too, so it reacts to the cursor along with her.

| # | Kanji | Reading | Meaning |
|---|-------|---------|---------|
| 01 | 希望 | kibō | hope |
| 02 | 愛 | ai | love |

## Interaction

| Input | Action |
|-------|--------|
| Move cursor / finger | Disturb the particles |
| Click / tap | Burst |
| Drag | Sweep ink along with the pointer |
| `1` `2` `3` `4` | Force mode: Repel · Vortex · Gravity · Wind |
| `Space` | Scatter everything, then let it reform |
| `←` `→`, scroll, or a quick swipe up/down | Previous / next drawing (particles morph between them) |
| Leave it alone | Autoplays to the next drawing every 10 seconds (pauses while you pet her or drag); the line under the counter shows the timer |
| Rest the cursor on her (or hold a finger) | She settles and purrs |
| Brush past an ear | The ear twitches |

The radius and strength sliders tune the force. The density slider (25–250%, default 190%) rebuilds the drawing with fewer or more particles: low is a loose, airy scatter, and high gets close to the original pen drawing.

### Ink and dust

The ink is **Indigo**: deep navy at rest, bleeding toward sky blue when the particles move. It's defined in the `PALETTES` list in `index.html` (paper, ink at rest, and the moving color) if you ever want to change it.

**Dust** toggles a sparse layer of faint ink motes drifting over the paper. It reacts to the cursor and Scatter like the drawings do.

## Run locally

The page reads image pixels from a canvas, which browsers block for `file://` URLs. So serve the folder rather than double-clicking `index.html`:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

(`npx serve` or the VS Code Live Server extension work too.)

## Publish with GitHub Pages

1. Push this folder to a GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then click **Save**.
4. After a minute the site is live at `https://<username>.github.io/<repo>/`.

There's no build step. It's one HTML file plus the images in `assets/`, and `.nojekyll` tells Pages to serve the files as they are.

## Adding a drawing to the series

Drop the image in `assets/` and add an entry to the `scenes` array near the top of the script in `index.html`:

```js
{ src: 'assets/cat-03.jpg', glyphs: ['眠', 'い'], reading: 'nemui · sleepy',
  gamma: 0.85, density: 1, glyphSize: 0.27, glyphY: 0.5,
  eyes: [{ u: .45, v: .30, rx: .05, ry: .03 }, { u: .62, v: .31, rx: .05, ry: .03 }],
  ears: [{ pu: .40, pv: .22, tu: .38, tv: .06, w: .08 }, { pu: .68, pv: .22, tu: .74, tv: .07, w: .08 }] },
```

| Field | What it does |
|-------|--------------|
| `glyphs` | Characters, stacked vertically, top to bottom |
| `reading` | Romanization and meaning, shown at the top right |
| `gamma` | Tone curve for sampling. Lower brings out light hatching; raise it (about 1.5) for drawings with a lot of dark fill |
| `density` | Particle-count multiplier. Raise it for darker drawings |
| `glyphSize` | Kanji size as a share of the drawing's height. About 0.27 for two characters, 0.36 for one |
| `scale` | Optional. Shrinks or enlarges the drawing (1 = default), for drawings that feel bigger than the rest of the series. Desktop only; on phones every drawing already fills the width |
| `glyphNudge` | Optional. Moves the kanji right by this share of the drawing's height after it's placed, so it can tuck over faint fur like a tail; anything it covers is cleared |
| `glyphY` | Vertical center of the kanji as a share of the drawing's height (0.5 = centered) |
| `eyes` | Two ellipses `{ u, v, rx, ry, tilt }` used for her random slow blinks (`tilt` is optional, in degrees, clockwise, so a lid can close along the angle of a tilted head): center and radii as fractions of the drawing's width (`u`, `rx`) and height (`v`, `ry`) |
| `sway` | Optional list of parts that swing on their own, like a pendant or tail: `{ pu, pv, tu, tv, w, taper, curl, amp, speed, wag }`. Pivot point, tip point, half-width, how much it narrows toward the tip, how much more the tip moves than the base, swing angle in radians, speed, and `wag: true` for an irregular tail-like rhythm |
| `ears` | Two ears `{ pu, pv, tu, tv, w }` used for the twitch: base point, tip point, and half-width, as fractions of the drawing |

The kanji is placed to the left of the drawing and slid right automatically until it's about 10px from her body, so you don't have to set it horizontally.

Drawings work best on a light, plain background, cropped close to the cat.

## Typeface

The kanji is set in **Kozuka Gothic Pr6N** when it's installed on the viewer's computer (it can be activated through Adobe Fonts). Adobe Fonts can't be loaded on a public page without a web project, so everyone else sees **Noto Sans JP Black** from Google Fonts, which has a similar structure.

## Tech

Plain HTML, CSS and JavaScript with no dependencies. Particles are simulated in typed arrays and drawn straight into an `ImageData` buffer, which keeps 50,000–110,000 particles at 60 fps.

## License

The code is released under the MIT License (see `LICENSE`). The drawings in `assets/` are not covered by it. All rights reserved.
