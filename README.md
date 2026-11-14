# Peach Koi Image Prompt Studio — Standalone Embed

A self-contained, single-page cinematic image prompt builder styled to the **Peach Koi Creative** identity. The entire distributable—including embedded brand artwork, styles, application logic, and all **181 reusable prompt blocks**—lives in [`index.html`](./index.html).

No framework, build step, database, account, or external JavaScript dependency is required.

## Features

- 40 scene presets
- 30 pose and interaction presets
- 20 environment presets
- 15 lighting presets
- 15 composition presets
- 15 mood presets
- 26 visual style presets
- 20 rendering and quality constraints
- Reusable character profiles
- Preset/custom toggles for scenes, poses, and environments
- Searchable selectors
- Multi-select rendering constraints
- Structured prompt assembly
- Draft autosave
- Save, favourite, load, copy, and delete prompts
- JSON backup and restore
- Responsive Peach Koi Creative interface using the official Pearl Cream, Midnight Plum, Koi Peach, Sakura Pink, and Wisteria palette
- Browser-local privacy: user data stays in `localStorage`
- Parent-page events for embedded integrations

## Quick start

Open `index.html` directly, or serve the repository with any static file server:

```powershell
python -m http.server 8080
```

Then visit `http://localhost:8080`.

## Embed in another website

Host `index.html` on any static host and embed it with an iframe:

```html
<iframe
  src="https://felixgeekfox.github.io/image-prompt-generator/"
  title="Peach Koi Image Prompt Studio"
  loading="lazy"
  style="width:100%;min-height:1100px;border:0;border-radius:20px;"
  allow="clipboard-write"
></iframe>
```

For responsive layouts:

```html
<div class="peach-koi-embed">
  <iframe
    src="https://felixgeekfox.github.io/image-prompt-generator/?compact=1"
    title="Peach Koi Image Prompt Studio"
    allow="clipboard-write"
  ></iframe>
</div>

<style>
  .peach-koi-embed {
    width: 100%;
    height: min(1300px, 90vh);
    min-height: 760px;
    overflow: hidden;
    border-radius: 20px;
  }
  .peach-koi-embed iframe {
    width: 100%;
    height: 100%;
    border: 0;
  }
</style>
```

## Query options

| Option | Example | Effect |
|---|---|---|
| `compact=1` | `?compact=1` | Uses slightly tighter corner radii for embeds |
| `accent=HEX` | `?accent=22d3ee` | Changes the primary cyan accent colour |

## Parent-page events

When embedded, the app sends events to the parent with `postMessage` and dispatches matching `CustomEvent`s inside its own document.

### Prompt generated

```js
window.addEventListener("message", (event) => {
  // In production, validate event.origin against the URL hosting the generator.
  if (event.data?.type === "peach-koi:prompt-generated") {
    console.log(event.data.detail.prompt);
  }
});
```

### Prompt saved

```js
window.addEventListener("message", (event) => {
  if (event.data?.type === "peach-koi:prompt-saved") {
    console.log(event.data.detail.prompt, event.data.detail.favorite);
  }
});
```

## JavaScript API

When used directly on a page, the app exposes a small API at `window.PeachKoiPromptStudio`:

```js
PeachKoiPromptStudio.generate();
const prompt = PeachKoiPromptStudio.assemble();
const state = PeachKoiPromptStudio.getState();
PeachKoiPromptStudio.setState({ title: "Lantern Night" });
const characters = PeachKoiPromptStudio.getCharacters();
```

`PeachKoiPromptStudio.clearLocalData()` erases all local draft, character, and saved-prompt data after reloading. `window.RKFX` remains as a backwards-compatible alias.

## Storage and privacy

The standalone edition deliberately has no authentication or backend. It stores the current draft, character profiles, and saved prompts in the visitor's browser using `localStorage`. Data is not sent to Peach Koi Creative, GitHub, or any third party.

Users can move their data between browsers with **Export data** and **Import data**.

## Customisation

The CSS variables at the start of `index.html` control the visual theme:

```css
:root {
  --bg: #fff3ed;
  --text: #21102f;
  --cyan: #ff9c83;
  --pink: #f6b2d4;
  --violet: #c394ea;
}
```

All prompt library data is embedded in the `BLOCKS` array near the bottom of the file. This keeps the shared build portable and prevents host websites from needing additional assets.

## Peach Koi brand system

The interface follows the Peach Koi Creative Brand Identity & Visual Guidelines v1.0:

- Pearl Cream and Midnight Plum foundations
- Koi Peach, Sakura Pink, and Wisteria primary colours
- Lantern Gold and Moonlight Mint as restrained accents
- Geometric display typography with a friendly body face and poetic italic accents
- Embedded Peach Koi artwork, with no separate image files required
- Rounded forms, luminous gradients, sakura/starlight details, and polished high-contrast presentation

The GitHub Pages workflow publishes the repository on every push to `main`, providing a stable URL for iframe embedding.

## License

MIT. See [`LICENSE`](./LICENSE).
