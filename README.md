# 🛡️ SKY-GUARD GCS
### AI-Powered Autonomous Drone Detection, Tracking & Airspace Monitoring System

**An intelligent Ground Control Station for real-time aerial object monitoring, visual tracking, geofence surveillance, and forensic analysis.**

<p align="center">
  <strong>Computer Vision • YOLOv8 • OpenCV • PyQt5 • PaddleOCR • Local AI</strong>
</p>

---

## 🚀 Project Overview

SKY-GUARD is an AI-powered Ground Control Station (GCS) designed to monitor video feeds, detect and track aerial objects, identify restricted-zone violations, and maintain a persistent record of surveillance events.

Built using **Python, PyQt5, OpenCV, and Ultralytics YOLOv8**, the application combines computer vision, trajectory analysis, geofence monitoring, forensic image storage, and optional local AI services in a desktop command interface.

The system also includes an AI Surveillance Assistant, optional OCR-based text extraction, and anomaly detection to support operator investigation and situational awareness.

SKY-GUARD is designed as a modular, locally operated monitoring platform. Its analysis results are intended to assist human operators rather than make autonomous security decisions.

## ✨ Key Features

### 1. AI-Based Object Detection & Tracking
- YOLOv8-powered object detection.
- Multi-object tracking with persistent target IDs.
- Real-time tracking overlays and target information.
- Target locking and visual tracking controls.
- Auto-Zoom Picture-in-Picture (PiP) for the highest-confidence detection.

### 2. Video Surveillance & Enhanced Optics
- Webcam and local video file input.
- Night-vision-style phosphor-green display.
- Thermal-style color-map visualization.
- Live video monitoring and feed controls.
- On-screen FPS and tracking information.

### 3. Trajectory Analysis & Geofence Monitoring
- Visual motion trails for tracked objects.
- Configurable no-fly-zone polygon.
- Geofence entry and exit detection.
- Editable geofence with drawing, undo, finish, and cancel controls.
- Persistent custom geofence configuration.
- Trajectory feature extraction, including:
  - Track duration and number of points.
  - Path length and displacement.
  - Average pixel speed and speed variation.
  - Direction changes.
  - Geofence entry and exit counts.

**Note:** Trajectory speed and distance are measured in pixels and seconds. They are not calibrated real-world measurements.

### 4. Forensic Threat Gallery
- Automatic forensic snapshot capture.
- Snapshot metadata includes timestamp, target ID, object class, and confidence.
- Capture threshold configured at **60% detection confidence**.
- One initial snapshot per target and video source, subject to the capture logic.
- Persistent forensic records using SQLite.
- Historical snapshot browsing across application launches, provided image files remain available.

### 5. OCR-Based Text Extraction
- Optional PaddleOCR integration.
- Text extraction from selected forensic images.
- OCR results stored persistently in SQLite.
- Threat Gallery displays previously saved OCR results.
- Recognized text, confidence, timestamp, and processing status.
- OCR runs only when requested by the operator.

OCR provides text extraction; automatic drone registration verification or license-plate identification is not currently implemented.

### 6. Intelligent Event Logging & Anomaly Analysis
- Queue-based SQLite event writer.
- Structured surveillance event records.
- Persistent event history.
- Optional Isolation Forest anomaly detection.
- Unusual trajectory patterns flagged for operator review.
- Anomaly detection does not independently declare a target dangerous or criminal.

### 7. AI Surveillance Assistant
- Natural-language interface for surveillance data queries.
- Parameterized SQL queries for supported exact-data questions.
- Optional Retrieval-Augmented Generation (RAG).
- ChromaDB and Sentence Transformers for event retrieval.
- Local Ollama integration for contextual answers.
- Evidence-based responses using retrieved event records.
- Asynchronous assistant processing to keep the GUI responsive.

The RAG system indexes structured event records, not video frames.

### 8. Alerts & Operator Dashboard
- Real-time event log.
- Target acquisition alerts.
- Geofence breach and exit alerts.
- Swarm warning when three or more targets are active.
- Voice alerts using `pyttsx3` on a dedicated thread.
- Alert siren and visual warning indicators.
- CSV report export.

### 9. User Interface & Accessibility
- PyQt5 desktop command interface.
- Dark and light themes.
- Resizable command layout.
- Adjustable interface text size.
- Keyboard shortcuts.
- High-visibility keyboard focus.
- Dedicated shortcut guide.

### 10. Operator Login
- Operator login window.
- Basic login gate before accessing the application.

**Security note:** The current default credentials are development credentials. Change or replace the authentication implementation before deploying the application in a production environment.

---

## 🖥️ Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python |
| Desktop GUI | PyQt5 |
| Computer Vision | OpenCV |
| Object Detection | Ultralytics YOLOv8 |
| Object Tracking | Tracking integrated with the detection pipeline |
| OCR | PaddleOCR |
| Database | SQLite |
| Anomaly Detection | Scikit-learn Isolation Forest |
| Vector Database | ChromaDB |
| Embeddings | Sentence Transformers |
| Local Language Model | Ollama |
| Voice Alerts | pyttsx3 |
| Reporting | CSV |

---

## 📂 Project Structure

```text
SKYNET/
│
├── main.py
├── run_skyguard.py
├── run.bat
│
├── core/
│   ├── event_writer.py
│   └── ...
│
├── database/
│   ├── event_repository.py
│   └── ...
│
├── intelligence/
│   ├── OCR and AI integrations
│   └── ...
│
├── gui/
│   └── forensic_ocr.py
│
├── config/
│   └── settings.py
│
├── tests/
│
├── requirements.txt
├── requirements-intelligence.txt
├── requirements-rag.txt
├── requirements-ocr.txt
│
├── .env.example
├── IMPLEMENTATION_STATUS.md
└── README.md
```

Runtime directories such as `forensics/`, `data/`, and `logs/` are created or used by the application as needed. They are excluded from Git where configured.

---

## ⚙️ Installation & Setup

### Prerequisites

- Windows 10 or Windows 11.
- Python installation compatible with the selected dependencies.
- Git.
- Webcam or local video files for testing.
- Internet connection for initial package and model downloads.

### 1. Clone the Repository

```powershell
git clone https://github.com/hajirashariff/SKYNET.git
cd SKYNET
```

### 2. Create a Virtual Environment

For the OCR-enabled setup, use Python 3.12:

```powershell
py -3.12 -m venv .venv-ocr
```

Activate it:

```powershell
.\.venv-ocr\Scripts\Activate.ps1
```

Upgrade pip:

```powershell
python -m pip install --upgrade pip
```

### 3. Install Core Dependencies

```powershell
python -m pip install -r requirements.txt
```

### 4. Install Optional OCR Dependencies

```powershell
python -m pip install -r requirements-ocr.txt
```

PaddleOCR and PaddlePaddle require compatible Python, NumPy, and OpenCV versions. If installation fails, consult the troubleshooting section and use compatible package versions.

### 5. Configure Ollama

Install Ollama for Windows:

https://ollama.com/

Download the tested local model:

```powershell
ollama pull llama3.2:3b
```

Start the local service if it is not already running:

```powershell
ollama serve
```

In the project terminal, configure the model:

```powershell
$env:SKYGUARD_OLLAMA_MODEL = "llama3.2:3b"
```

The application uses the local Ollama endpoint by default:

```text
http://localhost:11434
```

No cloud API key is required for local Ollama inference.

### 6. Optional RAG and Anomaly Detection

Install the optional intelligence dependencies when needed:

```powershell
python -m pip install -r requirements-intelligence.txt
```

For RAG:

```powershell
python -m pip install -r requirements-rag.txt
```

**Environment compatibility:** The tested setup used Python 3.14 for the core/RAG environment and a separate Python 3.12 environment for the OCR-enabled application. ChromaDB and Sentence Transformers were kept out of the OCR environment to avoid dependency conflicts. Installing every optional dependency into one environment is not guaranteed to work.

---

## ▶️ Running SKY-GUARD

Activate the environment first.

For the OCR-enabled environment:

```powershell
cd C:\Users\Hajir\SKYNET
.\.venv-ocr\Scripts\Activate.ps1
```

Configure the Ollama model:

```powershell
$env:SKYGUARD_OLLAMA_MODEL = "llama3.2:3b"
```

Launch the application:

```powershell
python main.py
```

Alternatively, use:

```powershell
python run_skyguard.py
```

Or double-click `run.bat` if it is configured for your environment.

---

## 🎮 Keyboard Shortcuts

| Shortcut | Function |
|---|---|
| `Ctrl + Shift + P` | Pause or resume video feed |
| `Ctrl + Shift + T` | Toggle tracking |
| `Ctrl + Shift + D` | Switch dark/light theme |
| `Ctrl + =` | Increase interface text size |
| `Ctrl + -` | Decrease interface text size |
| `F11` | Toggle full screen |

The `A−` and `A+` controls also adjust interface text size. Use the **SHORTCUTS** button inside the application to view the shortcut guide.

---

## 🛑 Geofence Configuration

The default no-fly zone is defined using normalized video-frame coordinates.

To customize the default polygon, edit `NO_FLY_ZONE` in `main.py`.

To draw a custom fence:

1. Select **DRAW FENCE**.
2. Click at least three locations on the video.
3. Select **FINISH FENCE** to save the polygon.
4. Use **UNDO POINT** to remove the last point.
5. Toggle drawing off to cancel and restore the previous fence.

Geofence detection is paused during drawing. The saved polygon and its enabled/disabled state persist between launches.

---

## 🧪 Testing

Run the automated test suite:

```powershell
python -m unittest discover -s tests -v
```

The project has previously passed all 19 automated tests. Re-run the suite after making code changes to verify the current version.

Additional manual testing should include:

- Opening the application.
- Testing webcam and local video feeds.
- Checking object detection and tracking.
- Testing geofence entry and exit alerts.
- Confirming forensic snapshot creation.
- Selecting saved forensic images.
- Testing OCR extraction on images containing readable text.
- Verifying persistent OCR results after restarting the application.
- Checking SQLite event records.
- Testing Ollama connectivity and contextual queries when configured.

---

## 🔧 Troubleshooting

### Application does not start

Check that the correct virtual environment is active and install the core requirements:

```powershell
python -m pip install -r requirements.txt
```

### Ollama is unavailable

Start Ollama:

```powershell
ollama serve
```

Check that the model is installed:

```powershell
ollama list
```

Download the model if needed:

```powershell
ollama pull llama3.2:3b
```

### OCR is unavailable

Confirm that the OCR environment is active and that compatible PaddleOCR and PaddlePaddle packages are installed.

### RAG is unavailable

Install compatible ChromaDB and Sentence Transformers dependencies in the designated RAG environment. Exact SQL-based queries remain available without RAG.

### Anomaly detection is unavailable

Install the intelligence dependencies:

```powershell
python -m pip install -r requirements-intelligence.txt
```

The Isolation Forest model requires a minimum number of persisted trajectory records before training.

### Forensic images do not appear

Check:

- The detection confidence.
- The configured forensic capture threshold.
- The application write permissions.
- The `forensics/` directory and database.
- Whether the target and video source have already triggered a snapshot.

---

## ⚠️ Current Limitations

- Speed and trajectory measurements are pixel-based, not calibrated real-world measurements.
- The system has not been validated for reliable drone-versus-bird-versus-aircraft classification.
- Automatic license plate or drone registration identification is not implemented.
- ONNX and TensorRT deployment optimization has not been benchmarked.
- A 30+ FPS performance target has not been verified on edge hardware.
- Isolation Forest anomaly results are recommendations for operator review, not confirmed threats.
- Threat levels based on calibrated physical proximity and speed are not currently implemented.
- Ollama, RAG, anomaly detection, and OCR are optional integrations.
- Local video and webcam performance depends on the hardware and model configuration.

SKY-GUARD is a project prototype and should not be treated as a certified aviation safety or autonomous threat-detection system.

---

## 🔮 Future Enhancements

- Kalman filter and optical-flow-based motion estimation.
- Improved drone, bird, and aircraft motion classification.
- Calibrated physical velocity and distance estimation.
- OCR-based registration and identification workflows.
- ONNX and TensorRT inference optimization.
- NVIDIA Jetson edge deployment.
- FPS and inference-latency benchmarking.
- Explainable Low, Medium, and High threat-level classification.
- Expanded operator alert and reporting capabilities.

---

## 👨‍💻 Project

**SKY-GUARD — AI-Powered Autonomous Drone Detection, Tracking & Threat Monitoring Ground Control Station**

Developed as an engineering project using Python, computer vision, deep learning, and local AI technologies.

**Repository:** https://github.com/hajirashariff/SKYNET

---

*Building smarter, more responsive airspace monitoring through AI and computer vision.*
