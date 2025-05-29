# Zenitsu V1: Drone VLM Integration with Real-Time GUI

**Zenitsu V1** is a Python-based application designed for integrating a Vision-Language Model (VLM) with a drone's camera feed, providing real-time scene description and system monitoring. The script offers a sleek graphical user interface (GUI) built with PyQt6, displaying the camera feed, VLM responses, and system usage metrics (CPU, RAM, GPU). It also includes a fallback OpenCV-based GUI for systems without PyQt6 support. The system is designed for drone applications, leveraging a VLM server (e.g., SmolVLM) and a Flask-based system monitoring server.

## Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Directory Structure](#directory-structure)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Features
- **Real-Time Camera Feed**: Captures and displays video from a webcam or drone camera at ~30 FPS.
- **VLM Integration**: Processes camera frames using a Vision-Language Model (e.g., SmolVLM-500M) to generate scene descriptions.
- **System Monitoring**: Displays CPU, RAM, and GPU usage via a Flask-based server, with dynamic progress bars.
- **Interactive GUI**: PyQt6-based interface with hover effects, animations, and customizable inference speeds.
- **Fallback GUI**: OpenCV-based interface for systems lacking PyQt6, with on-screen text overlays.
- **Configurable Inference Speeds**: Choose from six inference intervals (100ms to 2000ms) for VLM processing.
- **Robust Error Handling**: Includes logging, webcam availability checks, and HTTP retry mechanisms.
- **Cross-Platform Support**: Runs on Linux (with GPU support via `nvidia-smi`) and other systems with CPU/RAM monitoring.

## Prerequisites
Before running Zenitsu V1, ensure the following requirements are met:

### Software
- **Python**: Version 3.8 or higher
- **Operating System**: Linux (recommended for GPU support), Windows, or macOS
- **Webcam**: A connected webcam or drone camera (default device index: 0)
- **XTerm**: Required for launching VLM and Flask servers in separate terminals (Linux only)
- **NVIDIA GPU** (optional): For GPU usage monitoring on Linux (requires `nvidia-smi`)

### Python Packages
Install the required Python packages using the provided `requirements.txt` or manually:
