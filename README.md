# Three.js Product Configurator — Starter

A minimal, dependency-free **3D product configurator** built with [Three.js](https://threejs.org). It demonstrates the core pattern behind real e-commerce configurators: **parametric geometry**, live **material/color options**, dimension sliders, and a live spec/price readout — all in a single `index.html`.

Built and maintained by **[GroDev](https://grodev.pl)** — a studio that has shipped **9 production 3D configurators** for manufacturers.

## ▶️ Live demo

Open `index.html` in any modern browser, or serve it locally:

```bash
npx serve .
# then open http://localhost:3000
```

No build step, no npm install — Three.js is loaded via an ES module import map from a CDN.

## What it shows

| Concept | Where |
|---|---|
| **Parametric geometry** — dimensions regenerate the mesh, not a pre-baked model per size | `buildProduct()` |
| **Material / finish switching** — matte / gloss / metal via `MeshStandardMaterial` | `applyMaterial()` |
| **Live color options** | swatches → `state.color` |
| **Mobile-friendly rendering** — pixel-ratio clamp to 2 | `renderer.setPixelRatio(...)` |
| **OrbitControls** — rotate/zoom the product | addon import |

## ⚠️ Production note: pricing belongs on the server

This starter computes the demo price **client-side** for illustration only. In a real configurator you should **never** compute price in the browser:

- Users can open DevTools and change the numbers.
- Manufacturers change prices monthly — you don't want a redeploy for every change.

In production, pricing lives in a server endpoint (the client only sends the chosen configuration and receives a price back). That's how the live configurators below work.

## Real production configurators (built with this pattern)

These are live, clickable, in-browser configurators shipped for real manufacturers:

- 🏊 [basen3d.grodev.pl](https://basen3d.grodev.pl) — pools
- 🚪 [brama3d.grodev.pl](https://brama3d.grodev.pl) — gates & fencing
- ☀️ [pergola3d.grodev.pl](https://pergola3d.grodev.pl) — bioclimatic pergolas
- 🧖 [sauna3d.grodev.pl](https://sauna3d.grodev.pl) — garden saunas
- 💡 [lampy3d.grodev.pl](https://lampy3d.grodev.pl) — decorative lamps
- 🌿 [oranzeria3d.grodev.pl](https://oranzeria3d.grodev.pl) — garden rooms
- 📦 [opakowania3d.grodev.pl](https://opakowania3d.grodev.pl) — packaging
- 🎪 [zadaszenie3d.grodev.pl](https://zadaszenie3d.grodev.pl) — terrace roofs
- 🎯 [kasetony3d.grodev.pl](https://kasetony3d.grodev.pl) — LED light-box signs

## Need a custom configurator for your product?

If you manufacture a configurable product (furniture, windows, gates, steel structures, packaging…) and want a configurator like the ones above — with your model, your pricing rules, shop/CRM integration and **the source code owned by you** — see **[grodev.pl/konfigurator-produktowy-3d](https://grodev.pl/konfigurator-produktowy-3d)** or reach out at **[grodev.pl](https://grodev.pl)**.

## License

MIT — use it, learn from it, build on it.

---

*Made by [Dominik Groński / GroDev](https://grodev.pl) · Poznań, Poland · Three.js · WebGL · Laravel*
