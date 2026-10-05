# Lustra Roadmap — What Else Can We Build with Web AR?

Lustra already does hair. This document collects what is possible next using MediaPipe in the browser, grouped by the model each idea needs.

**Effort:** S = a day or two · M = about a week · L = several weeks
**Impact:** how much it should matter to users

## What MediaPipe can do in the browser

| Task | What it gives us | Unlocks |
| --- | --- | --- |
| **Image Segmenter** (already used) | Per-pixel masks for hair, face, body, clothes and background | Hair, clothes, skin and background effects |
| **Face Landmarker** | A dense face mesh, blendshapes (smile, blink and so on) and a head-pose matrix | Face tattoos, nose studs, makeup, glasses, 3D accessories |
| **Hand Landmarker** | 21 points per hand | Nails, rings, henna, gesture controls |
| **Gesture Recognizer** | Common hand gestures | Hands-free capture and shade switching |
| **Pose Landmarker** | 33 body points | Clothing and jewelry try-on |
| **Face Detector** | Fast face boxes | Multi-person support, privacy blur |

---

## 1. Quick wins (models we already load)

The multi-class segmenter we use for hair also labels **clothes, face and background**. These need no new model.

| Idea | Effort | Impact |
| --- | --- | --- |
| **Clothes recolor.** Reuse the dye engine on the clothes mask. | S | High |
| **Background blur and replacement.** Studio, beach or solid-color backdrops. | S | Medium |
| **Gray-coverage and roots controls.** Dark roots, grow-out effect, and a slider for how much gray to cover. | S | High |
| **Shine and gloss slider.** Adds a glossy highlight along strands. | S | Medium |
| **Video capture.** Record a short clip with `MediaRecorder` and `canvas.captureStream()`. | S | High |
| **Skin smoothing.** Gentle blur on the face mask while keeping hair sharp. | S | Medium |
| **Shareable looks.** Encode the shade, style and intensity in the URL (`#look=rose-gold&style=ombre`). | S | High |

## 2. Face Landmarker ideas

| Idea | Effort | Impact | Notes |
| --- | --- | --- | --- |
| **Nose stud, ring and nath** | M | High | Anchor to a landmark near the nostril wing. Rotate with head pose so it stays attached when the head turns. |
| **Face tattoos and temporary art** | M | High | Map a transparent texture onto the face mesh so it follows expressions. Add opacity and size controls. |
| **Bindi, maang tikka and forehead jewelry** | M | High | Strong cultural fit for Indian users. Anchor to the forehead and brow landmarks. |
| **Earrings and jhumkas** | M | High | Anchor to the ear area. Hide them when the ear turns away from the camera. |
| **Lipstick, blush and eyeshadow** | M | High | Fill polygons built from lip and eye landmarks, using the same blend modes as the hair engine. |
| **Eyebrow shaping and color** | M | Medium | Recolor and reshape using the brow landmarks. |
| **Eye color** | M | Medium | Tint the iris region. Needs careful masking around eyelids. |
| **Beard and mustache color** | M | Medium | Combine face landmarks with a facial-hair mask. A custom model may be needed for good edges. |
| **Glasses and sunglasses** | M | High | A 2D overlay first, then 3D with head pose. |
| **Face paint and festival looks** | S | Medium | Pre-made textures, similar to tattoos. |
| **Expression triggers** | S | Low | Smile to capture, blink to change shade, using blendshapes. |

## 3. Head tracking and 3D (Face Landmarker + Three.js)

The Face Landmarker returns a transformation matrix describing how the head is rotated and positioned. With this we can place real 3D objects on the head.

| Idea | Effort | Impact |
| --- | --- | --- |
| **Hats, caps, headbands and crowns** | L | High |
| **3D earrings and nose rings** that swing and hide correctly when the head turns | L | High |
| **Scalp and head tattoos** (for shaved heads or hair partings) | L | Medium |
| **Hair accessories** such as clips and flowers, placed using the hair mask plus head pose | L | Medium |
| **Parallax "mirror" effect** where the scene shifts as you move | M | Low |

Occlusion (hiding an object behind the head or ear) is the hard part. Plan for a face-mesh depth mask.

## 4. Hand Landmarker ideas

| Idea | Effort | Impact |
| --- | --- | --- |
| **Nail polish try-on** | M | High |
| **Mehndi (henna) on hands** | M | High |
| **Rings, bracelets and watches** | M | Medium |
| **Gesture controls.** Pinch to change shade, open palm to capture. | S | Medium |

## 5. Pose Landmarker ideas

| Idea | Effort | Impact |
| --- | --- | --- |
| **Necklaces and dupatta or scarf overlays** | L | Medium |
| **Simple 2D clothing try-on** | L | Medium |
| **Posture and framing guide**, such as "step back so your hair fits" | S | Low |

## 6. Product and growth ideas

| Idea | Effort | Impact |
| --- | --- | --- |
| **Real shade catalog** with brand shade names and codes, so users can match a result to a product | M | High |
| **Match from a reference photo.** Pick a color from a celebrity or salon photo with an eyedropper. | M | High |
| **Shade suggestions** based on skin tone and undertone. Must be tested for fairness across skin tones. | M | Medium |
| **Salon mode.** Fullscreen kiosk layout, QR code to send the result to a customer's phone. | M | High |
| **Send to WhatsApp** and other share targets. | S | High |
| **Installable app (PWA)** with an icon and offline support. | M | High |
| **Language support**, starting with Telugu and Hindi. | M | High |
| **Saved looks** and a favorites list, stored on the device. | S | Medium |
| **Collages.** Several shades side by side in one image. | S | Medium |

## 7. Engineering improvements

| Idea | Effort | Impact | Why |
| --- | --- | --- | --- |
| **Move the blend to a WebGL shader** | L | High | Removes the per-pixel JavaScript loop and gives smoother video on low-end phones. |
| **Self-host the model and MediaPipe files** | S | High | Removes the reliance on external CDNs and enables offline use. |
| **Service worker for offline use** | M | High | Fast repeat loads and no waiting for the model. |
| **Better hair edges** (guided filter or matting) | L | High | Cleaner flyaway strands and fewer color halos. |
| **Hair-mask stabilization** using motion between frames | M | Medium | Less flicker while the head moves quickly. |
| **Multi-person support** | M | Medium | Combine face detection with per-person hair regions. |
| **Test matrix** for iPhone, Android and low-end devices | S | High | Find real-world slowdowns early. |
| **Privacy-friendly feedback and analytics** | S | Medium | Learn what people use without collecting images. |

---

## Suggested order

1. **Polish what exists (now).** Gather feedback from friends, fix bugs, test on real phones.
2. **Quick wins (next).** Video capture, shareable looks, clothes recolor, gray-coverage controls.
3. **Face accessories (then).** Nose stud, bindi, earrings, face tattoos. This adds the Face Landmarker and opens up most of the ideas above.
4. **Hands and makeup.** Nails, mehndi, lipstick.
5. **3D and head tracking.** Hats and 3D jewelry, once the face pipeline is solid.
6. **Production hardening.** WebGL shader, self-hosted models, offline PWA.

## Good practice for every new feature

- **Keep it on-device.** Never upload camera frames. This is a core promise of the app.
- **Test across skin tones and hair types.** Segmentation and landmark quality can vary, so check results on a wide range of people.
- **Be honest in the preview.** Label results as a visual preview, not a guarantee.
- **Ask for the camera at the right moment**, with a short explanation of why it is needed.
- **Respect age and comfort.** Piercings and tattoos are try-on only. Keep the tone fun and neutral.
- **Keep it fast.** If a feature drops the frame rate on a mid-range phone, scale it back or make it optional.
