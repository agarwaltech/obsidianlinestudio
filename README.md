# Obsidian Line Studio — obsidianlinestudio.in

Single static page. No framework, no build step, no dependencies.

```
index.html    the entire site
resume.pdf    <- YOU MUST ADD THIS
_headers      security headers (Cloudflare Pages / Netlify)
robots.txt
sitemap.xml
```

---

## Before you deploy

**1. Add your resume.** Copy your PDF into this folder, named exactly `resume.pdf`
(all lowercase). The download button links to it.

**2. Add real YouTube video links.** The three studio cards currently all point at
your channel homepage. To point them at specific videos, open `index.html`,
search for `class="reel"`, and change each card's `href` to the real video URL.
Also update the `<h4>` title and `<p>` description to match the actual video.

**3. Optional — add real thumbnails.** Each card has a `<div class="frame">` that
draws a film-strip placeholder. To use a real thumbnail instead, replace:

```html
<div class="frame"><div class="play"></div></div>
```

with:

```html
<div class="frame">
  <img src="thumbs/my-video.jpg" alt="" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;opacity:.55">
  <div class="play"></div>
</div>
```

Put the image in a `thumbs/` folder next to `index.html`.
YouTube thumbnails are available at
`https://img.youtube.com/vi/VIDEO_ID/maxresdefault.jpg`

---

## Preview locally

```
python -m http.server 8080
```

Then open http://localhost:8080

---

## Deploy (Cloudflare Pages, free)

1. Push this folder to a GitHub repo, e.g. `agarwaltech/portfolio`
2. dash.cloudflare.com -> Workers & Pages -> Create -> Pages -> Connect to Git
3. Framework preset **None**, build command **empty**, output directory `/`
4. Deploy, then Custom domains -> add `obsidianlinestudio.in`

Updating later is just:

```
git add .
git commit -m "update"
git push
```

---

## Editing notes

- All styling is in the `<style>` block at the top of `index.html`.
- Colours are CSS variables in `:root` — `--neon` (green), `--ember` (red).
- The intro sequence runs for 2.45s and can be skipped. To remove it entirely,
  delete the `<div id="intro">` block and its CSS.
- Section anchors: `#engineering`, `#intelligence`, `#studio`, `#record`, `#contact`
- Project card ids: `p-pipeline`, `p-qa`, `p-infra`, `p-triage`


---

## Animation layers (and how to dial them back)

Everything is in `index.html`. All effects are automatically disabled for
visitors who have "reduce motion" turned on in their OS.

| Effect | Where | Turn it off |
|---|---|---|
| Boot intro (digital rain + console + progress bar) | `<div id="intro">` | Delete the whole `#intro` div |
| Particle network background | `<canvas id="net">` | Delete the canvas tag |
| Scanlines + sweeping light | `<div id="fx">` | Delete the div, or lower `opacity` in `#fx` |
| Glitch on hero words | `.glitch` class | Remove `glitch` from the two `<span>`s |
| Text scramble on headings | JS `scramble()` | Remove the `ho` IntersectionObserver block |
| Synthwave grid in studio | `.gridfloor` | Delete the div |
| Border beam on card hover | `.proj::after` / `.mini::after` | Set `opacity:0` |
| Cursor spotlight | `#spot` | Delete the div |
| HUD corner brackets on card hover | `.proj::after` / `.mini::after` | Set `opacity:0` in the hover rule |
| Sound effects | `SFX` block in JS | Delete the `#snd` button; sound is off by default |

**Intro length:** search for `setTimeout(endIntro, 3100)` — the number is
milliseconds. Lower it to shorten, or set the whole `#intro` div to
`display:none` to skip it entirely.

**Particle density:** search for `W < 720 ? 34 : W < 1200 ? 58 : 82` — those are
particle counts for mobile / tablet / desktop. Lower them if you want it calmer
or faster on old machines.

**Colours:** `--neon` and `--ember` in the `:root` block drive everything.


---

## Sound

All sound is **synthesised in the browser with Web Audio** — there are no audio
files to host, and nothing extra to download.

- **Off by default.** Browsers block audio until someone interacts with the page,
  and autoplaying sound on a portfolio is hostile. Visitors turn it on with the
  `SOUND` button in the nav.
- The choice is remembered in `localStorage`, so returning visitors keep it on.
- Master volume is set to `0.16` — search for `master.gain.value` to change it.

What makes noise, once enabled:

| Sound | When |
|---|---|
| Boot sweep + two chimes | Intro sequence |
| Short blip | Clicking any link or button |
| Soft tick | Hovering a project, card or pipeline stage |
| Low descending tone | Scrolling into the Studio section |

To remove sound entirely, delete the `<button id="snd">` from the nav — the
engine stays dormant and silent without it.
