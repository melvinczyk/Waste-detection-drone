# Tello Waste Detection Drone

Real-time trash detection on a [DJI Tello](https://www.ryzerobotics.com/de/tello) drone using a YOLOv5 model to run against the drone's live video feed. Any high-confidence detection gets logged with a timestamped screen capture.

<p align="center">
  <img src="images/Our%20model%20frame.png" width="400" alt="Personal-Trash-Data model frame">
  <img src="images/Medium%20model%20frame.png" width="400" alt="Medium-Trash-Data model frame">
</p>

## How it works

`main.py` connects to the Tello, checks its battery, and runs two processes in parallel:

- **`controls.py`** — opens a small pygame window that captures keyboard input and translates it into `djitellopy` drone commands.
- The detection loop (in `main.py`), pulls each frame from the drone's camera, runs it through YOLOv5, and for any detection above 90% confidence, hands it off to `logger.py` and `imageCapture.py` to record.

## Controls

Driven by keyboard input through the pygame window:

| Key                   | Action                               |
| --------------------- | ------------------------------------ |
| `W` / `A` / `S` / `D` | Forward / left / back / right        |
| `↑` / `↓`             | Up / down                            |
| `←` / `→`             | Rotate counter-clockwise / clockwise |
| `E`                   | Takeoff                              |
| `Q`                   | Land                                 |

## Models

Two YOLOv5s models were trained on two different datasets, both sourced from Roboflow Universe:

|         | Personal-Trash-Data                                                                    | Medium-Trash-Data                                                                                                             |
| ------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Images  | 42 (32 train / 6 valid / 4 test)                                                       | 882 (600 train / 240 valid / 42 test)                                                                                         |
| Classes | 6 — can, cardboard, cigarette, paper, plastic, trash                                   | 1 — waste                                                                                                                     |
| Source  | [Roboflow: cs-420/personal-trash](https://universe.roboflow.com/cs-420/personal-trash) | [Roboflow: trash-detection-1fjjc](https://universe.roboflow.com/trash-dataset-for-oriented-bounded-box/trash-detection-1fjjc) |
| License | CC BY 4.0                                                                              | CC BY 4.0                                                                                                                     |

<p align="center">
  <img src="images/ours.gif" width="400" alt="Personal-Trash-Data model detecting in real time">
  <img src="images/model_.gif" width="400" alt="Medium-Trash-Data model detecting in real time">
</p>
<p align="center"><i>Left: Personal-Trash-Data model. Right: Medium-Trash-Data model.</i></p>

The two models trade off differently: Personal-Trash-Data is more finely labeled (6 waste categories) but trained on only 42 images, while Medium-Trash-Data collapses everything into a single "waste" class but has 21x more training data. Medium-Trash-Data model obviously detects more reliably, but Personal-Trash-Data model tells you more specifically what kind of waste it found.

## Logging

Every run creates a new `Logs/runX` folder (`X` auto-incrementing from 0). Each run folder contains:

- **`runX.csv`** — confidence and timestamp for every detection above 90% confidence.
- **`images/`** — the corresponding captured frame for each logged detection, named by timestamp, so a row in the CSV and an image in this folder always refer to the same detection event.

## Setup

### 1. Clone YOLOv5

Detection is loaded locally via `torch.hub`, so the official YOLOv5 repo needs to live in a `yolov5/` folder at the project root:

```bash
git clone https://github.com/ultralytics/yolov5.git
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Train weights

Place them at the paths `main.py` expects.

### 4. Connect to your Tello

Power the drone on and connect your computer to its Wi-Fi network before running the script.

### 5. Run

```bash
python main.py
```

A pygame window will open to capture keyboard input, and a second window will show the live detection feed.
