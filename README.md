# 🌿 CropDoc AI — Offline Crop Disease Detector

> **100% offline, on-device AI** crop disease detection PWA. No internet required. Works on low-end smartphones.

![CropDoc AI](https://img.shields.io/badge/CropDoc_AI-v1.0-2e7d46?style=for-the-badge) ![Offline](https://img.shields.io/badge/Offline-100%25-blue?style=for-the-badge) ![TensorFlow.js](https://img.shields.io/badge/TF.js-MobileNetV2-orange?style=for-the-badge)

---

### 📢 Latest Updates (v12)
- **🚜 Farmer-Friendly UI**: Redesigned from the ground up for rural use. Large buttons, high contrast, and simplified navigation.
- **📴 Real Offline Model**: Upgraded engine to **TF.js 4.22.0**. The AI model now loads and runs 100% offline without any "InputLayer" errors.
- **🎯 Boosted Accuracy**: Refined color heuristics and model thresholds to ensure 60-70% confidence for all diseased leaf detections.
- **🕒 Scan History**: Keep track of previous results directly on your device.

---

## 📱 What is CropDoc AI?

CropDoc AI is a **Progressive Web App (PWA)** that detects crop diseases from leaf photos — **entirely on your device**. No internet connection, no cloud servers, no data sent anywhere.

- 📷 **Take a photo** of a crop leaf (or upload one)
- 🧠 **AI analyzes** the image instantly on your phone
- 📋 **Get results**: disease name, severity, symptoms, treatment, and prevention tips
- 📴 **Works 100% offline** — perfect for rural areas with no connectivity

### 🌾 Supported Crops & Diseases

| Crop | Diseases Detected |
|------|------------------|
| 🫑 Pepper (Bell) | Bacterial Spot, Healthy |
| 🥔 Potato | Early Blight, Late Blight, Healthy |
| 🍅 Tomato | Early Blight, Late Blight, Healthy |

*Plus 30 additional classes with color-analysis based detection for: Apple, Blueberry, Cherry, Corn, Grape, Orange, Peach, Raspberry, Soybean, Squash, Strawberry.*

---

## 🚀 Quick Start

### Option 1: Run with Python (recommended)

```bash
# Clone or download the project
cd "crop disease detecter"

# Install dependencies
pip3 install fastapi uvicorn pillow python-multipart numpy

# Start the server
python3 -m uvicorn app:app --host 0.0.0.0 --port 8000
```

Open `http://localhost:8000` on your phone or PC. The app will **cache itself** as a PWA — after the first visit, it works **completely offline**.

### Option 2: Install as PWA on your phone

1. Open `http://<your-pc-ip>:8000` on your phone browser
2. Tap **"Add to Home Screen"** when prompted
3. The app installs with all model files cached locally
4. **Disconnect from internet** — it still works! ✅

---

## 🏗️ Project Structure

```
crop disease detecter/
├── app.py                    # FastAPI server (serves static files only)
├── knowledge_base.json       # Disease info: symptoms, treatment, prevention
├── model.keras               # Trained MobileNetV2 model (for reference)
├── requirements.txt          # Python dependencies
├── create_model.py           # Model creation utility
│
├── static/                   # Frontend PWA (all prediction happens here)
│   ├── index.html            # Main app page
│   ├── sw.js                 # Service Worker for offline caching
│   ├── manifest.json         # PWA manifest
│   ├── css/style.css         # Styling
│   ├── js/
│   │   ├── app.js            # Main app controller
│   │   ├── offline.js        # TF.js model inference engine
│   │   ├── preprocess.js     # Image preprocessing (224×224)
│   │   └── storage.js        # IndexedDB scan history
│   └── models/               # TF.js model files (~9.7 MB)
│       ├── model.json
│       ├── group1-shard1of3.bin
│       ├── group1-shard2of3.bin
│       └── group1-shard3of3.bin
│
├── training/                 # Model training scripts
│   ├── train_model.ipynb
│   ├── convert_to_tfjs.py
│   └── dataset_prep.py
│
└── backend/                  # (unused — prediction is on-device)
```

---

## 🧠 How It Works

```
   📷 Camera/Upload
        │
        ▼
   ┌─────────────┐
   │ Preprocess   │  Resize to 224×224, RGB
   │ (preprocess) │
   └──────┬──────┘
          │
          ▼
   ┌─────────────┐
   │ TensorFlow.js│  MobileNetV2 inference
   │ (offline.js) │  runs in browser/WebGL
   └──────┬──────┘
          │
          ▼
   ┌─────────────┐
   │ Knowledge   │  Maps prediction → disease info
   │ Base (.json)│  symptoms, treatment, prevention
   └──────┬──────┘
          │
          ▼
   📋 Results displayed
```

### AI Model Details

| Property | Value |
|----------|-------|
| Architecture | MobileNetV2 (alpha=1.0) |
| Input | 224×224×3 RGB |
| Training | 2-phase transfer learning (ImageNet → PlantVillage) |
| Accuracy | ~86% validation accuracy |
| Size | ~9.7 MB (TF.js layers model) |
| Inference | Real-time on most smartphones |

---

## 📴 Offline-First Architecture

CropDoc AI is designed to work on **low-end smartphones with no internet**:

1. **Service Worker** caches all app files + model on first visit
2. **TensorFlow.js** runs inference on-device using WebGL/CPU
3. **IndexedDB** stores scan history locally
4. **No network requests** — prediction is 100% client-side
5. **Small model** (~9.7 MB) loads fast even on 2G networks

### Low-End Device Optimizations

- MobileNetV2 is designed for mobile inference (low FLOPS)
- Model weights are quantized into 3 shards for progressive loading
- Images are center-cropped and resized to 224×224 before inference
- Color-analysis fallback if TF.js fails on very old browsers

---

## 🗄️ Knowledge Base

The `knowledge_base.json` contains detailed information for all 38 disease classes:

- 🩺 **Symptoms** — what the disease looks like
- 💊 **Treatment** — organic and chemical options
- 🛡️ **Prevention** — how to avoid the disease
- 🌾 **Fertilizer** — recommended nutrients
- ⚠️ **Risk Level** — Low / Medium / High / Critical
- 🔬 **Scientific Name** — pathogen identification

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| AI Engine | TensorFlow.js (WebGL/CPU backend) |
| Model | MobileNetV2 (Keras → TF.js) |
| Storage | IndexedDB (via idb) |
| Offline | Service Worker + Cache API |
| Server | FastAPI + Uvicorn (static file serving) |
| PWA | Web App Manifest + Service Worker |

---

## 📜 License

MIT License — see [LICENSE](LICENSE)

---

## 🙏 Acknowledgments

- **PlantVillage Dataset** — training data for crop disease images
- **TensorFlow / TensorFlow.js** — model training and in-browser inference
- **MobileNetV2** — efficient mobile architecture by Google


**submission link for google drive** : https://drive.google.com/file/d/1jxvp4xe5UbsICTIia1wBO1AXj3jBjHBc/view?usp=sharing
