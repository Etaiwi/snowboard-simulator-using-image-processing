
# Snowboard Simulator Using Real-Time Image Processing
### Vision-Based Human Motion Interface for Interactive Gaming

---

## Overview

This project implements a real-time computer vision system that transforms physical body movements into game controls for the PC game *Snowboard Party*.

Using classical image processing and motion analysis techniques, the system captures a live webcam stream, extracts player posture and motion features, and translates them into simulated keyboard inputs.

The result is a low-latency, vision-based control interface that enables:

- Lean-based turning
- Jump and squat detection
- Wide-stance braking (“Slow”)
- Forward acceleration (“Fast”)
- Mid-air grab detection
- Combo potential via chained actions

The system was designed for robustness, responsiveness, and real-time performance under varying environmental conditions.

---

## Key Features

- Real-time body segmentation via background subtraction
- Binary mask refinement with Gaussian + Median filtering
- Center-of-mass tracking for posture analysis
- Lean detection using upper/lower body mass separation
- Wide-stance detection via Canny + Hough Transform
- HSV-based color tracking for glove detection
- Noise robustness analysis (Salt & Pepper, Gaussian)
- Adjustable thresholds and runtime calibration menus
- Multi-threaded architecture for smooth gameplay

---

## System Architecture

The system operates in three major stages:

### 1️) Frame Acquisition

- Webcam positioned ~2 meters from player
- Continuous frame sampling using threaded camera module
- Buffered frame retrieval for low IO latency

---

### 2️) Image Processing Pipeline

**A. Background Subtraction**
- Absolute frame difference
- Grayscale conversion
- Gaussian filtering
- Median filtering
- Thresholding
- Region of Interest (ROI) refinement

**B. Feature Extraction**
- Center of mass computation
- Upper/lower body separation
- Lean angle estimation
- Vertical velocity estimation for jump detection
- Canny edge detection
- Hough transform for leg-line detection
- HSV color segmentation for glove tracking
- Contour detection for accessory validation

---

### 3️) Control Interface

A dedicated controller module simulates keyboard events using:

- Non-blocking threaded key press handling
- Duration-controlled key simulation
- Continuous input buffering

This ensures precise in-game control without interrupting image processing tasks.

---

## Motion Detection Logic

### Lean (Left / Right)
- Compute upper and lower center of mass
- If angular tilt exceeds threshold → trigger turn

### Squat / Jump
- Track vertical displacement over time
- Detect squat compression and upward release

### Slow (Wide Stance)
- Apply Canny on lower binary mask
- Detect leg lines using Hough transform
- Validate intersection within angle constraints:
  - Right leg: 20–40°
  - Left leg: 140–160°

### Fast (Acceleration)
- Upper body tilt forward
- Hands extended forward at similar height

### Grab
- HSV color mask for red/green gloves
- Contour validation within ROI

---

## Robustness & Noise Analysis

The lean detection algorithm was evaluated under:

- Salt & Pepper noise (1%, 2%, 3%)
- Gaussian noise (4/255, 12/255, 16/255 amplitudes)

Performance was measured using:

- Confusion matrices
- F1 scores

Results show:

- Gradual degradation under increasing noise
- Higher robustness to Salt & Pepper noise than Gaussian
- Stronger right-lean detection compared to left-lean under heavy noise

---

## Runtime Adjustability

The system includes interactive tuning menus:

- `p` – Pause / Enter edit mode
- `c` – Adjust HSV color ranges
- `b` – Adjust motion thresholds
- `L` – Rescan background
- `r` – Resume game

These features allow dynamic adaptation to lighting, user differences, and environmental changes without restarting the program.

---

## Real-Time Optimization

- Multi-threaded camera streaming
- Multi-threaded key simulation
- Lightweight classical CV algorithms
- ROI restriction to minimize computation
- Threshold tuning to balance responsiveness and stability

The system prioritizes low-latency inference to maintain smooth gameplay.

---

## Tech Stack

- Python
- OpenCV
- NumPy
- Multi-threading
- Classical Image Processing
- Canny Edge Detection
- Hough Transform
- HSV Color Space Analysis

---

## Limitations

- Assumes mostly static background
- Requires full-body visibility within frame
- Accessory-based glove detection (color-dependent)
- Performance decreases under extreme lighting or heavy noise

---

## Why This Project Is Interesting

- Demonstrates full end-to-end real-time vision pipeline
- Integrates segmentation, geometric detection, and motion inference
- Balances accuracy with computational efficiency
- Implements threaded real-time architecture
- Includes robustness evaluation under synthetic noise
- Bridges classical computer vision with interactive systems

This project highlights strong foundations in image processing, system design, real-time constraints, and debugging under dynamic conditions.

---

## Future Improvements

- Pose-estimation based tracking (MediaPipe / OpenPose)
- ML-based motion classification
- Background-independent segmentation
- Adaptive noise filtering
- Cross-platform deployment
