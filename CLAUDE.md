# AI workflow notes — Three.js product configurator starter

Kept in the repo because I build with Claude Code (Anthropic) and want the AI/human split on every project to be legible from the source tree, not just claimed in a README.

## Human vs AI split on this repo

| Layer | Who did it | Why |
|---|---|---|
| Architecture & scope decision | **Human** — decided this was a *starter* (single-file, importmap, no build step) rather than a framework. The whole point is that a developer can `open index.html` and see it work. | Wrong shape here would kill the "wow in 5 seconds" this repo exists for. Not delegable. |
| Three.js scene setup (renderer, camera, OrbitControls, lighting) | **AI-drafted, human-reviewed** | This is boilerplate Claude writes correctly; my job was to pick the right lighting rig for material previews and reject the default 3-point that hides material response. |
| Parametric geometry + live-updating dimensions | **Human-designed, AI-coded** | Data flow (state → rebuild mesh → recompute price) was mine; the loop that disposes old geometry to prevent memory leaks was Claude. |
| UI (control panel, price readout) | **AI-drafted, human-styled** | Kept it dependency-free vanilla JS on purpose — no React, no Tailwind — so anyone can fork and drop into a WordPress theme. |
| README + backlinks to grodev.pl | **Human** | Marketing decisions belong to me. |

## Verification I ran before pushing

- Manually opened `index.html` in Chrome + Firefox — no console errors, no CORS issues from the importmap.
- Resized the geometry in the DevTools console (`window.state.width = 3.5; update()`) to confirm no stale references between rebuilds.
- Ran GitHub Pages build — verified it serves without a build step (that's the point).

## Known gotchas for the next AI edit

- **Do not add a bundler.** The importmap + `<script type="module">` is the entire pitch. If Claude suggests Vite/webpack, reject.
- **Do not switch OrbitControls to TrackballControls** — TrackballControls has no auto-inertia stop, feels drunk on touch devices.
- **Dispose geometry before replacing** (`old.geometry.dispose()`) — WebGL context leaks otherwise, browser tab hits ~500 MB after 20 config changes.
- Material color updates must be `.set(hex)` on the existing `MeshStandardMaterial`, **not** a new material assignment (breaks fog and shadow bindings).

## When to reach for Claude on this project vs code it yourself

- **Reach for Claude:** any new preset (a different product shape — desk, planter, lamp), any new material type (glass, brushed metal), a new export format.
- **Do it yourself:** any change to the price-calculation logic. Pricing is a business rule; letting an AI improvise it is how you ship a bug that costs you a real customer.
