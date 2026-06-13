# 👁️ NeuralEye — Real-Time Object Detection in the Browser 

> Live webcam object detection powered by TensorFlow.js + COCO-SSD. No backend, no API keys, no installation. Just open and detect. 

![NeuralEye Demo](https://img.shields.io/badge/status-live-39ff14?style=flat-square&labelColor=0d0d0d)
![TF.js](https://img.shields.io/badge/TensorFlow.js-4.17-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![COCO-SSD](https://img.shields.io/badge/Model-COCO--SSD%20MobileNetV2-blue?style=flat-square)
![No Server](https://img.shields.io/badge/backend-none-39ff14?style=flat-square&labelColor=0d0d0d)
![License](https://img.shields.io/badge/license-MIT-white?style=flat-square)

---

## What is NeuralEye?

NeuralEye is a **fully browser-based, real-time object detection app** that uses your webcam to identify and label everyday objects in live video — with zero server, zero API keys, and zero installation required. 

It runs the **COCO-SSD MobileNetV2** model entirely on your device using TensorFlow.js, achieving 15–30 FPS on modern hardware. It's built as a single self-contained HTML file — the entire app is ~300 lines.

### Detected Classes (80 total)
Person, car, bicycle, dog, cat, laptop, phone, chair, bottle, truck, bus, and 69 more from the COCO dataset.

---

## Features

- **Real-time inference** — runs on every frame from your webcam, no throttling
- **Zero dependencies to install** — models load from CDN on first run and cache in the browser
- **Adjustable confidence threshold** — filter out weak detections with a live slider
- **Max detections control** — tune between 1 and 40 simultaneous tracked objects
- **Live FPS + inference time counter** — see exactly how fast your device runs the model
- **Event log** — timestamped log of significant detections
- **Mirror toggle** — flip the video for natural front-camera use
- **Label + confidence toggles** — control what's rendered on the canvas overlay
- **Color-coded bounding boxes** — each class gets a unique persistent color
- **Corner-bracket style boxes** — clean tactical UI aesthetic, inspired by computer vision research tools

---

## Quick Start

No installation needed. Just:

1. Download `yolo_webcam_detector.html`
2. Open it in **Chrome** or **Firefox**
3. Click **"START CAMERA"** and allow camera access
4. Wait ~10–20 seconds for the model to load (first time only — it caches after)
5. Detection begins automatically

> The model weights (~5 MB) are downloaded once and cached by the browser. Subsequent loads are instant.

---

## How It Works

```
Webcam Frame
     │
     ▼
┌─────────────────────────┐
│   TensorFlow.js Runtime │  ← runs in the browser, GPU-accelerated via WebGL
└─────────────────────────┘
     │
     ▼
┌─────────────────────────┐
│  COCO-SSD MobileNetV2   │  ← object detection model, 80 classes
└─────────────────────────┘
     │
     ▼
┌─────────────────────────┐
│  Bounding Box Renderer  │  ← canvas overlay, drawn every animation frame
└─────────────────────────┘
     │
     ▼
    UI: class label · confidence · FPS · sidebar stats
```

The app uses `requestAnimationFrame` to run a continuous inference loop. Each tick:
1. Calls `model.detect(videoElement)` with the current frame
2. Filters results by the confidence threshold
3. Draws bounding boxes with corner brackets on a transparent `<canvas>` overlay
4. Updates the sidebar with sorted detections and live performance stats

---

## Tech Stack

| Layer | Technology |
|---|---|
| ML Runtime | TensorFlow.js 4.17 |
| Model | COCO-SSD MobileNetV2 |
| Rendering | HTML5 Canvas API |
| Fonts | JetBrains Mono + Syne (Google Fonts) |
| Backend | None |
| Dependencies | None (CDN only) |

---

## Browser Compatibility

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Recommended |
| Firefox 88+ | ✅ Works |
| Edge 90+ | ✅ Works |
| Safari | ⚠️ May work, WebGL support varies |
| Mobile Chrome | ⚠️ Works but slower — ~5–10 FPS |

---

## Performance Tips

- **Use Chrome** — it has the best WebGL backend for TensorFlow.js
- **Keep the tab in focus** — browsers throttle background tabs
- **Good lighting** — the model performs significantly better in well-lit environments
- **Lower max detections** — reducing from 20 to 10 improves FPS on weak hardware
- **Raise the confidence threshold** — fewer boxes drawn = faster render loop

---

## Extending the Project

This project is intentionally minimal to serve as a launchpad. Here's how to take it further:

### Add a custom model
Swap `cocoSsd.load()` for any TensorFlow.js-compatible model:
```javascript
// Replace this:
model = await cocoSsd.load({ base: 'mobilenet_v2' });

// With a custom model:
model = await tf.loadGraphModel('https://your-cdn.com/model.json');
```

### Record detections to a log file
Use the File System Access API (Chrome only):
```javascript
const handle = await window.showSaveFilePicker({ suggestedName: 'detections.json' });
const writable = await handle.createWritable();
await writable.write(JSON.stringify(detectionLog));
await writable.close();
```

### Add object counting over time
Track how many times each class appears across frames and visualize trends using Chart.js.

### Fine-tune on a custom dataset
Train your own COCO-SSD–compatible model using the [Teachable Machine](https://teachablemachine.withgoogle.com/) or fine-tune YOLOv8 with [Ultralytics](https://docs.ultralytics.com/) and export to TF.js format.

### Add a Python backend for heavier models
If you need YOLOv8 or better accuracy, run a FastAPI WebSocket server:
```bash
pip install ultralytics fastapi uvicorn
uvicorn server:app --reload
```
Then stream frames from the browser to the backend via WebSocket and render the returned bounding boxes on the same canvas.

---

## Project Structure

```
neuraleye/
│
├── yolo_webcam_detector.html   # The entire app — single file
└── README.md                   # This file
```

---

## Roadmap

- [ ] Object tracking with persistent IDs across frames (DeepSORT / ByteTrack)
- [ ] Detection history chart (objects over time)
- [ ] Screenshot / clip export with overlay burned in
- [ ] Support for video file upload (not just webcam)
- [ ] YOLOv8 backend mode via FastAPI WebSocket
- [ ] Mobile-optimized layout
- [ ] Custom model loader (drag and drop a `.json` model)

---

## License

MIT — do whatever you want with it. Attribution appreciated but not required.

---

## Acknowledgements

- [TensorFlow.js](https://www.tensorflow.org/js) — ML in the browser
- [COCO-SSD](https://github.com/tensorflow/tfjs-models/tree/master/coco-ssd) — pre-trained detection model
- [COCO Dataset](https://cocodataset.org/) — 80-class benchmark dataset
- [JetBrains Mono](https://www.jetbrains.com/lp/mono/) — monospace font

---

*Built as part of an advanced AI/ML project portfolio.*
