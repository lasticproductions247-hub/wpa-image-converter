# WPA — Wet-Plate & Pixel Atelier

**A darkroom-grade image converter that runs entirely in your browser.**
Batch · Resize · Compress — PNG, JPEG & WEBP. Zero servers, zero uploads.

🔗 **Live:** https://lasticproductions247-hub.github.io/wpa-image-converter/

---

## What it does

Drop a folder of images on the bed and WPA develops each one from one format
into another, optionally resized and recompressed, then hands you the results
individually or packed into a single `.zip`.

| | |
|---|---|
| **Accepts** | PNG · JPEG · WEBP · GIF · BMP · AVIF · ICO |
| **Delivers** | PNG · JPEG · WEBP |
| **Quality** | Q30 – Q100 (lossless when PNG) |
| **Scale** | 20% – 200%, any dimension |
| **Flatten** | Fills transparency with white / black / cream / custom — JPEG only |
| **Batch** | Unlimited files, developed one at a time with a visible progress bar |
| **Output** | Native Save-As dialog, `SAVE EACH`, or one `.zip` archive |
| **Cost** | Free, no account |

## Why it's different

Most "convert my image" sites upload your file to somebody else's server. WPA
doesn't. Decoding and encoding happen in your tab through the **Canvas 2D API** —
`createImageBitmap()` to decode, `canvas.toBlob()` to encode. Your images never
leave the device, nothing is logged, and the app keeps working with no network
connection at all.

## The saving problem, solved properly

Browsers block or hijack programmatic downloads in sandboxed frames, inside
`cross-origin` iframes, and under some mobile WebViews. Rather than pretend
that doesn't happen, WPA tries four layers in order:

1. **Native Save-As** — `showSaveFilePicker()` where available (Chromium).
2. **`navigator.msSaveOrOpenBlob`** — legacy Edge / IE.
3. **Anchor download** — the standard `blob:` + `download` path.
4. **Open in a new tab** — the guaranteed escape hatch.

A user-cancelled Save-As dialog is detected and *not* retried, so you never get
a duplicate file. If every layer is blocked, the UI says so plainly and points
you at the manual route: hover any developed frame and right-click the preview.

## Graceful degradation

- **No WEBP encoder?** Detected at boot via a 1×1 canvas probe — the WEBP button
  is disabled with an explanation and the default falls back to JPEG.
- **`toBlob` returns null?** The frame is marked `FAILED` and the batch continues.
- **JSZip won't load?** `ARCHIVE .ZIP` transparently falls back to `SAVE EACH`.
- **Running in a preview iframe?** A one-time notice explains the download
  restrictions before the user hits them.
- **`prefers-reduced-motion`** — all animation collapses to ~0ms.

## Accessibility

Keyboard-reachable dropzone (`Enter` / `Space`), visible dashed focus rings,
`aria-live` toast region, labelled controls, real `<button>` semantics
throughout, and a full `prefers-reduced-motion` path.

## Tech

Single file. No build step, no framework, no bundler.

- Vanilla JS (ES5-compatible IIFE, strict mode)
- Canvas 2D API for decode/encode
- [Lucide](https://lucide.dev) 0.462.0 — icons
- [JSZip](https://stuk.github.io/jszip/) 3.10.1 — archives
- Fraunces + IBM Plex Mono via Google Fonts
- CSS: custom properties, grid, `@media` breakpoints at 960 / 900 / 640px

Deployed by GitHub Pages Actions on every push to `main`.

## Run it locally

It's a static file — no server needed:

```bash
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

To serve instead (needed if you ever add a service worker):

```bash
python -m http.server 8000
```

## Local development

```bash
git clone https://github.com/lasticproductions247-hub/wpa-image-converter.git
cd wpa-image-converter
# edit index.html — that really is the whole app
```

## Privacy

Everything runs client-side. There is no analytics, no telemetry, no upload
endpoint and no server-side component of any kind. Close the tab and the
darkroom is gone.

## Licence

MIT — do what you like with it.