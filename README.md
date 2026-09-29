# Face Recognition System (OpenCV + LBPH)

A Python project that captures a labeled face dataset from your webcam, trains
an LBPH (Local Binary Patterns Histogram) face recognizer on it, and then
recognizes faces in real time from a live webcam feed — greeting each
recognized person by name.

Built with OpenCV's Haar Cascade classifier for face detection and
`cv2.face.LBPHFaceRecognizer` for recognition. Works as a standalone script
 inside a Jupyter Notebook.

 ---

## Features

- **Dataset capture** — collects cropped, grayscale face images from your
  webcam and organizes them into folders by person name.
- **Model training** — trains an LBPH recognizer on the captured dataset and
  saves the trained model to disk so it persists between runs.
- **Live recognition** — detects and recognizes faces in a live webcam feed,
  drawing a bounding box, the predicted name, and a confidence score, and
  prints a one-time `Hello <name>!` greeting per session.
- **Edge-case handling** — skips frames with zero or multiple faces during
  capture, warns on insufficient training samples, fails fast with clear
  error messages if the webcam or model files are missing, and safely
  releases the camera even if interrupted mid-run.

  ## Project structure

```
face_recognition/
│
├── face_recognition_app.py   # (or your .ipynb) - main script/notebook
├── dataset/                  # created automatically
│   ├── Alice/
│   │   ├── 1.jpg
│   │   ├── 2.jpg
│   │   └── ...
│   └── Bob/
│       └── ...
├── trained_model.yml         # saved LBPH model (created after training)
├── labels.pickle             # maps numeric labels back to names
└── README.md
```

---
## Requirements

- Python 3.8+
- A working webcam
- Dependencies:

```bash
pip install opencv-contrib-python numpy
```
> **Important:**
>  this project requires `opencv-contrib-python`, not plain
> `opencv-python`. The `cv2.face` module (used for `LBPHFaceRecognizer`)
> only ships with the contrib build. If both are installed at once, they can
> conflict — uninstall both and reinstall just the contrib version if you
> run into an `ImportError` on `cv2.face`:
>
> ```bash
> pip uninstall opencv-python opencv-python-headless opencv-contrib-python
> pip install opencv-contrib-python
> ```
> Then restart your Python kernel/terminal so the new install is picked up.

---

**Workflow:**
 **1. Capture** | Opens the webcam, detects a single face per frame with a Haar Cascade, crops and resizes it to 200×200 grayscale, and saves it to `dataset/<name>/`. 
 
| **2. Train** | Reads every image under `dataset/`, assigns each person folder a numeric label, trains `cv2.face.LBPHFaceRecognizer_create()` on the images, and saves the model (`trained_model.yml`) plus a label→name map (`labels.pickle`). |

| **3. Recognize** | Loads the saved model and label map, runs live face detection on the webcam feed, and for each detected face calls `recognizer.predict()`. If the returned confidence is below a threshold, it's labeled with the matched name; otherwise it's labeled "Unknown". |

---


  
