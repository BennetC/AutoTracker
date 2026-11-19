# AutoTracker

An experimental vehicle-sensing and machine-learning project developed by [BennetC](https://github.com/BennetC). AutoTracker collects accelerometer and gyroscope measurements from an ESP32 and MPU-6500, processes the sensor data in Python, and explores classification of driving environments such as city streets, country roads, and motorways.

## How it works

1. ESP32 firmware reads the IMU and streams measurements over serial.
2. A Python collector calibrates the sensor and records labeled measurements.
3. Processing code explores sensor alignment, filtering, and feature extraction.
4. A random forest classifier evaluates road categories, including a leave-one-recording-out experiment.

The repository also contains a Flask dashboard prototype. Its charts currently use generated data; it is not connected to the sensor pipeline.

## Project structure

- `src/` — ESP32 firmware and Python collection, calibration, filters, analysis, and classification code.
- `src/Website/` — Flask dashboard prototype.
- `data/` — calibration metadata and historical output; the recording CSVs needed for model experiments are not included.
- `platformio.ini` and `requirements.txt` — embedded and Python dependency declarations.

## Getting started

This is a research prototype, not a polished, reproducible release. Hardware, dependency versions, data collection, and model evaluation need further verification.

### Python

Create a virtual environment and install the Python dependencies from the repository root:

```sh
python -m venv .venv
# Activate .venv using the command for your shell, then:
python -m pip install -r requirements.txt
```

To explore the dashboard prototype:

```sh
python src/Website/app.py
```

For hardware collection, configure the serial port and baud rate in `src/py/configs/config.py` (the checked-in default is `COM3` at 115200 baud). The collector entry point is `python -m src.py.record_data.data_collector`. It requires a compatible connected device and calibration; review its recording paths and controls before use.

The classifier entry point is `python -m src.py.ml_model.model`. Its experiment expects locally collected CSV files and matching calibration metadata under `data/raw/`; these recordings are not included, so results cannot be reproduced from this checkout alone.

### Firmware

The PlatformIO configuration targets `esp32dev` with the Arduino framework. The firmware uses FastIMU as well as ArduinoEigen; FastIMU is not currently declared in `platformio.ini`. Multiple firmware source files are present and the build and upload process has not been verified. Review the intended firmware entry point and dependencies before building or flashing.

## Status and provenance

This repository preserves the project's original Git history, including commits attributed to the original host. It was previously hosted at [EnnioMrk/AutoTracker](https://github.com/EnnioMrk/AutoTracker). Model performance and hardware behavior have not been independently verified in this portfolio version.

The existing [MIT license](LICENSE) and copyright notice are retained.
