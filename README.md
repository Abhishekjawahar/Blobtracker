# BlobTracker

Real-time blob detection and tracking that runs entirely in the browser — no server, no install, no latency.

**[→ Live Demo](https://abhishekjawahar.github.io/blobtracker)**

![BlobTracker Demo](docs/preview.gif)

---

## What it does

BlobTracker uses your device camera to detect and track moving objects in real time. It runs 100% client-side using the browser's native Canvas and Camera APIs — meaning zero server round trips and true real-time performance on any device including phones.

Point your camera at any moving object — birds, hands, cars, people — and the app draws tracking boxes, assigns IDs, and lets you layer mathematical overlays on top of the motion.

---

## Features

| Feature | Description |
|---------|-------------|
| Movement-only detection | Uses frame differencing — static objects are ignored |
| Blob tracking | Nearest-centroid matching keeps IDs stable across frames |
| Overlap detection | Boxes turn red with an OVERLAP warning when blobs collide |
| Trail lines | Neon green path history up to 300 frames deep |
| Camera switching | Toggle front/rear camera instantly |
| Recording | Record the processed output directly from the browser |
| Download | Save recordings as `.webm` to your phone gallery |

### Mathematical Overlays

| Overlay | What it shows | Color |
|---------|--------------|-------|
| Parabolic curve | Least-squares 2nd-order polynomial fit to trail — predicts trajectory | Amber |
| Gradient vectors | Velocity arrow showing speed and direction of each blob | Cyan |
| Optical flow | Sparse Lucas-Kanade flow on a pixel grid across the whole frame | Violet |
| Convex hull + web | Outer boundary of all blobs + lines to group centroid | Orange |
| Velocity heatmap | Radial color overlay: blue = slow, green = medium, red = fast | Red |

---

## Architecture

Everything runs inside a single `requestAnimationFrame` loop in the browser:
```
Camera (getUserMedia)
    ↓
Video element (live stream)
    ↓
requestAnimationFrame loop (~60fps)
    ↓
┌─────────────────────────────────────┐
│  Processing pipeline (per frame)    │
│  1. Frame differencing              │
│  2. Morphological dilation          │
│  3. BFS flood fill → blobs          │
│  4. Nearest-centroid ID matching    │
│  5. Math overlays (optional)        │
└─────────────────────────────────────┘
    ↓
Overlay canvas (drawn output)
    ↓
MediaRecorder → .webm download
```

No Python. No server. No WebRTC negotiation. One HTML file.

---

## How to run locally

No install needed. Just open the file:
```bash
git clone https://github.com/abhishekjawahar/blobtracker
cd blobtracker
open index.html
```

Or serve it locally if your browser blocks camera on `file://`:
```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

---

## How to use

1. Open the [live link](https://abhishekjawahar.github.io/blobtracker) on your phone or desktop
2. Allow camera access when prompted
3. Move an object in front of the camera — green boxes appear on moving blobs
4. Adjust **Movement Sensitivity** if nothing appears (try raising it to 30+)
5. Toggle overlays using the buttons in the controls panel
6. Press **Record** to capture, **Save** to download to your device

### Tuning tips

- **Too many blobs** — lower sensitivity or raise Min Blob Size
- **Missing blobs** — raise sensitivity or lower Min Blob Size
- **Jittery boxes** — lower sensitivity slightly, keep camera steady
- **Trails not appearing** — toggle Trail Lines on, move the object slowly

---

## Customisation

These are the values in `index.html` you can change without breaking anything:
```javascript
// Line 12-13 — trail settings
const TRAIL_LEN   = 300;     // how many frames of history to keep
const TRAIL_COLOR = (0, 255, 136);  // neon green — change to any RGB

// Line 245 — motion decay speed
// Higher = slower to forget background (more stable)
// Lower = faster to react to new positions (more sensitive)
prev_gray = prev_gray * 0.7 + gray * 0.3

// Line 310 — blob matching distance
// Increase if fast-moving blobs keep losing their ID
const maxDist = 80;
```

---

## Tech stack

| Technology | Purpose |
|-----------|---------|
| HTML5 Canvas API | Drawing overlays and frame processing |
| MediaDevices API | Camera access (front and rear) |
| MediaRecorder API | Recording canvas output |
| requestAnimationFrame | 60fps render loop |
| Vanilla JavaScript | All processing — no libraries |

Zero dependencies. No npm. No build step. One file.

---

## Browser support

| Browser | Support |
|---------|---------|
| iPhone Safari (iOS 14+) | Full |
| Chrome on Android | Full |
| Chrome on Desktop | Full |
| Firefox | Full |
| Safari on Mac | Full |

---

## File structure
```
blobtracker/
├── index.html        ← entire app (HTML + CSS + JS)
├── README.md         ← this file
├── CHANGELOG.md      ← version history
├── LICENSE           ← MIT
└── docs/
    └── preview.gif   ← demo gif for README (optional)
```

---

## Roadmap

- [ ] Export trail data as JSON (x,y coordinates per blob)
- [ ] Count blobs over time graph
- [ ] Colour-code each blob independently
- [ ] Zoom/pan controls
- [ ] Adjustable trail fade (opacity over time)
- [ ] Touch gesture controls for mobile

---

## Contributing

This is a personal project but PRs are welcome. The entire app lives in `index.html` — find the section you want to change, make the edit, test in your browser.

---

## License

MIT — do whatever you want with it.

---

## Author

Built by **Abhishek Jawahar** as a personal computer vision experiment.  
Started as a Python + OpenCV desktop app, evolved into a zero-dependency browser app.

If you use this or build on it, a mention would be appreciated.