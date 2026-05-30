# NeuralEye

What is NeuralEye?
NeuralEye is a fully browser-based, real-time object detection app that uses your webcam to identify and label everyday objects in live video — with zero server, zero API keys, and zero installation required.
It runs the COCO-SSD MobileNetV2 model entirely on your device using TensorFlow.js, achieving 15–30 FPS on modern hardware. It's built as a single self-contained HTML file — the entire app is ~300 lines.
Detected Classes (80 total)
Person, car, bicycle, dog, cat, laptop, phone, chair, bottle, truck, bus, and 69 more from the COCO dataset.

Features

Real-time inference — runs on every frame from your webcam, no throttling
Zero dependencies to install — models load from CDN on first run and cache in the browser
Adjustable confidence threshold — filter out weak detections with a live slider
Max detections control — tune between 1 and 40 simultaneous tracked objects
Live FPS + inference time counter — see exactly how fast your device runs the model
Event log — timestamped log of significant detections
Mirror toggle — flip the video for natural front-camera use
Label + confidence toggles — control what's rendered on the canvas overlay
Color-coded bounding boxes — each class gets a unique persistent color
Corner-bracket style boxes — clean tactical UI aesthetic, inspired by computer vision research tools


Quick Start
No installation needed. Just:

Download yolo_webcam_detector.html
Open it in Chrome or Firefox
Click "START CAMERA" and allow camera access
Wait ~10–20 seconds for the model to load (first time only — it caches after)
Detection begins automatically


The model weights (~5 MB) are downloaded once and cached by the browser. Subsequent loads are instant.


How It Works
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
The app uses requestAnimationFrame to run a continuous inference loop. Each tick:

Calls model.detect(videoElement) with the current frame
Filters results by the confidence threshold
Draws bounding boxes with corner brackets on a transparent <canvas> overlay
Updates the sidebar with sorted detections and live performance stats


Tech Stack
LayerTechnologyML RuntimeTensorFlow.js 4.17ModelCOCO-SSD MobileNetV2RenderingHTML5 Canvas APIFontsJetBrains Mono + Syne (Google Fonts)BackendNoneDependenciesNone (CDN only)

Browser Compatibility
BrowserStatusChrome 90+✅ RecommendedFirefox 88+✅ WorksEdge 90+✅ WorksSafari⚠️ May work, WebGL support variesMobile Chrome⚠️ Works but slower — ~5–10 FPS

Performance Tips

Use Chrome — it has the best WebGL backend for TensorFlow.js
Keep the tab in focus — browsers throttle background tabs
Good lighting — the model performs significantly better in well-lit environments
Lower max detections — reducing from 20 to 10 improves FPS on weak hardware
Raise the confidence threshold — fewer boxes drawn = faster render loop


Extending the Project
This project is intentionally minimal to serve as a launchpad. Here's how to take it further:
Add a custom model
Swap cocoSsd.load() for any TensorFlow.js-compatible model:
javascript// Replace this:
model = await cocoSsd.load({ base: 'mobilenet_v2' });

// With a custom model:
model = await tf.loadGraphModel('https://your-cdn.com/model.json');
Record detections to a log file
Use the File System Access API (Chrome only):
javascriptconst handle = await window.showSaveFilePicker({ suggestedName: 'detections.json' });
const writable = await handle.createWritable();
await writable.write(JSON.stringify(detectionLog));
await writable.close();
Add object counting over time
Track how many times each class appears across frames and visualize trends using Chart.js.
Fine-tune on a custom dataset
Train your own COCO-SSD–compatible model using the Teachable Machine or fine-tune YOLOv8 with Ultralytics and export to TF.js format.
Add a Python backend for heavier models
If you need YOLOv8 or better accuracy, run a FastAPI WebSocket server:
bashpip install ultralytics fastapi uvicorn
uvicorn server:app --reload
Then stream frames from the browser to the backend via WebSocket and render the returned bounding boxes on the same canvas.

Project Structure
neuraleye/
│
├── yolo_webcam_detector.html   # The entire app — single file
└── README.md                   # This file

Roadmap

 Object tracking with persistent IDs across frames (DeepSORT / ByteTrack)
 Detection history chart (objects over time)
 Screenshot / clip export with overlay burned in
 Support for video file upload (not just webcam)
 YOLOv8 backend mode via FastAPI WebSocket
 Mobile-optimized layout
 Custom model loader (drag and drop a .json model)


License
MIT — do whatever you want with it. Attribution appreciated but not required.

Acknowledgements

TensorFlow.js — ML in the browser
COCO-SSD — pre-trained detection model
COCO Dataset — 80-class benchmark dataset
JetBrains Mono — monospace font
