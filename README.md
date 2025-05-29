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
```
numpy
opencv-python
requests
psutil
PyQt6
pillow
flask
flask-cors
```
To install:
```bash
pip install -r requirements.txt
```

### VLM Server
- **SmolVLM-500M-Instruct-GGUF** (or compatible model): Must be set up with `llama.cpp` server.
- Ensure the VLM server is configured at `~/Drone/YOLO+LLM/llama.cpp/build` with the command:
  ```bash
  ./bin/llama-server -hf ggml-org/SmolVLM-500M-Instruct-GGUF -ngl 99
  ```
- Default endpoint: `http://localhost:8080/v1/chat/completions`

## Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/zenitsu-v1.git
   cd zenitsu-v1
   ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set Up VLM Server**:
   - Download and configure the SmolVLM model in the `llama.cpp` build directory.
   - Ensure the server is accessible at the default endpoint or update the `--vlm-endpoint` argument.

4. **Verify Webcam**:
   - Connect a webcam or drone camera and confirm it is accessible (default device: `/dev/video0` on Linux).

## Usage
Run the script with default settings:
```bash
python justvlm.py
```
This will:
- Start the VLM server in a new `xterm` window.
- Launch the Flask system monitoring server in another `xterm` window.
- Open the PyQt6 GUI displaying the camera feed, VLM responses, and system usage.

### Command-Line Arguments
Customize the script's behavior with the following arguments:
```bash
python justvlm.py --vlm-endpoint <endpoint> --fallback --camera-device <index> --instruction <text> --inference-interval <ms>
```
- `--vlm-endpoint`: VLM server URL (default: `http://localhost:8080/v1/chat/completions`)
- `--fallback`: Use OpenCV-based GUI instead of PyQt6
- `--camera-device`: Camera device index (default: 0)
- `--instruction`: VLM prompt (default: "Describe the scene")
- `--inference-interval`: VLM inference interval in milliseconds (choices: 100, 250, 500, 1000, 1500, 2000; default: 500)

### GUI Controls
- **Inference Speed**: Select from dropdown (Flash, Cheetah, Ideal, Normal, Lethargic, Sloth) to adjust VLM processing frequency.
- **Start/Stop Button**: Toggle processing of camera feed and VLM inference.
- **Camera Feed**: Displays real-time video.
- **VLM Response**: Shows scene descriptions with fade-in animations.
- **System Usage**: Visualizes CPU, RAM, and GPU usage with gradient progress bars.

### Fallback GUI
If `--fallback` is used, the OpenCV GUI displays:
- Camera feed with text overlays for state, inference speed, VLM response, and system usage.
- Press `s` to toggle processing; press `q` to quit.

## Configuration
### VLM Server
Modify the `start_vlm_server` function in `justvlm.py` to change the VLM model or endpoint:
```python
vlm_command = "cd <your-vlm-directory> && ./bin/llama-server -hf <model-name> -ngl 99"
```

### Flask Server
The Flask server runs at `http://localhost:5000/system-usage`. Adjust the port or host in the `FLASK_SCRIPT` string if needed:
```python
app.run(debug=True, host='0.0.0.0', port=5000)
```

### Camera Settings
Adjust camera resolution or FPS in the `CameraFeed` class:
```python
def __init__(self, device=0, resolution=(240, 180)):
    self.cap.set(cv2.CAP_PROP_FPS, 30)
```

## Directory Structure
```
zenitsu-v1/
├── justvlm.py           # Main script
├── requirements.txt     # Python dependencies
├── system_monitor.py    # Generated Flask script (auto-created at runtime)
├── README.md            # This file
└── LICENSE              # License file (add your preferred license)
```

## Troubleshooting
- **Webcam Not Found**:
  - Ensure the camera is connected and accessible (`ls /dev/video*` on Linux).
  - Try a different `--camera-device` index.
- **VLM Server Fails to Start**:
  - Verify the `llama.cpp` build path and model file exist.
  - Check if port 8080 is free (`netstat -tuln | grep 8080`).
- **Flask Server Errors**:
  - Ensure port 5000 is available.
  - Check if `flask` and `flask-cors` are installed (`pip show flask flask-cors`).
- **PyQt6 GUI Fails**:
  - Install PyQt6 (`pip install PyQt6`).
  - Use `--fallback` for the OpenCV GUI as an alternative.
- **Logs**:
  - Check logs in the console or `justvlm.log` (if redirected) for detailed error messages.

## Contributing
Contributions are welcome! To contribute:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.

Please include tests and update documentation as needed.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---
**Note**: This project is intended for research and development purposes. Ensure compliance with local regulations when using drones or webcams.
