# Peach Koi Image Prompt Studio — Standalone Embed

A self-contained, single-page cinematic image prompt builder styled to the **Peach Koi Creative** identity. The core application—including embedded brand artwork, styles, application logic, and all **181 reusable prompt blocks**—lives in [`index.html`](./index.html). A small manifest, service worker and icon set make the hosted version installable as a desktop app.

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
- Simple mode that turns a few natural-language sentences into a complete production-ready prompt
- Advanced mode with direct control over every prompt section
- Reusable character profiles
- Preset/custom toggles for scenes, poses, and environments
- Searchable selectors
- Multi-select rendering constraints
- Structured prompt assembly
- Draft autosave
- Save, favourite, load, copy, and delete prompts
- JSON backup and restore
- Responsive Peach Koi Creative interface using the official Pearl Cream, Midnight Plum, Koi Peach, Sakura Pink, and Wisteria palette
- Persistent Peach Koi light and dark modes, with first-visit system-theme detection
- Browser-local privacy: user data stays in `localStorage`
- Parent-page events for embedded integrations
- Installable desktop PWA with its own app window and offline access

## Quick start

Open `index.html` directly, or serve the repository with any static file server:

```powershell
python -m http.server 8080
```

Then visit `http://localhost:8080`.

## Install as a desktop app

Open the live site in a current version of **Chrome** or **Microsoft Edge**:

1. Visit `https://felixgeekfox.github.io/image-prompt-generator/`.
2. Select **Install app** in the Peach Koi header.
3. Confirm the browser installation prompt.

The Prompt Studio then opens from the operating system's application menu in its own standalone window. Drafts, saved prompts, character profiles and theme preferences remain stored locally on that device.

If the install prompt is not ready yet, reload the page once or use the browser menu and choose **Install Peach Koi Image Prompt Studio**. Safari users can use **Add to Dock** where supported.

The service worker caches the application shell for offline use. Opening the site while online refreshes the cached app when a newer deployment is available.

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
| `mode=simple` | `?mode=simple` | Opens the natural-language Simple builder |
| `mode=advanced` | `?mode=advanced` | Opens the full preset-driven Advanced builder |

Visitors can switch themes from the header. Their choice is stored locally in the browser and restored on future visits.

## Simple and Advanced modes

**Simple mode** accepts a short natural-language image idea and expands it locally into a structured visual brief covering character/reference fidelity, scene and pose clarity, clothing, environment, composition, lighting, art direction, clean rendering, text handling, comic continuity, tone, and the final emotional impression. Keyword matching selects relevant presets only when they strongly support the supplied concept.

**Advanced mode** exposes the complete preset-driven builder for direct control over every prompt section.

Simple mode is deterministic and private: it does not send descriptions to an AI service or require an API key.

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
const simplePrompt = PeachKoiPromptStudio.assembleSimple();
const state = PeachKoiPromptStudio.getState();
PeachKoiPromptStudio.setState({ title: "Lantern Night" });
PeachKoiPromptStudio.setMode("advanced");
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
