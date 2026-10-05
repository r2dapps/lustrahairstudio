# Lustra — Hair Color Studio

Try on any hair color live on your camera, or on a photo. Solid shades, ombré, highlights, two-tone and fantasy colors. Free, private, and it runs entirely in your browser.

> **Privacy first:** your camera feed and photos never leave your device. There is no server, no account, and no tracking.

## Features

- **Live camera try-on** with a full-screen, Snapchat-style layout. Swipe the shade carousel to change color.
- **Take a photo, then keep editing.** Tap the center circle to capture. You can still change shades, intensity and style on the captured photo, then save it with the download icon.
- **Upload a photo** (or drag one onto the page) and try colors on it.
- **Before / after slider** to compare your natural hair with the new color.
- **Realistic dye effect.** Strand shading and shine are preserved, so the result looks dyed rather than painted on.
- **Natural and gray-hair shades.** Jet Black, Soft Black and Dark Brown cover gray hair. Platinum, Silver and Snow White go the other way.
- **Multi-color looks.** Four layouts work with any shade: Solid, Ombré, Highlights and Two-tone.
- **Intensity slider** to control how strong the color is.
- **Works on phones and desktops.** Front and rear cameras are supported.

## Try it

Open the live site: `https://<your-username>.github.io/<repo-name>/`

Camera access requires **HTTPS**, which GitHub Pages provides.

## Deploy on GitHub Pages

1. Create a repository and add `index.html` (and these docs) to the root.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then select `main` and `/ (root)`.
4. Wait a minute, then open the link shown on that page.

## Run locally

Browsers only allow camera access on `https://` pages or `localhost`, so do not open the file by double-clicking it.

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Collect feedback from friends

Open `index.html` and set `FEEDBACK_URL` near the top of the script, for example to a Google Form link or a `mailto:` address:

```js
const FEEDBACK_URL = "https://forms.gle/your-form";
```

A "Send us feedback" link will then appear on the start screen.

## How it works

1. The camera frame (or photo) is passed to **MediaPipe's multi-class selfie segmenter**, which labels every pixel as background, hair, body, face, clothes or other.
2. The hair confidence map is smoothed over time to reduce flicker, then softened at the edges.
3. A color layer (solid, gradient or stripes) is built from the chosen shade and clipped to the hair area.
4. The layer is blended onto the original frame in several passes. A *color* pass applies hue and saturation while keeping the original strand shading. A *soft-light* pass adds depth. Light shades get a *screen* lift, and deep shades get a *multiply* darkening so they also work on gray or pale hair.

Everything runs in the browser with the HTML canvas. Dependencies are loaded from a CDN:

| What | Where from |
| --- | --- |
| MediaPipe Tasks Vision (pinned to 0.10.32) | jsDelivr |
| Hair segmentation model | Google Cloud Storage (`mediapipe-models`) |

## Browser support

| Platform | Status |
| --- | --- |
| Chrome, Edge (desktop and Android) | Supported |
| Safari (macOS and iOS 16+) | Supported. Edge softening is slightly crisper because Safari ignores canvas blur filters. |
| Firefox | Should work. Performance depends on GPU support. |

Phones render at a lower internal resolution to keep the preview smooth. Performance varies by device.

## Known limitations

- Results are best with a clear, front-facing view and even lighting.
- Very curly, tied-back or partly covered hair may be detected less accurately.
- Hair that is the same color as the background can be hard to separate.
- Only one hair region is recolored per frame, so group photos are not supported yet.
- Colors are a visual preview. Real dye results depend on your starting color and the product used.

## Roadmap

Ideas for what comes next (nose studs, face tattoos, head tracking, makeup, nails, and more) are in [ROADMAP.md](ROADMAP.md).

## Project structure

```
index.html   The whole app (HTML, CSS and JavaScript)
README.md    This file
ROADMAP.md   Ideas and plans
```

## Credits

Built with [MediaPipe](https://ai.google.dev/edge/mediapipe) by Google. Check the MediaPipe and model licenses before using Lustra commercially.
